# Kubernetes Cheat Sheet

**What is Kubernetes?** [Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/), also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.

It groups containers that make up an application into logical units for easy management and discovery. Kubernetes builds upon [15 years of experience of running production workloads at Google](http://queue.acm.org/detail.cfm?id=2898444), combined with best-of-breed ideas and practices from the community.

**What is kubectl?** Kubectl is a command line tool used to run commands against Kubernetes clusters. It does this by authenticating with the Master Node of your cluster and making API calls to do a variety of management actions. The format of a kubectl command looks like this:

```text
kubectl [command] [type] [name] [flags]
```

* \[command\]: specifies the action you want to perform like create, delete, get, apply
* \[type\]: any Kubernetes resource, whether automatically provided by Kubernetes \(like a service or a pod\) or created by you with a Custom Resource Definition
* \[name\]: the name you have given the resource — if you omit the name, kubectl will return every resource specified by the type
* \[flags\]: specify any additional global or command specific options such as the output format

### Cluster Management

```text
#Display endpoint information about the master and services in the cluster
kubectl cluster-info

#Display the Kubernetes version running on the client and server
kubectl version

```



.

.

.



