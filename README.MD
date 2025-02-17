# Kubernetes

Kubernetes is a distributed system of servers that host software applications, and you interact with it primarily through your local command line - it's not a programming language.

Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.

Kubernetes orchestrates and manages collections of containers (often those created by Docker). 
It takes care of scaling, distribution, and connectivity among these containers.

Kubernetes clusters are running in production that have thousands of nodes. 
That's a lot of resources to manage! 
But that's the beauty of Kubernetes.

## 1. Install

### Kubectl

The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. 
It's a client that communicates with a Kubernetes API server.

### Minikube

In production, you probably wouldn't use Minikube, you would use a cluster of servers, probably in the cloud. 
That's expensive! 
Minikube is a fantastic tool that allows you to run a single-node Kubernetes cluster on your local machine.

Minikube is a great tool for learning Kubernetes, but it's not a production-scale Kubernetes cluster. 
The primary difference is that Minikube runs a single-node cluster, whereas production clusters are multi-node distributed systems.

With Minikube, you only get one node! 
So once your machine runs out of resources, you're out of luck. 
That's why Minikube is great for learning, but not for production.

### Deploying Synergychat

SynergyChat is a metaverse-enabled chat app that provides data-driven insights through cutting-edge AI models that run on Web 3 infrastructure. 

## 2. Pods

A Pod is the smallest and simplest unit in the Kubernetes object model that you create or deploy. 
It represents one (or sometimes more) running container(s) in a cluster. 
In a simple web application, you might have one single pod: the web server. 
As traffic grows, you might deploy that same code to multiple pods to handle the increased load. 
Several pods, one codebase. 
In a more complex backend system, you might have several pods for the web server and several pods that handle video processing. 
Multiple pods, multiple codebases.

Pods are just wrappers around containers. 
You can think of it as a Docker container with a little extra Kubernetes magic. 
The container is the actual application, and the Pod is the Kubernetes abstraction that manages the container and the resources it needs to run.

### Ephemeral

The ephemeral (fancy word for "temporary") nature of Pods is one of the defining features of Kubernetes. 
Unlike traditional virtual machines (VMs) or physical servers that might run indefinitely (or until hardware failure), Pods are designed to be spun up, torn down, and restarted at a moment's notice.

### Unique IP Addresses

Every Pod in a Kubernetes cluster has a unique internal-to-k8s IP address. 
By giving each Pod a unique IP, Kubernetes simplifies communication and service discovery within the cluster. 
Pods within the same Node or across different Nodes can easily communicate.

All the resources inside a k8s cluster are virtualized. 
So, the IP address of a Pod is not the same as the IP address of the Node it's running on. 
It's a virtual IP address that is only accessible from within the cluster.

## 3. Deployments

A Deployment provides declarative updates for Pods and ReplicaSets.

You describe your desired state in a Deployment, and the Deployment Controller's job is to make the current state match the desired state. 
You declare your hopes and dreams, and it's Kubernetes' job to make them come true.

### Replica Sets

A ReplicaSet maintains a stable set of replica Pods running at any given time. 
It's the thing that makes sure that the number of Pods you want running is the same as the number of Pods that are actually running.

You might be thinking, "I thought that's what a Deployment does." 
Well...yes.

A Deployment is a higher-level abstraction that manages the ReplicaSets for you. 
You can think of a Deployment as a wrapper around a ReplicaSet. 
Here's the rub:

You will probably never use ReplicaSets directly. 
I just need to mention what they are because you'll hear the term thrown around, and might even see them referenced in logs and such.

### YAML Config

Kubernetes resources are primarily configured using YAML files.

### API Service

We've deployed one service, and we've deployed multiple instances of it. Time to deploy a second service!

This service doesn't serve a webpage! 
It's a JSON API. 
It's the backend for our chat application. 
By deploying the API and configuring the front-end to talk to it, we'll have a functional chat application!

### Thrashing Pods

One of the most common problems you'll run into when working with Kubernetes is Pods that keep crashing and restarting. 
This is called "thrashing" and it's usually caused by one of a few things:

* The application recently had a bug introduced in the latest image version
* The application is misconfigured and can't start properly
* A dependency of the application is misconfigured and the application can't start properly
* The application is trying to use too much memory and is being killed by Kubernetes

When a pod's status is `CrashLoopBackoff`, that means the container is crashing (the program is exiting with error code 1).

Because Kubernetes is all about building self-healing systems, it will automatically restart the container. 
However, each time it tries to restart the container, if it crashes again, it will wait longer and longer in between restarts. 
That's why it's called a "backoff".

## 4. ConfigMaps

There are several ways to manage environment variables in Kubernetes. 
One of the most common ways is to use ConfigMaps. 
ConfigMaps allow us to decouple our configurations from our container images, which is important because we don't want to have to rebuild our images every time we want to change a configuration value.

ConfigMaps are a great way to manage innocent environment variables in Kubernetes. 
Things like:

* Ports
* URLs of other services
* Feature flags
* Settings that change between environments, like `DEBUG` mode

However, they are not cryptographically secure. 
ConfigMaps aren't encrypted, and they can be accessed by anyone with access to the cluster.

If you need to store sensitive information, you should use Kubernetes Secrets or a third-party solution.

## 5. Services

Services provide a stable endpoint for pods. 
They are an abstraction used to provide a stable endpoint and load balance traffic across a group of Pods. 
By "stable endpoint", I just mean that the service will always be available at a given URL, even if the pod is destroyed and recreated.

## 6. Ingress

## 7. Storage

## 8. Namespaces

## 9. Scaling

## 10. Nodes