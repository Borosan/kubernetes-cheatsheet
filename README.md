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

#Get the configuration of the cluster
kubectl config view

#List the API resources that are available
kubectl api-resources

#List the API versions that are available
kubectl api-versions

#List everything
kubectl get all --all-namespaces
```

### Daemonsets

Shortcode = **ds**

```text
#List one or more daemonsets
kubectl get daemonset

#Edit and update the definition of one or more daemonset
kubectl edit daemonset <daemonset_name>

#Create a new daemonset
kubectl create daemonset <daemonset_name>

#Delete a daemonset
kubectl delete daemonset <daemonset_name>

#Manage the rollout of a daemonset
kubectl rollout daemonset

#Display the detailed state of daemonsets within a namespace
kubectl describe ds <daemonset_name> -n <namespace_name>
```

### Deployments

Shortcode = **deploy**

```text
#List one or more deployments
kubectl get deployment

#Display the detailed state of one or more deployments
kubectl describe deployment <deployment_name>

#Edit and update the definition of one or more deployment on the server
kubectl edit deployment <deployment_name>

#Create one a new deployment
kubectl create deployment <deployment_name>

#Delete deployments
kubectl delete deployment <deployment_name>

#See the rollout status of a deployment
kubectl rollout status deployment <deployment_name>
```

### Events

Shortcode = **ev**

```text
#List recent events for all resources in the system
kubectl get events

#List Warnings only
kubectl get events --field-selector type=Warning

#List events but exclude Pod events
kubectl get events --field-selector involvedObject.kind!=Pod

#Pull events for a single node with a specific name
kubectl get events --field-selector involvedObject.kind=Node, involvedObject.name=<node_name>

#Filter out normal events from a list of events
kubectl get events --field-selector type!=Normal
```

### Logs

```text
#Print the logs for a pod
kubectl logs <pod_name>

#Print the logs for the last hour for a pod
kubectl logs --since=1h <pod_name>

#Get the most recent 20 lines of logs
kubectl logs --tail=20 <pod_name>

#Get logs from a service and optionally select which container
kubectl logs -f <service_name> [-c <$container>]

#Print the logs for a pod and follow new logs
kubectl logs -f <pod_name>

#Print the logs for a container in a pod
kubectl logs -c <container_name> <pod_name>

#Output the logs for a pod into a file named ‘pod.log’
kubectl logs <pod_name> pod.log

#View the logs for a previously failed pod
kubectl logs --previous <pod_name>
```

_For logs we also recommend using a tool developed by Johan Haleby called Kubetail. This is a bash script that will allow you to get logs from multiple pods simultaneously. You can learn more about it at its_ [_Github repository_](https://github.com/johanhaleby/kubetail)_._

### Manifest Files

Another option for modifying objects is through Manifest Files. Using this method is highly recommend. It is done by using yaml files with all the necessary options for objects configured. Also it is recommended to store your  yaml files in a git repository, so you can track changes and streamline changes.

```text
#Apply a configuration to an object by filename or stdin. Overrides the existing configuration.
kubectl apply -f manifest_file.yaml

#Create objects
kubectl create -f manifest_file.yaml

#Create objects in all manifest files in a directory
kubectl create -f ./dir

#Create objects from a URL
kubectl create -f ‘url’

#Delete an object
kubectl delete -f manifest_file.yaml
```

### Namespaces

Shortcode = **ns**

```text
#List one or more namespaces
kubectl get namespace <namespace_name>

#Display the detailed state of one or more namespace
kubectl describe namespace <namespace_name>

#Create namespace <name>
kubectl create namespace <namespace_name>

#Delete a namespace
kubectl delete namespace <namespace_name>

#Edit and update the definition of a namespace
kubectl edit namespace <namespace_name>

#Display Resource (CPU/Memory/Storage) usage for a namespace
kubectl top namespace <namespace_name>
```

### Nodes

Shortcode = **no**.

```text
#Update the taints on one or more nodes
kubectl taint node <node_name>

#List one or more nodes
kubectl get node

#Delete a node or multiple nodes
kubectl delete node <node_name>

#Display Resource usage (CPU/Memory/Storage) for nodes
kubectl top node

#Resource allocation per node
kubectl describe nodes | grep Allocated -A 5

#Pods running on a node
kubectl get pods -o wide | grep <node_name>

#Annotate a node
kubectl annotate node <node_name>

#Mark a node as unschedulable
kubectl cordon node <node_name>

#Mark node as schedulable
kubectl uncordon node <node_name>

#Drain a node in preparation for maintenance
kubectl drain node <node_name>

#Add or update the labels of one or more nodes
kubectl label node
```

### Pods

Shortcode = **po**

```text
#List one or more pods
kubectl get pod

#Display the detailed state of a pods
kubectl describe pod <pod_name>

#Create a pod
kubectl create pod <pod_name>

#Delete a pod
kubectl delete pod <pod_name>

#Get interactive shell on a a single-container pod
kubectl exec -it <pod_name> /bin/sh

#Execute a command against a container in a pod
kubectl exec <pod_name> -c <container_name> <command>

#Display Resource usage (CPU/Memory/Storage) for pods
kubectl top pod

#Add or update the annotations of a pod
kubectl annotate pod <pod_name> <annotation>

#Add or update the label of a pod
kubectl label pod <pod_name>
```

### Replication Controllers

Shortcode = **rc**

```text
#List the replication controllers
kubectl get rc

#List the replication controllers by namespace
kubectl get rc --namespace=”<namespace_name>”
```

### ReplicaSets

Shortcode = **rs**

```text
#List ReplicaSets
kubectl get replicasets

#Display the detailed state of one or more ReplicaSets
kubectl describe replicasets <replicaset_name>

#Scale a ReplicaSet
kubectl scale --replicas=[x] 
```

### Secrets

```text
#List secrets
kubectl get secrets

#List details about secrets
kubectl describe secrets

#Create a secret
kubectl create secretc

#Delete a secret
kubectl delete secret <secret_name>
```

### Services

Shortcode = **svc**

```text

```

.

.



