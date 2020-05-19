# Kubernetes notes


## YAML file format
yaml file format for kube config files have following sections -  
  
    apiVersion:
    kind:
    metadata:
    spec:  

## Pods  

Logical kubernetes objects which are responsible for running containers.  

Usually, 1 pod is dealing with 1 container but we can have pods with mutliple containres called ```multicontainer pod```. These containes main container and helper containers for the main container.  

commands -  
    ```
    kubectl run nginx --image nginx
    ```  
    ```
    kubectl get pod
    ```  
    ```
    kubectl create -f pod/pod-definition.yaml
    ```  
    ```
    kubectl describe pod myapp-pod
    ```

