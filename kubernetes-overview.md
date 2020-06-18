Overview
========
Kubernetes is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications. It has a large and rapidly growing ecosystem.

The name Kubernetes originates from Greek, meaning helmsman or pilot. Google open-sourced the Kubernetes project in 2014.

This module provides a brief overview on the core concepts in Kubernetes, based on the documentation in [kubernetes.io](https://kubernetes.io/docs/home/).

Control Plane
=============

To work with Kubernetes, you use *Kubernetes API objects* to describe the *desired* state of your cluster. This includes describing the applications, workloads, container images, number of replicas, network, disk resources and more for the desired end state.

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
* [Service](https://kubernetes.io/docs/concepts/services-networking/service/)
* [Volume](https://kubernetes.io/docs/concepts/storage/volumes/)
* [Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

Kubernetes also contains higher-level abstractions that rely on controllers to build upon the basic objects to provide additional functionality and convenience features. These include:

* [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
* [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
* [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
* [Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/)

Architecture
============
![k8s_architecture]({% image_path kubernetes-components.png %})
