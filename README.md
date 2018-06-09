# notes made during investigation

## Introduction

- Kubernetes using containerization to enable application to be released and updated without downtime.
- Kubernetes is an open-source platform and is production-ready.
- Kubernetes coordinates a highly available cluster of computers that are connected to work as a single unit.

A cluster consists of one master that coordinates work and of several nodes, also called workers that run the applications: 
![Cluster Diagram](https://d33wubrfki0l68.cloudfront.net/99d9808dcbf2880a996ed50d308a186b5900cec9/40b94/docs/tutorials/kubernetes-basics/public/images/module_01_cluster.svg)

The master's obligation:
- scheduling applications
- maintaining application's desired state
- scaling applications
- rolling out new updates

Each node:
- has a Kubelet that manages the node and communicates with the master.
- should have tools for handling container operations such as [docker](https://www.docker.com) or [rkt](https://coreos.com/rkt/)
- communicate with the master using the Kubernetes API as well as end users do.

## Kubernetes deployments

- once the application instances are created, a Kubernetes Deployment Controller continuously monitors those instances
- and provides a self-healing mechanism to address machine failure or maintenance

![Kubernetes Cluster Deployment](https://d33wubrfki0l68.cloudfront.net/152c845f25df8e69dd24dd7b0836a289747e258a/4a1d2/docs/tutorials/kubernetes-basics/public/images/module_02_first_app.svg)

## Kubernetes Pods

Having done a Deployment, Kubernetes created a Pod to host your application instance.

- one pod represents a group of one or more application containers that includes:
  - shared storage (Volumes)
  - a unique cluster IP address and port
  - information about how to run each container
- Pods are the atomic unit of Kubernetes

![Pods Overview](https://d33wubrfki0l68.cloudfront.net/fe03f68d8ede9815184852ca2a4fd30325e5d15a/98064/docs/tutorials/kubernetes-basics/public/images/module_03_pods.svg)

## Kubernetes Node

- each node is managed by the master
- a node can have several pods
- each node runs at least:
  - a Kublet process that
    - communicates between master and node
    - manages pods and containers
  - a container runtime that
    - pulls container image from a registry
    - unpacks the container
    - runs the application

![Node overview](https://d33wubrfki0l68.cloudfront.net/5cb72d407cbe2755e581b6de757e0d81760d5b86/a9df9/docs/tutorials/kubernetes-basics/public/images/module_03_nodes.svg)

## Kubernetes Services

A Kubernetes Service is an abstraction layer which defines a logical set of Pods and enables external traffic exposure, load balancing and service discovery for those Pods.

- services enable a loose coupling between dependent Pods
- the set of pods of a service can be determined by a LabelSelector
- through a ``type`` in the ServiceSpec a service can be exposed in different ways:
  - ClusterIP (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
  - NodePort - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP.
  - LoadBalancer - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
  - ExternalName - Exposes the Service using an arbitrary name (specified by externalName in the spec) by returning a CNAME record with the name. No proxy is used. This type requires v1.7 or higher of kube-dns.

![Services and Labels](https://d33wubrfki0l68.cloudfront.net/cc38b0f3c0fd94e66495e3a4198f2096cdecd3d5/ace10/docs/tutorials/kubernetes-basics/public/images/module_04_services.svg)

### Services and Labels

- Services match a set of Pods using labels and selectors
- Labels are key/value pairs attached to objects and can be used for:
  - Designate objects for development, test, and production
  - Embed version tags
  - Classify an object using tags

![Services and Labels](https://d33wubrfki0l68.cloudfront.net/b964c59cdc1979dd4e1904c25f43745564ef6bee/f3351/docs/tutorials/kubernetes-basics/public/images/module_04_labels.svg)

## Scaling an application

- scaling is accomplished by changing the number of replicas in a Deployment
- Kubernetes also supports autoscaling of Pods
- Services have an integrated load-balancer that will distribute network traffic to all Pods
- Services will monitor continuously the running Pods using endpoints, to ensure the traffic is sent only to available Pods

Scaled to one Pod:
![Scaling - one Pod](https://d33wubrfki0l68.cloudfront.net/043eb67914e9474e30a303553d5a4c6c7301f378/0d8f6/docs/tutorials/kubernetes-basics/public/images/module_05_scaling1.svg)

Scaled to four Pods:
![Scalinig - four Pods](https://d33wubrfki0l68.cloudfront.net/30f75140a581110443397192d70a4cdb37df7bfc/b5f56/docs/tutorials/kubernetes-basics/public/images/module_05_scaling2.svg)
