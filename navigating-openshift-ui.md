Navigating OpenShift Web Console
================================

This module provides a brief overview of the OpenShift Web Console.

The web console url will be `{{  CONSOLE_URL }}`

Your openshift user name is `{{  USER_ID }}` and password is `{{  OPENSHIFT_USER_PASSWORD }}`


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

Upon creating a project you will be brought to the overview page for the new project.

If you want to get to a list of all the projects that are available, you can select `Home->Projects` from the side menu on the left. You can click on the hamburger menu item button on the top left corner of the web console if you do not see the side menu.

![openshift_project_list]({% image_path openshift-project-list.png %})


Accessing Project
=================
Click on `myproject` and we will get to the `Overview` page of the project

![myproject_overview_page]({% image_path myproject-overview-page.png %})

Next, select the `Developer` perspective for the project instead of the `Adminstrator` perspective from the left hand side menu

![openshift_developer_view]({% image_path openshift-developer-view.png %})


Deploying Application Using Web Console
=======================================
Proceed to select *From Catalog*. This will bring us to the Developer Catalog.

![openshift_developer_catalog]({% image_path openshift-developer-catalog.png %})

In this example, we will deploy a web application which is implemented using Python.

Click on *Languages* and then select *Python*. We will see the options for deploying applications which are related to Python. Select the *Python* tile for the generic Python Source-to-Image (S2I) builder.

![developer_catalog_python]({% image_path developer-catalog-python.png %})

This will bring up a dialog with the details of the builder image. Click on `Create Application` in the dialog.

![python_s2i_create_app]({% image_path python-s2i-create-app.png %})

Under the *Git* settings, we will insert the Git Repo URL that we will be using for this example, i.e. `https://github.com/openshift-katacoda/blog-django-py`. This repository contains a sample implementation of a blog application, designed to show the various features of OpenShift. The blog application is implemented using Python and Django.

![django_git_repo_url]({% image_path django-git-repo-url.png %})

Next, we will scroll down to the *General* settings. We can see that the settings in the *Application Name* field have been pre-populated with values based on the Git repository name. Changes can be made as needed. We are leaving the settings with their default values for this example. Proceed to hit the `Create` button at the bottom of the page after this.

![django_application_settings]({% image_path django-application-settings.png %})

Upon hitting the `Create` button, we will get redirected to the *Topology* overview of the project.

The topology overview provides a visual representation of the application you have deployed.

The Git icon shown to the lower right of the ring can be clicked on to take us to the hosted Git repository from which the source code for the application was built.

![django_application_topology_view]({% image_path django-application-topology-view.png %})

The icon shown to the lower left represents the build of the application. The icon will change from showing an hour glass, indicating the build is starting, to a sync icon indicating the build is in progress, and finally to a tick or cross depending on whether the build was successful or failed. Clicking on this icon will take you to the details of the current build.

The ring itself will progress from being white, indicating the deployment is pending, to light blue indicating the deployment is starting, and blue to indicate the application is running. The ring can also turn dark blue if the application is stopping.

Once the build has started, click on the *View Logs* link shown on the Resources panel.

This will allow you to monitor the progress of the build as it runs. The build will have completely successfully when you see a final message of `Push successful`. This indicates that the container image for the application was pushed to the OpenShift internal image registry.

![django_image_build]({% image_path django-image-build.png %})

![django_image_build_logs]({% image_path django-image-build-logs.png %})

Once the build of the application image has completed, it will be deployed.

![django_app_deployment]({% image_path django-app-deployment.png %})

A *Route* is automatically created for the application upon successful creation of the Django application. The route will be exposed outside of the cluster. The URL which can be used to access the application from a web browser will be visible.

Once the application is running, the icon shown to the upper right can be clicked to open the URL for the application route which was created.
 
![django_app_deployed]({% image_path django-app-deployed.png %})

Clicking on the URL link will bring us to the Django application that was just deployed.

![django_app_webpage]({% image_path django-app-webpage.png %})


Administrator View
==================
Select the `Adminstrator` perspective for the project from the left hand side menu

![openshift_administrator_view]({% image_path openshift-administrator-view.png %})


Pods
----
OpenShift Container Platform leverages the Kubernetes concept of a pod, which is one or more containers deployed together on one host, and the smallest compute unit that can be defined, deployed, and managed.

Pods are the rough equivalent of a machine instance (physical or virtual) to a container. Each pod is allocated its own internal IP address, therefore owning its entire port space, and containers within pods can share their local storage and networking.

Pods have a lifecycle; they are defined, then they are assigned to run on a node, then they run until their container(s) exit or they are removed for some other reason. Pods, depending on policy and exit code, may be removed after exiting, or may be retained in order to enable access to the logs of their containers.

OpenShift Container Platform treats pods as largely immutable; changes cannot be made to a pod definition while it is running. OpenShift Container Platform implements changes by terminating an existing pod and recreating it with modified configuration, base image(s), or both. Pods are also treated as expendable, and do not maintain state when recreated. Therefore pods should usually be managed by higher-level controllers, rather than directly by users.

Go to the `Workloads` tab and select *Pods* to view the pods in this project.

![django_pods]({% image_path django-pods.png %})


Services
--------
A Kubernetes service serves as an internal load balancer. It identifies a set of replicated pods in order to proxy the connections it receives to them. Backing pods can be added to or removed from a service arbitrarily while the service remains consistently available, enabling anything that depends on the service to refer to it at a consistent address. The default service clusterIP addresses are from the OpenShift Container Platform internal network and they are used to permit pods to access each other.

Services are assigned an IP address and port pair that, when accessed, proxy to an appropriate backing pod. A service uses a label selector to find all the containers running that provide a certain network service on a certain port.

Like pods, services are REST objects. Go to the `Networking` tab and select *Services* to view the services in this project.

![django_services]({% image_path django-services.png %})


Routes
------
An OpenShift route is a way to expose a service by giving it an externally-reachable hostname like `www.example.com`. A defined route and the endpoints identified by its service can be consumed by a router to provide named connectivity that allows external clients to reach your applications.

![django_routes]({% image_path django-routes.png %})


Deployments and DeploymentConfigs
---------------------------------
Deployments and DeploymentConfigs in OpenShift Container Platform are API objects that provide two similar but different methods for fine-grained management over common user applications. A DeploymentConfig or a Deployment describes the desired state of a particular component of the application as a Pod template.

DeploymentConfigs involve one or more ReplicationControllers, which contain a point-in-time record of the state of a DeploymentConfig as a Pod template. Similarly, Deployments involve one or more ReplicaSets, a successor of ReplicationControllers.

The DeploymentConfig deployment system provides the following capabilities:

* A DeploymentConfig, which is a template for running applications
* Triggers that drive automated deployments in response to events
* User-customizable deployment strategies to transition from the previous version to the new version. A strategy runs inside a Pod commonly referred as the deployment process.
* A set of hooks (lifecycle hooks) for executing custom behavior in different points during the lifecycle of a deployment
* Versioning of your application in order to support rollbacks either manually or automatically in case of deployment failure
* Manual replication scaling and autoscaling

Go to the `Workloads` tab and select *Deployment Configs* to view the DeploymentConfig in this project.

![django_deployment_configs]({% image_path django-deployment-configs.png %})

![django_deployment_configs_yaml]({% image_path django-deployment-configs-yaml.png %})


Replication Controllers
-----------------------
A ReplicationController ensures that a specified number of replicas of a Pod are running at all times. If Pods exit or are deleted, the ReplicationController acts to instantiate more up to the defined number. Likewise, if there are more running than desired, it deletes as many as necessary to match the defined amount.

A ReplicationController configuration consists of:

* The number of replicas desired (which can be adjusted at runtime)
* A Pod definition to use when creating a replicated Pod
* A selector for identifying managed Pods

Go to the `Workloads` tab and select *Replication Controllers* to view the ReplicationController for this project.

![django_replication_controllers]({% image_path django-replication-controllers.png %})

![django_replication_controllers_overview]({% image_path django-replication-controllers-overview.png %})


Secrets
-------
The `Secret` object type provides a mechanism to hold sensitive information such as passwords, OpenShift Container Platform client configuration files, `dockercfg` files, private source repository credentials, and so on. Secrets decouple sensitive content from the pods. You can mount secrets into containers using a volume plug-in or the system can use secrets to perform actions on behalf of a pod.

![django_secrets]({% image_path django-secrets.png %})


Config Maps
-----------
Many applications require configuration using some combination of configuration files, command line arguments, and environment variables. These configuration artifacts should be decoupled from image content in order to keep containerized applications portable.

The `ConfigMap` object provides mechanisms to inject containers with configuration data while keeping containers agnostic of OpenShift Container Platform. A `ConfigMap` can be used to store fine-grained information like individual properties or coarse-grained information like entire configuration files or JSON blobs.

The `ConfigMap` API object holds key-value pairs of configuration data that can be consumed in pods or used to store configuration data for system components such as controllers. `ConfigMap` is similar to secrets, but designed to more conveniently support working with strings that do not contain sensitive information.

Go to the `Workloads` tab and select *Config Maps* to view the Config Maps for this project. In this case, we can see the *CA certificates* as config maps.

![django_configmaps]({% image_path django-configmaps.png %})


Persistent Volume and Volume Claim
----------------------------------
A `PersistentVolume` object is a storage resource in an OpenShift Container Platform cluster. Storage is provisioned by cluster administrator by creating `PersistentVolume` objects from sources such as GCE Persistent Disk, AWS Elastic Block Store (EBS), and NFS mounts.

Storage can be made available by laying claims to the resource. We can make a request for storage resources using a `PersistentVolumeClaim` object; the claim is paired with a volume that generally matches our request.

A `PersistentVolume` is a specific resource. A `PersistentVolumeClaim` is a request for a resource with specific attributes, such as storage size. In between the two is a process that matches a claim to an available volume and binds them together. This allows the claim to be used as a volume in a pod. OpenShift Container Platform finds the volume backing the claim and mounts it into the pod.

A `PersistentVolumeClaim` is used by a pod as a volume. OpenShift Container Platform finds the claim with the given name in the same namespace as the pod, then uses the claim to find the corresponding volume to mount.

![openshift_pv_pvc_sample]({% image_path openshift-pv-pvc-sample.png %})


Summary
=======
In this chapter, we learnt about deploying an application from source code using a Source-to-Image (S2I) builder. We have deployed the application from the web console from `Developer` perspective and looked at the different tabs under the `Administrator` perspective.

The web application was implemented using the Python programming language. OpenShift provides S2I builders for a number of different programming languages/frameworks in addition to Python. These include Java, NodeJS, Perl, PHP and Ruby.
