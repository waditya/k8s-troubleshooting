# kubectl refresher

<span style="color:green">&rarr;</span>  get  
<span style="color:green">&rarr;</span>  describe  
<span style="color:green">&rarr;</span>  logs  
<span style="color:green">&rarr;</span>  explain  
<span style="color:green">&rarr;</span>  exec  
<span style="color:green">&rarr;</span>  port-forward  
<span style="color:green">&rarr;</span>  top node  
<span style="color:green">&rarr;</span>  diff  
<span style="color:green">&rarr;</span>  auth-can-i

## kubectl get 

```shell 
student-node ~ ➜  k create deployment web-01 --image=nginx:latest --replicas=2```      
deployment.apps/web-01 created

```shell
student-node ~ ➜  k get pods   
```   
```      
NAME                     READY   STATUS              RESTARTS   AGE
web-01-8f4bd4c77-hh75m   0/1     ContainerCreating   0          9s
web-01-8f4bd4c77-tl6pp   0/1     ContainerCreating   0          9s
```

```k get -n default deployment web-01 -o=jsonpath='{.spec.replicas}'```  
2

```student-node ~ ➜  k get -n default pod web-01-8f4bd4c77-tl6pp -o=jsonpath='{.status.podIP}'```  
10.42.2.3

## kubectl describe 

```shell
student-node ~ ➜  k get nodes
```  
```  
NAME                    STATUS   ROLES                  AGE     VERSION
cluster1-node01         Ready    <none>                 3h52m   v1.30.0+k3s1
cluster1-node02         Ready    <none>                 3h52m   v1.30.0+k3s1
cluster1-controlplane   Ready    control-plane,master   3h52m   v1.30.0+k3s1
```   