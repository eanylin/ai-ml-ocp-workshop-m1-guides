Navigating OpenShift Web Console
================================

This module provides a brief overview of the OpenShift Web Console.


Logging into An OpenShift Cluster
=================================

The OpenShift cluster web console login page prompts user for their Username and Password

![openshift_ui_login]({% image_path openshift-ui-login.png %})

We will be presented with a `Welcome to OpenShift` message and the option of creating a new project

![welcome_to_openshift]({% image_path welcome-to-openshift.png %})


Creating a New Project
======================
Proceed to create a new project by selecting `Create Project`

![openshift_create_new_project]({% image_path openshift-create-new-project.png %})

Upon creating a project you will be left on the overview page for the new project.

If you want to get to a list of all the projects that are available, you can select `Home->Projects` from the side menu on the left. You can click on the hamburger menu item button on the top left corner of the web console if you do not see the side menu.

![openshift_project_list]({% image_path openshift-project-list.png %})


Deploying Jupyter Notebooks in OpenShift
========================================
Jupyter notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text. It can be used in many different areas such as data cleaning and transformation, numerical simulation, statistical modeling, data visualization and machine learning.

To deploy the notebook, we will switch to `Developer Perspective` and go to the `Topology` tab on the left side menu

![openshift_ui_topology]({% image_path openshift-ui-topology.png %})


Next, we will select `From Catalog` for the options for deploying applications to our project. This will bring up the Developer Catalog. We will filter using the keyword `jupyter`.

![jupyter_catalog]({% image_path jupyter-catalog.png %})


We will select the tile for *Jupyter Notebook* and click on *Instantiate Template*

![jupyter_instantiate_template]({% image_path jupyter-instantiate-template.png %})


This will bring up a form with the parameters for the template which we can customize.

![jupyter_template_form]({% image_path jupyter-template-form.png %})

The purpose of the template parameters are:

* `APPLICATION_NAME` - The name of the deployment
* `NOTEBOOK_IMAGE` - The name of the image stream for the Jupyter notebook image and the version tag
* `NOTEBOOK_PASSWORD` - The password used to protect access to the Jupyter notebook
* `NOTEBOOK_INTERFACE` - The Jupyter notebook web interface to use. Setting this to lab will enable the JupyterLab web interface.
* `NOTEBOOK_MEMORY` - The maximum amount of memory the Jupyter noteboook deployment is allowed to use


We will click on the *Topology* view in the left menu to monitor the deployment.

The ring shown in the visualization of the deployment will change from white, indicating the deployment is pending, to light blue, indicating the application is starting, and finally blue, indicating the application is running.

![jupyter_notebook_deploying]({% image_path jupyter-notebook-deploying.png %})

![jupyter_notebook_deployed]({% image_path jupyter-notebook-deployed.png %})


Once the notebook is deployed, we can click on icon on the top right of the ring to access the URL for the deployment. It will bring us to the webpage of the Jupyter notebook.

![jupyter_notebook_open_url]({% image_path jupyter-notebook-open-url.png %})


At the login prompt, we will key in the username and password to get the Jupyter notebook file browser. At this point in time, we will be able to create new notebooks or upload existing ones.


![jupyter_notebook_login_prompt]({% image_path jupyter-notebook-login-prompt.png %})

![jupyter_notebook_file_browser]({% image_path jupyter-notebook-file-browser.png %})


Under the Hood with OC Commands
-------------------------------
The images for the Jupyter notebook application has been loaded to the project for this example. The image is needed in order for us to deploy the Jupyter notebook. We can verify by running the following command:

```
$ oc get imagestreams -o name
imagestream.image.openshift.io/s2i-minimal-notebook
```

The minimal Jupyter notebook images that have been loaded can be deployed as is, but to make it easier to secure access, add persistent storage, define resources, as well as use it as a Source-to-Image (S2I) builder to create custom Jupyter notebook images, the Jupyter on OpenShift project also provides a set of OpenShift templates. The ones that are loaded for this example can be verified using the following command:

```
$ oc get templates
NAME                  DESCRIPTION                                                                        PARAMETERS    OBJECTS
notebook-builder      Template for creating customised Jupyter Notebook images.                          5 (2 blank)   2
notebook-deployer     Template for deploying Jupyter Notebook images.                                    5 (1 blank)   3
notebook-quickstart   Template for creating and deploying customised Jupyter Notebook images.            8 (3 blank)   5
notebook-workspace    Template for deploying Jupyter Notebook images with persistent storage and we...   6 (1 blank)   4
```

In this example, we will be using the notebook-deployer template.

The purpose of the OpenShift templates which have been loaded are:

* `notebook-deployer` - Template for deploying a Jupyter notebook image.

* `notebook-builder` - Template for building a custom Jupyter notebook image using Source-to-Image (S2I) against a hosted Git repository. Python packages listed in the requirements.txt file of the Git repository will be installed and any files, including notebook images, will be copied into the image. The image can then be deployed using the notebook-deployer template.

* `notebook-quickstart` - Template for building and deploying a custom Jupyter notebook image. This combines the functionality of the notebook-builder and notebook-deployer templates.

* `notebook-workspace` - Template for deploying a Jupyter notebook instance which also attaches a persistent volume, and copies any Python packages and notebooks included in the image, into the persistent volume. Any work done on the notebooks, or to install additional Python packages, will survive a restart of the Jupyter notebook environment. A webdav interface is also enabled to allow remote mounting of the persistent volume to a local computer.


Finally, if we want to see all the resources that are deployed for the Jupyter notebook, we can run the following command (see below). This uses a label selector to reference only the resources for this deployment.

```
$ oc get all --selector app=custom-notebook -o name
pod/custom-notebook-1-4r4fc
replicationcontroller/custom-notebook-1
service/custom-notebook
deploymentconfig.apps.openshift.io/custom-notebook
route.route.openshift.io/custom-notebook
```
