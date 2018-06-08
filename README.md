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
