# Kubernetes Cheat Sheet

**What is Kubernetes?** [Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/), also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.

It groups containers that make up an application into logical units for easy management and discovery. Kubernetes builds upon [15 years of experience of running production workloads at Google](http://queue.acm.org/detail.cfm?id=2898444), combined with best-of-breed ideas and practices from the community.

**What is kubectl?** Kubectl is a command line tool used to run commands against Kubernetes clusters. It does this by authenticating with the Master Node of your cluster and making API calls to do a variety of management actions. 

The format of a kubectl command looks like this:

```text
kubectl [command] [type] [name] [flags]
```

* \[command\]: specifies the action you want to perform like create, delete, get, apply
* \[type\]: any Kubernetes resource, whether automatically provided by Kubernetes \(like a service or a pod\) or created by you with a Custom Resource Definition
* \[name\]: the name you have given the resource — if you omit the name, kubectl will return every resource specified by the type
* \[flags\]: specify any additional global or command specific options such as the output format

**What is kubeconfig ?** kubeconfig is a configuration file which is used by kubectl  In order to access your Kubernetes cluster.  The default kubectl configuration file is located at `~/.kube/config` and is referred to as the kubeconfig file.

kubeconfig files organize information about clusters, users, namespaces, and authentication mechanisms. The kubectl command uses these files to find the information it needs to choose a cluster and communicate with it. 

The loading order follows these rules:

1. If the `--kubeconfig` flag is set, then only the given file is loaded. The flag may only be set once and no merging takes place.
2. If the `$KUBECONFIG` environment variable is set, then it is parsed as a list of filesystem paths according to the normal path delimiting rules for your system.
3. Otherwise, the `${HOME}/.kube/config` file is used and no merging takes place.

If you see a message similar to the following, `kubectl` is not configured correctly or is not able to connect to a Kubernetes cluster.

### Cluster Management

```text
#Display endpoint information about the master and services in the cluster
kubectl cluster-info

#Display the status of all components in the cluster
kubectl get componentstatuses

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

#List all objects in current namespace plus labels
kubectl get all --show-labels

#List components that match label
kubectl get all --selector <label>

#Get a list of contexts
kubectl config get-contexts

#Get the current context
kubectl config current-context

#Switch current context
kubectl config use-context <context_name>
```

_The context consist of the clustername and namespace that the current user connects to._

### ConfigMaps

Shortcode=**cm**

```text
#Display ConfigMaps information
kubectl get configmaps

#create a configmap form a file
kubectl create configmap <my-cmname> --from-file=path/to/configmap/

#Display detailed information of a configmap
kubectl describe configmaps <configmap_name>

#delete a configmap
kubectl delete configmap  <configmap_name>
```

### Daemonsets

Shortcode = **ds**

```text
#List one or more daemonsets
kubectl get daemonset

#Display the detailed state of daemonsets within a namespace
kubectl describe ds <daemonset_name> -n <namespace_name>

#Edit and update the definition of one or more daemonset
kubectl edit daemonset <daemonset_name>

#Create a new daemonset
kubectl create daemonset <daemonset_name>

#Delete a daemonset
kubectl delete daemonset <daemonset_name>

#Manage the rollout of a daemonset
kubectl rollout daemonset
```

### Deployments

Shortcode = **deploy**

```text
#List one or more deployments
kubectl get deployments

#dumps all the deployment yaml code on the screen
kubectl get deployments <deployment_name> -o yaml

#Display the detailed state of one or more deployments
kubectl describe deployment <deployment_name>

#Edit and update the definition of one or more deployment on the server
kubectl edit deployment <deployment_name>

#Create one a new deployment
kubectl create deployment --image=<img_name> <deployment_name>

#Delete deployments
kubectl delete deployment <deployment_name>

#scale a deployment
kubectl scale deployment <deployment_name> --replicas=[X]

#Set Autoscaling config
kubectl autoscale deployment <deployment_name> --min=10 --max=15 --cpu-percent=80

#See the rollout status of a deployment
kubectl rollout status deployment <deployment_name>

#see rollout history of all deployments
kubectl rollout history deployment

#See the overview of recent changes
kubectl rollout history deployment <deployment_name>
kubectl rollout history deployment <deployment_name> --reversion=2

#bring down the new replicaset and bring up the old ones
kubectl rollout undo deployment <deployment_name>
kubectl rollout undo deployment <deployment_name> --to-revision=1

#Pause a rollout
kubectl rollout pause deployment <my_deployment>

#Resume a rollout
kubectl rollout resume deployment <my_deployment>

#expose a deployment as a kubernetes service (type can be  NodePort/ClusterIP for on-promise cluster)
kubectl expose deployment <deployment_name> --type=NodePort --targetport=80 --name=<myapp-service>

#Add label to a deployment
kubectl lable deployments <deployment_name> state=LA

#Show deployments labels
kubectl get deployments --show-labels
```

_If you use kubectl create to create a deployment, it will automatically get a lable with the  name  app=&lt;NameofDeployment&gt; ._ 

_Label  plays an essential role in the monitoring that the kubernetes deployment is doing, label is used to make sure that the suffcient amount of pods are available._

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

### Ingress

Shortcode=**ing**

```text
#List 
kubectl get ingress <ingress-resource-name>

# Display detailed information about an ingress resource
kubectl describe ingress <ingress-resource-name>
```

### Labels

Labels play an essential role in the kubernetes.

```text
#General command for adding label to one of kubernetes resources
kubectl label <resource_type> <resource_name> <label>
```

_Usually labels are applied automatically, or we add them trough the yaml files._

### Logs

Container doesn't have stout, to see what's happening within a container use logs:

```text
#Print the logs for a pod
kubectl logs <pod_name>

#Quering pod logs using label selector
kubectl logs -l app=<my-app>

#Print the logs for the last hour for a pod
kubectl logs --since=1h <pod_name>

#Get the most recent 20 lines of logs
kubectl logs --tail=20 <pod_name>

#Print the logs for a pod and follow new logs
kubectl logs -f <pod_name>

#Print the logs for a container in a pod
kubectl logs -c <container_name> <pod_name>

#Output the logs for a pod into a file named ‘pod.log’
kubectl logs <pod_name> pod.log

#Copy files out of pod (Requires tar binary in container).
kubectl cp <pod_name>:/var/log .

#View the logs for a previously failed pod
kubectl logs --previous <pod_name>

#Watch logs in real time:
kubectl attach <pod_name>
```

_For logs we also recommend using a tool developed by Johan Haleby called Kubetail. This is a bash script that will allow you to get logs from multiple pods simultaneously. You can learn more about it at its_ [_Github repository_](https://github.com/johanhaleby/kubetail)_._

### Manifest Files

Another option for modifying objects is through Manifest Files. Using this method is highly recommend. It is done by using yaml files with all the necessary options for objects configured. Also it is recommended to store your  yaml files in a git repository, so you can track changes and streamline changes.

```text
#Create objects
kubectl create -f manifest_file.yaml

#Create objects in all manifest files in a directory
kubectl create -f ./dir

#Create objects from a URL
kubectl create -f ‘url’

#Delete an object
kubectl delete -f manifest_file.yaml

#Apply a configuration to an object by filename or stdin. Overrides the existing configuration.
kubectl apply -f manifest_file.yaml

#Replace a configuration to an object by filename or stdin.
kubectl replace -f manifest_file.yaml
```

_Incase of error while using `apply` or `replace`, use `delete` and then `create` combination._

### Namespaces

Shortcode = **ns**

```text
#List one or more namespaces
kubectl get namespace <namespace_name>

#Display the detailed state of one or more namespace
kubectl describe namespace <namespace_name>

#Set default namesapce
kubectl config set-context $(kubectl config current-context) --namespace=<my-namespace>

#Create namespace <name>
kubectl create namespace <namespace_name>

#Delete a namespace
kubectl delete namespace <namespace_name>

#Edit and update the definition of a namespace
kubectl edit namespace <namespace_name>

#Display Resource (CPU/Memory/Storage) usage for a namespace
kubectl top namespace <namespace_name>
```

_The optional_ [_kubectx_](https://github.com/ahmetb/kubectx) _package can be used to make switching between namespaces easier, it contains **kubectx** to switch between context, and **kubens** to switch between Namespaces.  If multiple clusters are available to a kubernetes client, switching context is relevant. If multiple namespaces exist within a cluster, switching namespaces is relevant._

### Nodes

Shortcode = **no**.

```text
#List one or more nodes
kubectl get nodes

#Delete a node or multiple nodes
kubectl delete node <node_name>

#edit a node
kubectl edit node <node_name>

#Display Resource usage (CPU/Memory/Storage) for node(s)
kubectl top node <node_name> 

#Resource allocation per node
kubectl describe nodes | grep Allocated -A 5

#Pods running on a node
kubectl get pods -o wide | grep <node_name>

#Annotate a node
kubectl annotate node <node_name> <annotation>

#Update the taints on one or more nodes
kubectl taint node <node_name> <taint_name> 

#Mark a node as unschedulable
kubectl cordon node <node_name>

#Mark node as schedulable
kubectl uncordon node <node_name>

#Drain a node in preparation for maintenance
kubectl drain node <node_name>

#Add or update the labels of one or more nodes
kubectl label nodes <node-name> disktype=ssd

#list nodes and their labels
kubectl get nodes --show-labels
```

### Pods

Shortcode = **po**

```text
#List one or more pods
kubectl get pods

#dumps all the pod yaml code on the screen
kubectl get pods <pod_name> -o yaml

#Display the detailed state of a pods
kubectl describe pod <pod_name>

#create a pod, impremitive (depricated)
kubectl run <pod_name> --image=<image_name>

#Create a pod from a yaml file
kubectl create -f <pod.yaml>

#Delete a pod
kubectl delete pod <pod_name>

#edit a pod
kubectl edit pod <pod_name>

#Get interactive shell on a a single-container pod
kubectl exec -it <pod_name> /bin/sh


#Execute a command in a pod (pod contains a single container)
kubectl exec -it <pod_name>  -- <command>

#Execute a command against a container in a pod (pod contains multiple containers)
kubectl exec -it <pod_name> -c <container_name> -- <command>

#Display Resource usage (CPU/Memory/Storage) for pods
kubectl top pod

#Add or update the annotations of a pod
kubectl annotate pod <pod_name> <annotation>

#Add or update the label of a pod
kubectl label pods <pod_name> env=prod

#Get pods showing labels
kubectl get pods --show-labels

#Remove the lable of a pod
kubectl label pods <pod_name> <label>-
```

_kubectl exec only works on pods!_

### Persistent Volume

Shortcode=**pv**

```text
#List one or more persistent volumes
kubectl get pv

#Display the detailed information about a persistent volume
kubectl describe pv <pv_name>
```

### **Persistent Volume Claim**

shortcode=**pvc**

```text
#List one or more persistent volume claims
kubectl get pvc

#Display the detailed information about a persistent volume claim
kubectl describe pvc <pvc_name>
```

### Replication Controllers

Shortcode = **rc**

```text
#List the replication controllers
kubectl get rc

#List the replication controllers by namespace
kubectl get rc --namespace=<namespace_name>
```

### ReplicaSets

Shortcode = **rs**

```text
#List ReplicaSets
kubectl get replicasets

#Display the detailed state of one or more ReplicaSets
kubectl describe replicasets <replicaset_name>

#Scale a ReplicaSet
kubectl scale --replicas=[x] replicaset <replicaset_name>

#autoscale replicaset
kubectl autoscale rs <replicaset_name> --max=10 --min=3 --cpu-percent=50

#Delete a ReplicaSet
kubectl delete replicaset <replicaset-name>
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
#List one or more services
kubectl get services

#List endpoint pods of a service
kubectl get endpoints <service_name>

#Display the detailed state of a service
kubectl describe services <service_name>

#Expose a deployment as a new Kubernetes service
kubectl expose deployment <deployment_name> --port=[X] --target-port=[Y]

#Edit and update the definition of one or more services
kubectl edit services <service_name>

#delete a service
kubectl delete services <service_name>
```

_Select service type using --type . it could be ClusterIP, NodePort, LoadBalancer or ExternalName. Default is 'ClusterIP'._

_Services are using Labels, So it is very important for a Service that  a Label is present, if you try to expose something that doesn't have a lablel, you can use a Service on top of that._

### Service Accounts

Shortcode = **sa**

```text
#List service accounts
kubectl get serviceaccounts

#Display the detailed state of one or more service accounts
kubectl describe serviceaccounts

#Replace a service account
kubectl replace serviceaccount

#Delete a service account
kubectl delete serviceaccount <service_account_name>
```

### StatefulSet

Shortcode = **sts**

```text
#List StatefulSet
kubectl get statefulset

#Delete StatefulSet only (not pods)
kubectl delete statefulset/[stateful_set_name] --cascade=false
```

### Common Options

In Kubectl you can specify optional flags with commands. Here are some of the most common and useful ones. 

-o Output format. For example if you wanted to list all of the pods in ps output format with more information:

```text
kubectl get pods -o wide 
```

 `--dry-run` you can generate the yaml file  using  kubectl command,  with out creating that object`:`

```text
kubectl create pod <pod_name> --image=nginx --dry-run -o yaml > my-pod.yaml
```

-n Shorthand for --namespace. For example, if you’d like to list all the Pods in a specific Namespace you would do this command:

```text
kubectl get pods --namespace=[namespace_name]
kubectl get pods -n=[namespace_name]
```

`-f Filename`, directory, or URL to files to use to create a resource. For example when creating a pod using data in a file named newpod.json.

```text
kubectl create -f ./newpod.json
```

 _**Create vs Apply** :kubectl create can be used to create new resources while kubectl apply inserts or updates resources while maintaining any manual changes made like scaling pods._

`--field-selector`let you [select Kubernetes resources](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects) based on the value of one or more resource fields.  This `kubectl` command selects all Pods for which the value of the [`status.phase`](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#pod-phase) field is `Running`:

```text
kubectl get pods --field-selector status.phase=Running
```

 You can use the `=`, `==`, and `!=` operators with field selectors \(`=` and `==` mean the same thing\). 

```text
kubectl get pods -l environment=production,tier!=frontend
kubectl get pods -l 'environment in (production,test),tier notin (frontend,backend)'
```

 --watch or -w - watch for changes:

```text
kubectl get pods -n kube-system -w
```

`--record`  Add the current command as an annotation to the resource. The recorded change is useful for future introspection. For example, to see the commands executed in each Deployment revision:

```text
#it would add CHANGE-CAUSE
kubectl create -f deployment-definition.yml --record 

#Now it shows CHANGE-CAUSE
kubectl rollout history deployment/myapp-deployment
```

`-h` for getting help:

```text
kubectl -h
kubectl create -h
kubectl run -h
```

explain command is always handy:

```text
kubectl explain deployment
kubectl explain deployment.spec
kubectl explain deployment.spec.strategy
```

good luck!

.

.

.

Collected by Payam Borosan

