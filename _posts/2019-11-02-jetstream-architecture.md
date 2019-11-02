---
published: true
layout: post
title: Building a Production-Level ETL Pipeline Platform Using Apache Airflow
---

The CernerWorks Enterprise System Management team is responsible for mining systems data from Cerner clients’ systems, providing visibility to the collected data for various teams within Cerner, and building monitoring solutions using the collected data. Our primary mission is to help increase the reliability and security of Cerner clients’ systems. About three years ago, our team was at a place where we had developed an effective telemetry framework for systems data collection. At the same time, we were seeing an exponential increase in use-cases where we had to transform the collected systems data in various ways to support our visibility and monitoring efforts. We thereby felt a pressing need to introduce a dedicated ETL pipeline platform to our data architecture. [Link to this post on towards data science.](https://towardsdatascience.com/building-a-production-level-etl-pipeline-platform-using-apache-airflow-a4cf34203fbd?source=friends_link&sk=0f30754578cd9ed92b8ec6dd42c0d8aa) 

After some research, we found that the Apache Airflow open source framework would be a good fit for our requirements as it was designed to implement, schedule and monitor data workflows. In the Airflow context, a data workflow is represented by a DAG (Directed Acyclic Graph) which is a set of tasks with acyclic dependencies as shown below.  

![]({{site.baseurl}}/images/jetstream_architecture/example_dag_in_airflow.PNG)


Each task in a DAG is implemented using an Operator.  Airflow’s open source codebase provides a set of general operators, however, the framework’s primary appeal to us, was that we could implement custom operators uniquely suited for Cerner’s data workflows. Beyond being able to write custom operators, Airflow as a framework is designed to be heavily customizable.  

Our team worked on customizations motivated by requirements along two fronts (i) minimizing the development overhead for scheduling Cerner data workflows on our platform, and (ii) adopting a robust and reliable production architecture. We named our customized Airflow implementation Jetstream.  

**Jetstream’s design pattern for efficiently supporting Cerner’s data workflows.** 

![]({{site.baseurl}}/images/jetstream_architecture/jetstream_data_workflow.PNG)

The meat of the logic for implementing a given data workflow in Jetstream is within its operators and modules. The modules have logic that might be useful across a wide range of data workflows. For instance, connecting to various databases, connecting to various external and internal APIs, scheduling logic, batching logic, and so on. The operators are a generalized representation for a class of tasks. They load configuration data from the task YAML and utilize the modules to implement a given task. For instance, the DatabaseTransferOperator, allows consumers to transform and move data from one database to another. The task YAML captures the specialized and essential business logic associated with a given task.  

Our team’s systems data visibility and monitoring efforts involve partnerships with a wide variety of teams across Cerner. Many of our requirements for transforming systems data come from these teams.  By abstracting away, the meat of the involved logic, we were able to empower these teams to be consumers of our ETL platform with minimal development overhead. This is why the platform has continued to grow significantly over the last three years, and currently has more than 3000 daily tasks. A couple of example task YAMLs are included below, 
  
![]({{site.baseurl}}/images/jetstream_architecture/example_yamls.PNG)

Particularly since we introduced Jetstream, very early in Airflow's open source journey, developing a robust and reliable production architecture has been an iterative process. We will now discuss the design motivations for each significant evolution of Jetstream’s architecture. 

 

**Jetstream Architecture: Improving Scheduling Metadata Reliability and Using Pools** 

![]({{site.baseurl}}/images/jetstream_architecture/jetstream_architecture_iteration_0.PNG)

The picture above captures two significant architecture changes that we made to our initial vanilla proof of concept setup.  

(1) Using our Vertica data warehouse to store important task scheduling metadata. 

 Having reliable and custom scheduling metadata was an important requirement for our team.   We particularly wanted to remove our dependency on Airflow’s native options for storing scheduling metadata. As early adopters of Airflow, we wanted to hedge against significant airflow design changes that might have affected any native scheduling metadata logic that we leveraged. Also, since Cerner has clients all over the world, many of our tasks, rely heavily on logic built on top of our scheduling metadata to process correctly. For instance, Airflow adopted the UTC time standard (AIRFLOW-289) in its 1.9 release which would have broken our logic if we had a hard dependency on Airflow’s native scheduling options.  

The other important consideration was the sheer value of the scheduling metadata. This metadata essentially captures the time intervals for which the data associated with each task was processed. Say our scheduling metadata got corrupted, this could lead to data gaps (missing time intervals) or duplicate data (reprocessed time intervals). Having such critical data on our Jetstream nodes would introduce a high-cost single point of failure. Vertica has been a central component of our team’s data architecture (see [Cerner Advances Big Data Analytics Capabilities. Dan Woicke. Director, Enterprise System Management](https://hp.cioreview.com/cxoinsight/cerner-advances-big-data-analytic-capabilities-nid-11200-cid-59.html)). So, our Vertica cluster is closely monitored and thereby has high reliability. By storing this metadata on Vertica, even if our Jetstream nodes go down, we can fire them back up and start our jobs without worrying about corrupt, inconsistent or missing scheduling metadata. So, adding this Vertica dependency, allowed us to significantly increase the robustness and reliability of our platform.  Having this data in Vertica also lets us build SLA based alerting logic and develop performance reporting.  

(2) Leveraging Airflow Pools.  

We initially didn’t leverage the execution pools support provided by the Airflow framework. When a DAG’s (Directed Acyclic Graph) task was ready to be scheduled it would be added to Airflow’s default execution pool, to be picked up for execution by the next available executor.  The problem was when we added a DAG with more than a thousand tasks, this DAG’s tasks crowded out the tasks from the other DAGs. We then started to use named execution pools which would limit the number of worker slots available to each DAG. This let us develop a granular control on execution parallelism and prevent DAG’s from being unintentionally crowded out.    

**Jetstream Architecture: Setting Up a Jetstream Cluster and Adding Support for Near Real Time Scheduling** 

[[https://github.cerner.com/Olympus/jetstream/blob/WIKI-IMAGES-BRANCH/jetstream_readme_images/jetstream_architecture.png|alt=octocat]] 

![]({{site.baseurl}}/images/jetstream_architecture/jetstream_architecture_iteration.PNG)

The picture above represents the current production architecture of Jetstream.  

(3) Setting up a Jetstream Cluster.   

We used Airflow’s support for leveraging Celery to setup a production cluster. This let us leverage the benefits of concurrent processing and thereby boost the platform’s robustness and efficiency.  Each DAG is created with an associated Celery queue.  Each worker node is assigned a set of Celery queues. This means that the tasks from each DAG can be executed on worker nodes ‘listening’ for the corresponding DAG’s queue. We make sure that every queue has two or more assigned worker nodes to boost the reliability of the platform as we don’t want to have worker nodes that are potential single points of failure.  However, notice that the master node is a single point of failure.  See ‘Making Apache Airflow Highly Available' for a potential solution to this problem. Addressing this hasn’t been a design priority yet. 

(4) Support for Near Real Time Scheduling.  

We recently had important use cases to do near real time scheduling on our platform. We realized that we needed to store scheduling metadata in-memory in order to support near real time data workflows. So, for our near real time DAGs, we use Redis instead of Vertica for storing scheduling metadata.  

For additional reading, see [A Definitive Compilation of Apache Airflow Resources.](https://towardsdatascience.com/a-definitive-compilation-of-apache-airflow-resources-82bc4980c154)  

 

 

 

 