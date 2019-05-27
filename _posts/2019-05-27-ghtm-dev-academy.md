---
published: true
layout: post
title: GitHub Time Machine (Cerner DevAcademy Project)
---
This is a summary of the work that I did in Cerner's DevAcademy over the course of two months. DevAcademy is a part of Cerner's onboarding process where freshly hired software engineers work on low priority internal projects with the objective of learning good development practices and Cerner's dev. culture. DevAcademy is pretty rigorous and unofficially serves as the final validation step in Cerner's hiring process. For more information on DevAcademy, check out the brilliant [Michelle Brush's](https://www.linkedin.com/in/michellebrush/) [post on Cerner's engineering blog](https://engineering.cerner.com/2013/08/devacademy/).

**GitHub Time Machine Presentation**

![]({{site.baseurl}}/images/ghtm_images/ghtm_features_and_analysis.PNG)

Notes:
  1. GitHub only supports search on the default branch.
  1. [CppCMS](http://cppcms.com/wikipp/en/page/main) is a C++ web framework.
  1. The project had been active for about 19 months before I started working on it.
  1. The outdated maintenance docs caused a three day delay in starting actual development work. It took three days of substantial effort from senior mentors and previous team members to resolve the setup issues.   

***

![]({{site.baseurl}}/images/ghtm_images/ghtm_key_contributions.PNG)

Notes:
  1. Placed an aggressive emphasis on process learning and learning to be hyper-adaptable
  1. Approach informed by initial codebase analysis

***

![]({{site.baseurl}}/images/ghtm_images/ghtm_elasticsearch_integration.PNG)

Notes:
  1. Class design so that indexing logic is agnostic of the search implementation.
  1. Reasons for choosing Elasticsearch in this project:
    1. ES provides per segment cache's whereas Solr provides global caches. This makes ES much better suited for dynamically changing data.
    2. ES makes it easier to use distributed processing techniques. ES supports auto-scaling to a much greater extent than Solr does.
    3. ES is much easier to setup and scale. ES has a much lower overhead for deployment and maintenance. This is a particularly useful feature for a DevAcademy project.
    4. Helpful Links:
      1. [Solr vs. Elasticsearch: Who’s The Leading Open Source Search Engine?](https://logz.io/blog/solr-vs-elasticsearch/)
      1. [Apache Solr vs Elasticsearch: The Feature Smackdown](http://solr-vs-elasticsearch.com/)  
      1. [Solr vs. Elasticsearch in the Age of Insight Engines](https://www.searchtechnologies.com/blog/solr-elasticsearch-cognitive-search)
      1. [Top 15 Solr vs. Elasticsearch Differences](https://sematext.com/blog/solr-vs-elasticsearch-differences/)
