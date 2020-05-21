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
## Pods  

Logical kubernetes objects which are responsible for running containers.  

Usually, 1 pod is dealing with 1 container but we can have pods with mutliple containres called ```multicontainer pod```. These contains main container and helper containers for the main container.  

just for example -

```golang
type Pod struct {
    Container mainContainer
    Container helperContainer
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