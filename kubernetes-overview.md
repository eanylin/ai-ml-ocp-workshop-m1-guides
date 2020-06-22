Overview
========
Kubernetes is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications. It has a large and rapidly growing ecosystem.

The name Kubernetes originates from Greek, meaning helmsman or pilot. Google open-sourced the Kubernetes project in 2014.

This module provides a brief overview on the core concepts in Kubernetes, based on the documentation in [kubernetes.io](https://kubernetes.io/docs/home/).


Kubernetes Control Plane
========================
Kubernetes API objects are used to describe the *desired* state of the Kubernetes cluster. This includes describing the desired end state of the applications, workloads, container images, number of replicas, network, disk resources, etc.

The Control Plane maintains a record of all of the Kubernetes Objects in the system and runs continuous control loops to manage the state of all the objects. At any given point in time, the control loops will respond to changes in the cluster and work to make the actual state of all the objects match the desired state.

The Kubernetes Control Plane consists of a collection of processes running within the cluster:

* [Master] (https://kubernetes.io/docs/concepts/#overview)
  - A collection of 3 processes that run on a single node, designated as the master node in the cluster. The processes are [kube-apiserver](https://kubernetes.io/docs/admin/kube-apiserver/), [kube-controller-manager](https://kubernetes.io/docs/admin/kube-controller-manager/) and [kube-scheduler](https://kubernetes.io/docs/admin/kube-scheduler/)
  - Responsible for maintaining the desired state of the cluster
  - The master can also be replicated for availability and redundancy.

* [Node] (https://kubernetes.io/docs/concepts/architecture/nodes/)
  - Machines (VMs, physical servers, etc) that run the applications and cloud workflows
  - Nodes are controlled by Kubernetes Master
  - Runs 2 processes, i.e. [kubelet](https://kubernetes.io/docs/admin/kubelet/) and [kube-proxy](https://kubernetes.io/docs/admin/kube-proxy/)
  - kubelet allows the node to communicate with the Kubernetes Master
  - kube-proxy is a network proxy which reflects Kubernetes networking services on each node


Kubernetes Objects
==================
The basic Kubernetes object include:

* [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)
  - A Pod is the smallest unit in the Kubernetes object model that can be created or deployed. It represents processes running on the cluster.
  - A Pod encapsulates an application's container(s), storages resources, unique network identity (IP address) as well as options that govern how the container(s) should run

* [Service](https://kubernetes.io/docs/concepts/services-networking/service/)
  - An abstract way to expose an application running on a set of Pods as a network service
  - An abstraction which defines a logical set of Pods and a policy by which to access them
  - The set of Pods that are targeted by a Service is usually determined by a [selector](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)

* [Volume](https://kubernetes.io/docs/concepts/storage/volumes/)
  - On-disk files in a Container are ephemeral, which is not idea for non-trivial applications when running in Containers
  - Kubernetes volume allows data to be preserved across Container restarts
  - It has an explicit lifetime, i.e. the same as the Pod that encloses it
  - There is support for different types of [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes), such as awsElasticBlock, cephfs, etc
  
* [Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)
  - Kubernetes supports multiple virtual clusters backed by the same physical cluster
  - These virtual clusters are called namespaces and are a way to divide cluster resources between multiple users via [resource quota](https://kubernetes.io/docs/concepts/policy/resource-quotas/)

---

Kubernetes also contains higher-level abstractions that rely on controllers to build upon the basic objects to provide additional functionality and convenience features. These include:

* [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
  - Provides declarative updates for Pods and ReplicaSets
  - You can describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate.
  - Used in different scenarios, e.g.
    - Create a Deployment to rollout a ReplicaSet
      * ReplicaSet creates Pods in the background and checks the status of the rollout to see if it succeeds or not
    - Rollback to an earlier Deployment revision if the current state of the Deployment is not stable
      * Each rollback updates the revision of the Deployment
    - Scale up the Deployment to facilitate more load
    - Clean up ReplicaSets

* [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
  - Ensures that all or some Nodes run a copy of the Pod
  - Pods are added and garbage collected as nodes are added/removed from the cluster
  - Deleting a DaemonSet will clean up all the Pods that it created
  - Common use cases include the following:   
    - Running cluster storage daemon on every node
    - Running logs collection daemon on every node
    - Running node monitoring daemon on every node

* [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
  - Workload API object used to manage stateful applications
  - Manages deployment scaling of a set of Pods and provides *guarantees* about the ordering and *uniqueness* of these Pods
  - Unlike a Deployment, a StatefulSet maintains a sticky identity for each of their Pods
  - Pods are created from the same spec but are not interchangeable, i.e. each has a persistent identifier that it maintains across any rescheduling  

* [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
  - Maintain a stable set of replica Pods running at any given time
  - It is used to guarantee the availability of a specified number of identical Pods
  - A ReplicaSet is linked to its Pods via the Pods' [metadata.ownerReferences](https://kubernetes.io/docs/concepts/workloads/controllers/garbage-collection/#owners-and-dependents) field, which specifies what resource the current object is owned by

* [Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
  - Creates one or more Pods and ensures that a specified number of them successfully terminate
  - Tracks successful completions of pods
  - When a specified number of successful completions is reached, the task, i.e, Job is complete
  - Deleting a Job will clean up the Pods it created


Kubernetes Cluster Architecture
===============================
![k8s_architecture]({% image_path kubernetes-components.png %})


Containers
==========
Containers are a technology for packaging the compiled code for an application along with the dependencies it needs at run time. The standardization from having dependencies included means that we will always get the same behavior for the container no matter where we run it. Containers decouple applications from the underlying infrastructure, which makes it easy to deploy in different private/public clouds or operating systems (OS).


Container Images
----------------
A container image represents binary data that encapsulates an application and all its software dependencies. It is an executable software bundle that can run standalone and one that makes very well defined assumptions about its runtime environment.

The container image of an application is typically pushed to a registry before it is referred to in a Pod.

Containers are immutable by design, i.e. you cannot change the code of a container that is already running. If you have a containerized application and want to make changes, you need to build a new container that includes the change, then recreate the container to start from the updated image.


Container Runtimes
------------------
The container runtime is the software that is responsible for running containers.

Kubernetes supports several container runtimes:

* [CRI-O](https://cri-o.io/#what-is-cri-o)
* [containerd](https://containerd.io/docs/)
* [Docker](https://docs.docker.com/engine/)
* Any other implementation of the [Kubernetes CRI](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md) (Container Runtime Interface)


Containers vs Virtual Machines
------------------------------
Linux containers and virtual machines (VMs) are packaged computing environments that combine various IT components and isolate them from the rest of the system. Their main differences are in terms of scale and portability.

Containers are typically measured by the megabyte. They do not package anything bigger than an application and all the files necessary to run, and are often used to package single functions that perform specific tasks (known as a microservice). The lightweight nature of containers and their shared OS makes them very easy to move across multiple environments.

VMs are typically measured by the gigabyte. They usually contain their own OS, allowing them to perform multiple resource-intensive functions at once. The increased resources available to VMs allow them to abstract, split, duplicate, and emulate entire servers, OSs, desktops, databases, and networks. 

Compared to VMs, containers are best used to: 

* Build cloud-native apps
* Package microservices
* Instill DevOps or CI/CD practices
* Move scalable IT projects across a diverse IT footprint that shares the same OS

Compared to containers, VMs are best used to:

* House traditional, legacy, and monolithic workloads
* Isolate risky development cycles
* Provision infrastructural resources (such as networks, servers, and data)
* Run a different OS inside another OS (such as running Unix on Linux)
