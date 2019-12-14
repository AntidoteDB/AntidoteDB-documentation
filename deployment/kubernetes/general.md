# General

Examples on how to deploy Antidote via Kubernetes can be found [here](examples.md).

## Minikube

Minikube simulates a local kubernetes-cluster containing a single physical node. It is therefore good for testing purposes.

1. Install [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
1. Install [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)

### Configuration Kubectl

Configure kubectl to use minikube as its context, if minikube did not already do it automatically. Use `kubectl config view` to lookup the config file. The file itself can by default be found at:
- linux: ~/.kube/config
- windows: C:/user/\<userX\>/.kube/config

### Configuration Minikube
The configuration file can by default be found here:
- linux: ~/.minikube/config/config.json
- windows: C:/user/\<userX\>/.minikube/config/config.json  

Configure minikube to make it more suitable for testing to deploy Antidote:
- insecure registries: start minikube with the insecure-registry flag, e.g.: `minikube start --insecure-registry="hostname:port"`.
- give minikube more cpus and memory to use, by using the flags --cpus, --memory, e.g.: `minikube start --cpus 6 --memory 8192`. You can also edit the configuration file to add these flags to every `minikube start` command.

Note that you have to restart (delete, start) minikube to make these configuration changes valid. `minikube stop` is not sufficient.

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

