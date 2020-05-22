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


## Pod  

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
Logical kubernetes object to handle load balancing & scaling,high availability of Pods. It ensures particular no of Pods are running as per the ```replicas``` set.  

Just for example -  

```golang
type ReplicationController struct {
    Template podTemplate
    Replicas int // no of replicas for above mentioned pod
}
```  

commands -  

```
kubectl create -f replication-controller/rc-config.yaml
kubectl get rc
kubectl scale --replicas=8 -f replication-controller/rc-config.yaml
kubectl scale --replicas=8 rc rc-nginx
kubectl edit rc rc-nginx
kubectl delete rc rc-nginx
```

## Replica Set
It is similar to replication controller object to handle load balancing & scaling,high availability of Pods. It ensures particular no of Pods are running as per the ```replicas``` set. It has added feature called ```selector``` which is being used to check if particular pods are already running, if yes then these pods will be added to replica set. Only pods without parents(replicaSet or replicationController) will be matched.

Just for example -  

```golang
type ReplicaSet struct {
    Template podTemplate
    Replicas int // no of replicas for above mentioned pod
    Selector podSelector // match particular pods to be added to replica set
}
```  

commands -  

```
kubectl create -f replica-set/rs-config.yaml
kubectl get rs
kubectl scale --replicas=8 -f replica-set/rs-config.yaml
kubectl scale --replicas=8 rs rs-nginx
kubectl edit rs rs-nginx
kubectl delete rs rs-nginx
```

## Deployment
Kubernetes object for handling deployments of replica sets. The main use cases of deployment is as follows -  

-   Rollout replica sets
-   Deploy the new  state of pods
-   Rollback to an earlier version
-   Pause the deployments
-   Use the status of the deployment
-   Clean up older replica sets


Just for an example -

```golang
type Deployment struct {
    Template podTemplate
    Replicas int // no of replicas for above mentioned pod
    Selector podSelector // match particular pods to be added to replica set
}
``` 

commands -  

```
kubectl create -f deployment/deployment.yaml --record=true
kubectl get deployment
kubectl rollout status deployment nginx-deployment
kubectl rollout history deployment nginx-deployment
kubectl rollout undo deployment nginx-deployment --to-revision=2
kubectl scale --replicas=6 deployment nginx-deployment
kubectl delete deployment nginx-deployment
```