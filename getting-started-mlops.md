## Getting Started with ML Ops

In the next few modules you will look at how AI/ML workload on OpenShift (OCP) can be integrated with Red Hat Middleware portfolio, focusing on MLOps, which enables data science and IT teams to collaborate and increase the pace of model development and deployment.

This workshop is for data scientists and ML engineers who want to apply DevOps Principles to MLOps. 

## Automating The End-To-End Lifecycle Of Machine Learning Applications

In reality, that intelligent application isn’t just one small deployed thing. It is an entire distributed system in and of itself with many moving parts that are all tied together in various ways. A data scientist’s model is just one small part of this large distributed system, but we’re going to focus on it for this workshop

As shown in the following diagram, only a small fraction of a real-world ML system is composed of the ML code. The required surrounding elements are vast and complex.

![mlops_elements]({% image_path mlops-continuous-delivery-and-automation-pipelines-in-machine-learning-elements.png %}) 

## What Is MLOps

From [Wikipedia](https://en.wikipedia.org/wiki/MLOps):

>MLOps (a compound of “machine learning” and “operations”) is a practice for collaboration and communication between data scientists and operations professionals to help manage production ML (or deep learning) lifecycle. Similar to the DevOps or DataOps approaches, MLOps looks to increase automation and improve the quality of production ML while also focusing on business and regulatory requirements. 

## Differences Between DevOps And MLOps

 Unlike DevOps, MLOps is much more experimental in nature. Data scientists try different features, parameters and models. With all these changes, they must manage their code base and datasets, to create reproducible results.

Unlike application developments, MLOps needs to:

* Data/model versioning != code versioning: Model reuse has an entirely different meaning compared to software reuse, as models need tuning based on scenarios and data.

* Model monitoring. Models statistics needs to be monitored to ensure is performing within limits, so that it can be retrained when necessary (retraining needs to be on-demand) 

* Testing. In addition to unit tests, models need to be validated for model quality, such as accuracy, AUC, ROC, confusion matrix, precision, recall, etc. 

As such:

* Continuous integration (CI) for MLOps also involves validating the data and the schema in addition to testing code.

* Continuous deployment(CD) validating the performance of models in production – including the ability to deploy new models and rollback changes from a model.

* Continuous testing (CT) to retrain and serve models. CT is unique to ML systems that is for 

## Data Science Steps For MLOps

![cd4ml-end-to-end]({% image_path cd4ml-end-to-end.png %}) 
Image Source: [CD4ML](https://martinfowler.com/articles/cd4ml.html#TestingAndQualityInMachineLearning)



### Model Building
Once the data has been cleaned and is made available, we will start the iterative approach for model building. 

#### Feature Engineering, Model Evaluation and Experimentation

As the ML process is very experimental in nature and you may have multiple experiments running in parallel, it is important to capture key model metrics. This helps in the decision whether the model can be promoted through the different stages, such as staging and finally to production. 

We will be using [JupyterHub](https://jupyter.org/hub) and [MLFlow](https://www.mlflow.org/) to log the model and experiment results. 

#### Reproducible Dataset

This allows the data scientists to reproduce the model results, but also allows them to share the work with fellow data scientists or machine learning engineers who need to deploy the model.

We will be using [dvc](https://dvc.org/) in this workshop. 

### Productize Model

Once a model has been chosen, we will begin to productize the model from a notebook to source code. This allows better source revision control compared to having a notebook in a Source code management (SCM). 

We will be using [Git](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F) to version control the source code. 

### Testing 

As part of the pipeline, the model will be validated and tested. 

### Model Deployment

Once the model has been built, the model will be packaged into a container image and deployed onto OpenShift by the pipeline. 

#### Model Serving

The model will be served using [Seldon](https://www.seldon.io/). The pipeline will build an image using Source-to-Image ([S2I](https://github.com/openshift/source-to-image)) and deploy the model onto OpenShift. 

## Model Monitoring and Observability

Seldon models can expose prometheus endpoints which we will use to monitor key model metrics using the Grafana dashboard. 

## Continuous Delivery 

[Tekton](https://tekton.dev/) CI/CD pipeline will be used in the workshop to automate the different stages, such as building, testing, deployment and promotion of the model onto OpenShift. 

## Suggested additional reading:
[MLOps Platform – Productionizing Machine Learning Models](https://www.xenonstack.com/blog/mlops/)


Let's get started!

