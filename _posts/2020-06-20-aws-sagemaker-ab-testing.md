---
published: true
layout: post
title: A/B Testing Machine Learning Models in Production Using Amazon SageMaker (Discussion)
---

[Kieran Kavanaugh](https://www.linkedin.com/in/kierankavanagh/), [David Nigenda](https://www.linkedin.com/in/david-nigenda-49aab264/), and I, recently wrote a post for the [AWS Machine Learning Blog](https://aws.amazon.com/blogs/machine-learning/) about [A/B Testing ML models in production using Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/a-b-testing-ml-models-in-production-using-amazon-sagemaker/). I recommend reading the post, and also checking out our accompanying Jupyter notebook ([A/B Testing with Amazon SageMaker](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_endpoints/a_b_testing)).

In this post, I wanted to add context to our AWS blog post, by sharing a high level design diagram of a potential real-time inference production machine learning workflow.

**A Potential Real-time Inference ML Workflow**

![]({{site.baseurl}}/images/aws-sagemaker-a-b-testing/sample-real-time-inference-workflow.png)

Notice that the ML inference service has multiple models that can be used to service an inference request to its endpoint. The question then is, what logic does the service use, to route an inference request to a specific ML model.

In the [Amazon SageMaker](https://aws.amazon.com/sagemaker/) context, a [ProductionVariant](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html) “identifies a model that you want to host and the resources to deploy for hosting it” (straight from the docs!). In our post, we discuss how with SageMaker endpoints hosting multiple ProductionVariants, users can (1) specify how traffic is distributed between ProductionVariants and their corresponding models using a weighted random approach, or override this default traffic distribution behavior and (2) explicitly specify which ProductionVariant and corresponding model should service an inference request.

This flexibility opens up the possibility of A/B testing a new ML model with production traffic in various ways, thereby adding an effective final step in the validation process for a new model.

[AWS Developer Guide: Test models in production](https://docs.aws.amazon.com/sagemaker/latest/dg/model-ab-testing.html).

Also published on towards data science ([Medium blog](https://towardsdatascience.com/a-b-testing-machine-learning-models-in-production-using-amazon-sagemaker-discussion-ee8953043397), [Twitter post](https://twitter.com/TDataScience/status/1272353383576453122), [LinkedIn post](https://www.linkedin.com/posts/towards-data-science_ab-testing-machine-learning-models-in-production-activity-6678117314975600640-uTSa/)).
