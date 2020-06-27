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

You will be using Red Hat CodeReady Workspaces, an online IDE based on Eclipse Che. Built on the open Eclipse Che project, Red Hat CodeReady Workspaces uses Kubernetes and containers to provide any member of the development or IT team with a consistent, secure, and zero-configuration development environment. The user experience is as fast and familiar as an integrated development environment (IDE) on their laptop.

CodeReady Workspaces is included in OpenShift® and is available in the OpenShift Operator Hub. Once deployed, CodeReady Workspaces provides development teams a faster and more reliable foundation on which to work, and it gives operations centralized control and peace of mind.

To get started, [access the CodeReady Workspaces instance]({{ ECLIPSE_CHE_URL }}), and log in using the username and password you’ve been assigned (e.g. `{{ USER_ID }}/{{ CHE_USER_PASSWORD }}`):

![che-login]({% image_path che-login.png %})

Once you log in, you’ll be placed on your personal dashboard. Click on the name of the pre-created workspace on the left, as shown below (the name will be different depending on your assigned number). You can also click on the name of the workspace in the center, and then click on the green {{USER_ID}}-namespace that says _Open_ on the top right hand side of the screen.

After a minute or two, you’ll be placed in the workspace:

![che-workspace]({% image_path che-workspace.png %})

> If things get weird or your browser appears, you can simply reload the browser tab to refresh the view.

This IDE is based on Eclipse Che (which is in turn based on MicroSoft VS Code editor).

You can see icons on the left for navigating between project explorer, search, version control (e.g. Git), debugging, and other plugins.  You’ll use these during the course of this workshop. Feel free to click on them and see what they do:

*Changes to files are auto-saved every few seconds*, so you don’t need to explicitly save changes.

## Converting A Notebook To Python Code

Within your workspace, click on "Open New Terminal" and run

> The Terminal window in CodeReady Workspaces. You can open a terminal window for any of the containers running in your Developer workspace. For the rest of these labs, anytime you need to run a command in a terminal, you can use the **>_ New Terminal** command on the right:

This will convert the notebook into a python code.

~~~ bash
cd /projects/bin
./nb2py.sh /path/to/nb /path/lr.py
~~~ 


Next, we will then modify the code into a format that the pipeline can run to train and build the image with the model. The pipeline will call `train.sh` and expects the model to be written to a folder at `/workspace/model`. The pipeline expects the model to be written to a folder which can then be used in a pipeline stage to build the image.

We have prepared the refactored model at `src/`. Copy it to `src/blah`.

## Reviewing the New Model

The python code generated from the notebook has been refactored into a python class and visualization removed.  

Furthermore, the version of the data used is going to be committed together with source, thus allowing us to have reproducible result easily with dvc. 

~~~ python
sample code here
~~~ 

## Testing The Model


~~~ bash
pip install -r src/train/requirements.txt
/run/something

some output here
~~~ 

## Commit the Code

~~~ sh
git add
git commit -m 'my model'
git push -v origin master
~~~ 

The code has now been pushed to [your]({{GIT_URL}}) git repository.

