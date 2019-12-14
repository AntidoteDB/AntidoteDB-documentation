# General

## Minikube

Minikube simulates a local kubernetes-cluster containing a single physical node. It is therefore good for testing purposes.

1. Install [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
1. 

## Deployment

In the deployment of Antidote on a Kubernetes-cluster we need the following Kubernetes objects:

- HeadlessService
- StatefulSet
    - ContainerTemplate
    - LocalPersistentVolumeClaim
- Services for exposing all instances
- StorageClass






### StatefulSet

[StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)

[HeadlessService](https://kubernetes.io/docs/concepts/services-networking/service/#headless-services)

### ContainerTemplate

### LocalPersistentVolume

[LocalPersistentVolume](https://kubernetes.io/blog/2019/04/04/kubernetes-1.14-local-persistent-volumes-ga/)

### Accessing each instance of a StatefulSet

[Service](https://kubernetes.io/docs/concepts/services-networking/service/)

### Creating a data center

### Connecting data centers

