# 定义

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

## Service

Different from pod, service's IP address is permanent. Each service will be attached to each pod, but life cycle of service is not connected 
to pod.

There are two types of services: external services and internal services. External services open the communication from external sources, 
while internal services cannot be accessed by external sources.

## Ingress

For external request, it will first be sent to ingress, then Ingress forward it to the corresponding service. The purpose of using Ingress is 
that it can be configured to give externally-reachable URLs. It can also load balance traffic, terminate SSL / TLS, and offer 
name-based virtual hosting. 

参考：[Ingress [Kubernetes docs]](https://kubernetes.io/docs/concepts/services-networking/ingress/)

# ConfigMap

It would be tedious to restart app every time just to change a simple property, such as the DB url. K8s provides ConfigMap as external configuration 
of application.

However, it is not recommended to put credential information such as username and password in ConfigMap since it would be insecure.

# Secret

For crendential information, k8s also provide another component called Secret. Data in Secret will be encoded in base64 format.

# Access pod

`kubectl exec --stdin --tty <pod> -- /bin/bash`

参考：[Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)  

# 参考资源

[Kubernetes Tutorial for Beginners [FULL COURSE in 4 Hours] [YouTube]](https://www.youtube.com/watch?v=X48VuDVv0do)
  
[Ingress [Kubernetes docs]](https://kubernetes.io/docs/concepts/services-networking/ingress/)
