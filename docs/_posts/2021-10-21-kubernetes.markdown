# Definition

Open source container orchestration tool

Help to manage containerized applications

# Why such tool is needed

Trend from monolith to microservices

increased usage of containers

# Features

1. high availibiliy
2. scalability
3. disaster recovery

# Components

## Node

A simple server of physical or virtual machine

## Pod

The smallest unit of k8s. It is the abstraction over container, and it creates the running environment of containers. Such abstraction
is useful when we want to replace current containers with others, for example, replace docker containers with rkt containers. We only need
to interact with k8s layer.

Pod usually only has one application running inside it, but there could also be some helper containers together with the main application 
running inside one pod.

Each pod has one unique IP address. When a pod dies, a new pod will be created to replace it, and this new pod will obtain a new IP address. 
In that case, communication with pods need to be reconfigured every time one pod dies, which calls the need for service.

## ReplicaSet

A ReplicaSet manages the replicas of a pods.

## Service

Different from pod, service's IP address is permanent. Each service will be attached to a set of pods, but life cycle of service is not connected 
to the pods.

There are two types of services: external services and internal services. External services open the communication from external sources, 
while internal services cannot be accessed by external sources.

## Ingress

For external request, it will first be sent to ingress, then Ingress forward it to the corresponding service. The purpose of using Ingress is 
that it can be configured to give externally-reachable URLs. It can also load balance traffic, terminate SSL / TLS, and offer 
name-based virtual hosting. 

参考：[Ingress [Kubernetes docs]](https://kubernetes.io/docs/concepts/services-networking/ingress/)

## ConfigMap

It would be tedious to restart app every time just to change a simple property, such as the DB url. K8s provides ConfigMap as external configuration 
of application.

However, it is not recommended to put credential information such as username and password in ConfigMap since it would be insecure.

## Secret

For crendential information, k8s also provide another component called Secret. Data in Secret will be encoded in base64 format.

## Volumes

Attach physical storage to pod. The storage can be either local or remote. One thing to notice is that k8s does not manage data persistency.

## Deployment

Deployment is the blueprint of pod, it is another layer of abstraction over pods (actually it's over ReplicaSet). We only need to create deployment 
to manipulate pods.

```mermaid
graph TD
Deployment --> ReplicaSet;
ReplicaSet --> Pod;
Pod --> Container;
```
## StatefulSet

Deployment should not be used to replicate DB pods, as replicated DB pods will access the same DB and it could introduce inconsistency problems. 
Another component, StatefulSet, is used to deploy stateful apps like DBs, and it ensures that no inconsistency problem can occur. However, StatefulSet 
is not easy to use, so users of k8s usually prefer to only deploy stateless apps in k8s.

# Processes

Three processes have to be installed on each node for K8s.

## Container runtime

Container such as docker

## Kublet

Kublet interacts with container and node.

## Kube proxy

Kube proxy forward the requests. It is used to implement the idea of Service.

## Master processes

There are 4 master prcoesses running on master node: Api server, Scheduler, Controller manager, and etcd.

**Api server** works as cluster gateway and gatekeeper for authentication. The requests coming from client will be validated by Api server and then 
forwarded to other processes.

**Scheduler** handles the requests forwarded by Api server, and starts the pod on one of the worker node. Scheduler decides which worker node to place the new 
pod.

**Controller manager** detects cluster state changes, and it will send request to scheduler to reschedule pods if a cluster dies.

**Etcd** is a key value store used for persistency. It saves all the changes such as a pod died or a pod is rescheduled.

Multiple master nodes should be used. The requests to Api servers on these master nodes will be load balanced, and the etcds are distributed storage across 
all master nodes.

# Minikube

Minikube is a node which runs both master processes and worker processes. It will create virtual box on the machine and run the node inside the virtual 
box. Minikube is used to set up a cluster in one machine for testing purposes.

# Kubectl

Kubectl is a command line tool for k8s cluster. We can use kubectl to send request to Api server on a node to interact with pods.

[Kubectl reference docs [Kubernetes docs]](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
> Documentation for all kubectl commands provided by Kubernetes.

## Main kubectl commands

`kubectl get`
> Display one or many resources. Frequently used to display active pods with `pods` suffix.

`kubectl logs`
> Print the logs from the specfied pod and container.

`kubectl exec`
> Execute a command in a container. It can be used to get an interactive shell on a pod. 
> (`kubectl exec --stdin --tty shell-demo -- /bin/bash`)

Reference：[Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)  

`kubectl edit`
> Edit a resource from the default editor.

`kubectl describe`
> Show details of a specific resource or group of resources.

`kubectl apply`
> Apply a configuration to a resource by file name or stdin.

# Reference

[Kubernetes Tutorial for Beginners [FULL COURSE in 4 Hours] [YouTube]](https://www.youtube.com/watch?v=X48VuDVv0do)
  
[Ingress [Kubernetes docs]](https://kubernetes.io/docs/concepts/services-networking/ingress/)

[Deployment [Kubernetes docs]](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

[Kubectl reference docs [Kubernetes docs]](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
