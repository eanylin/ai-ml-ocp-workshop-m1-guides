## Productize The Model

Now the model has been created, we will now begin to productize the model. 

We will be using CodeReady Workspaces and using `juyptext` to convert the notebook to python code.

## Pipeline

A pipeline in software development is an automated process that drives software through a path of building, testing, and deploying code. By automating the process, the objective is to minimize human error and maintain a consistent process for how software is deployed. Tools that are included in the pipeline could include compiling code, unit tests, code analysis, security, and installer creation. For containerized environments, this pipeline would also include packaging the code into a container to be deployed across the hybrid cloud. A pipeline is critical in supporting continuous integration and continuous deployment (CI/CD) processes.

There is a staging and production pipeline that is used to deploy the model into the staging or production namespace.

### Development Environment

This is for development. JupyterHub and CodeReady Workspaces runs in this environment. This is the place to run experiments.

### Staging Environment

The staging pipeline will deploy the model into this environment for further testing. 

The pipeline is responsible for:

1. Checking out the source code
2. Running the training code using `train.sh`
3. Build the image with the model and push the image to OpenShift's registry
4. Deploy the model 

### Production Environment

The production pipeline will promote (tag) the image from staging to production. 

## Logging Into CodeReady Workspaces

Built on the open Eclipse Che project, Red Hat CodeReady Workspaces uses Kubernetes and containers to provide any member of the development or IT team with a consistent, secure, and zero-configuration development environment. The user experience is as fast and familiar as an integrated development environment (IDE) on their laptop.

CodeReady Workspaces is included in OpenShift® and is available in the OpenShift Operator Hub. Once deployed, CodeReady Workspaces provides development teams a faster and more reliable foundation on which to work, and it gives operations centralized control and peace of mind.

## Converting A Notebook To Python Code

Within your workspace, click on "Open New Terminal" and run

``` bash

# This will convert the notebook into a python script
```

This will convert the notebook into a python code.

Next, we will then modify the code into a format that the pipeline can run to train and build the image with the model. The pipeline will call `train.sh` and expects the model to be written to a folder at `/workspace/model`.

The pipeline expects the model to be written to a folder which can then be used in a pipeline stage to build the image.

We have prepared the refactored model at `src/`. Copy it to `src/blah`.

## Reviewing the New Model

The python code generated from the notebook has been refactored into a python class and visualization removed.  

Furthermore, the version of the data used is going to be committed together with source, thus allowing us to have reproducible result easily with dvc. 

```

```

## Testing The Model

``` bash
pip install -r src/train/requirements.txt
/run/something

some output here
```

## Commit the Code

```bash

git add
git commit -m 'my model'
git push -v origin master
```

The code has now been pushed to [your]({{GIT_URL}}) git repository.

