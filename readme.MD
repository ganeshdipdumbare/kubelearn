# Kubernetes notes


## Kubernetes architecture
Kubernetes is consists of following components -  

**Master**
-   ApiServer - to communicate with kube cluser
-   etcd - distributed key-value store info about cluster nodes
-   controller - brain of orchestration
-   scheduler - schedule running of pods on nodes

**Worker**  

-   kubelet - communicate with master to send status of the node
-   container runtime - run container(in our case docker)

## Config file format
yaml file format for kube config files have following sections -
```
apiVersion:
kind:
metadata:
spec:  
```  

## Container 
Running instance of docker image which is sharing host OS core with other processes but is having its own OS packages to run the code(Ubuntu packages and libraries in case we are running Ubuntu docker image).


```golang
type Container struct {
    Environemnt ownEnv // with required packages and libs
    OSKernal sharedHostOSKernal // host OS kernal is shared 
}
```


## Pods  

Logical kubernetes objects which are responsible for running containers.  

Usually, 1 pod is dealing with 1 container but we can have pods with mutliple containres called ```multicontainer pod```. These contains main container and helper containers for the main container.  

just for example -

```golang
type Pod struct {
    Container mainContainer
    Container helperContainer // in case of multi container pods only
}
``` 

 

commands -

```bash
kubectl run nginx --image nginx

kubectl get pod

kubectl create -f pod/pod-definition.yaml  

kubectl describe pod myapp-pod

kubectl delete pod myapp-pod
```

## Replication Controller 
Logical kubernetes object to handle scaling of Pods. It ensures particular no of Pods are running as per the ```replicas``` set.  

Just for example -  

```golang
type ReplicationController struct {
    Template podTemplate
    Replicas int // no of replicas for above mentioned pod
}
```