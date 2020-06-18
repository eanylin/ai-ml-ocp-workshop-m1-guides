Overview
========
Kubernetes is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications. It has a large and rapidly growing ecosystem.

The name Kubernetes originates from Greek, meaning helmsman or pilot. Google open-sourced the Kubernetes project in 2014.

This module provides a brief overview on the core concepts in Kubernetes, based on the documentation in [kubernetes.io](https://kubernetes.io/docs/home/).

Control Plane
=============

Kubernetes API objects are used to describe the *desired* state of the Kubernetes cluster. This includes describing the desired end state of the applications, workloads, container images, number of replicas, network, disk resources, etc.

The Control Plane maintains a record of all of the Kubernetes Objects in the system and runs continuous control loops to manage the state of all the objects. At any given point in time, the control loops will respond to changes in the cluster and work to make the actual state of all the objects match the desired state.

The Kubernetes Control Plane consists of a collection of processes running within the cluster:

* Kubernetes Master
  - A collection of 3 processes that run on a single node, designated as the master node in the cluster. The processes are [kube-apiserver](https://kubernetes.io/docs/admin/kube-apiserver/), [kube-controller-manager](https://kubernetes.io/docs/admin/kube-controller-manager/) and [kube-scheduler](https://kubernetes.io/docs/admin/kube-scheduler/)
  - Responsible for maintaining the desired state of the cluster

* Node
  - Machines (VMs, physical servers, etc) that run the applications and cloud workflows.
  - Nodes are controlled by Kubernetes Master
  - Runs 2 processes, i.e. [kubelet](https://kubernetes.io/docs/admin/kubelet/) and [kube-proxy](https://kubernetes.io/docs/admin/kube-proxy/)
  - kubelet allows the node to communicate with the Kubernetes Master
  - kube-proxy is a network proxy which reflects Kubernetes networking services on each node

Kubernetes Objects
==================

The basic Kubernetes object include:

* [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)
  - A Pod is the smallest unit in the Kubernetes object model that can be created or deploy. It represents processes running on the cluster.
  - A Pod encapsulates an application's container(s), storages resources, unique network identity (IP address) as well as options that govern how the container(s) should run

* [Service](https://kubernetes.io/docs/concepts/services-networking/service/)
  - An abstract way to expose an application running on a set of Pods as a network service
  - An abstraction which defines a logic set of Pods and a policy by which to access them
  - The set of Pods that are targeted by a Service is usually determined by a [selector](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)

* [Volume](https://kubernetes.io/docs/concepts/storage/volumes/)
  - On-disk files in a Container are ephemeral, which is not idea for non-trivial applications when running in Containers
  - Kubernetes volume allows data to be preserved across Container restarts
  - It has an explicit lifetime, i.e. the same as the Pod that encloses it
  - There is support for different types of [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes), such as awsElasticBlock, cephfs, etc
  
* [Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)
  - Kubernetes supports multiple virtual clusters backed by the same physical cluster
  - These virtual clusters are called namespaces and are a way to divide cluster resources between multiple users via [resource quota](https://kubernetes.io/docs/concepts/policy/resource-quotas/)

Kubernetes also contains higher-level abstractions that rely on controllers to build upon the basic objects to provide additional functionality and convenience features. These include:

* [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
* [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
* [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
* [Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/)

Architecture
============
![k8s_architecture]({% image_path kubernetes-components.png %})