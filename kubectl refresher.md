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
student-node ~ ➜  k create deployment web-01 --image=nginx:latest --replicas=2
```
The output will look like below : 
```  
deployment.apps/web-01 created   
```

```shell
student-node ~ ➜  k get pods   
```   
```      
NAME                     READY   STATUS              RESTARTS   AGE
web-01-8f4bd4c77-hh75m   0/1     ContainerCreating   0          9s
web-01-8f4bd4c77-tl6pp   0/1     ContainerCreating   0          9s
```

```shell
k get -n default deployment web-01 -o=jsonpath='{.spec.replicas}'
```  
```
2
```

```shell
student-node ~ ➜  k get -n default pod web-01-8f4bd4c77-tl6pp -o=jsonpath='{.status.podIP}'
```  
The output will look like : 
```   
10.42.2.3
```

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

```shell
student-node ~ ➜  k describe node cluster1-node01
```

Output will look like below :

```   
Name:               cluster1-node01
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/instance-type=k3s
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=cluster1-node01
                    kubernetes.io/os=linux
                    node.kubernetes.io/instance-type=k3s
Annotations:        alpha.kubernetes.io/provided-node-ip: 192.1.136.10
                    flannel.alpha.coreos.com/backend-data: {"VNI":1,"VtepMAC":"9e:ce:eb:12:a5:35"}
                    flannel.alpha.coreos.com/backend-type: vxlan
                    flannel.alpha.coreos.com/kube-subnet-manager: true
                    flannel.alpha.coreos.com/public-ip: 192.1.136.10
                    k3s.io/hostname: cluster1-node01
                    k3s.io/internal-ip: 192.1.136.10
                    k3s.io/node-args: ["agent","--server","https://cluster1-controlplane:6443","--token","********","--flannel-iface","eth0"]
                    k3s.io/node-config-hash: CUM5TDVJJKL37DKFKW3QJR6FXW7HL2BU2NS6OIAHQ3OKHTC3Z4XA====
                    k3s.io/node-env: {"K3S_DATA_DIR":"/var/lib/rancher/k3s/data/f9f37b05ac205a3ca783d075d40c4ba8be73efd8caf83a27add3ed0ab8035e96"}
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Mon, 28 Oct 2024 00:23:58 +0000
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  cluster1-node01
  AcquireTime:     <unset>
  RenewTime:       Mon, 28 Oct 2024 04:16:03 +0000
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Mon, 28 Oct 2024 04:11:17 +0000   Mon, 28 Oct 2024 00:23:58 +0000   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Mon, 28 Oct 2024 04:11:17 +0000   Mon, 28 Oct 2024 00:23:58 +0000   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Mon, 28 Oct 2024 04:11:17 +0000   Mon, 28 Oct 2024 00:23:58 +0000   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Mon, 28 Oct 2024 04:11:17 +0000   Mon, 28 Oct 2024 00:23:59 +0000   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.1.136.10
  Hostname:    cluster1-node01
Capacity:
  cpu:                18
  ephemeral-storage:  1016057248Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             98888308Ki
  pods:               110
Allocatable:
  cpu:                18
  ephemeral-storage:  988420490080
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             98888308Ki
  pods:               110
System Info:
  Machine ID:                 
  System UUID:                bbdbe79a-1085-7763-21af-7ea02d401aed
  Boot ID:                    daaabf14-e474-49d5-a0d6-455d652abe5c
  Kernel Version:             5.4.0-1106-gcp
  OS Image:                   Alpine Linux v3.16
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://1.7.15-k3s1
  Kubelet Version:            v1.30.0+k3s1
  Kube-Proxy Version:         v1.30.0+k3s1
PodCIDR:                      10.42.1.0/24
PodCIDRs:                     10.42.1.0/24
ProviderID:                   k3s://cluster1-node01
Non-terminated Pods:          (2 in total)
  Namespace                   Name                            CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                            ------------  ----------  ---------------  -------------  ---
  kube-system                 svclb-traefik-2ded8aed-shs6g    0 (0%)        0 (0%)      0 (0%)           0 (0%)         3h52m
  default                     web-01-8f4bd4c77-hh75m          0 (0%)        0 (0%)      0 (0%)           0 (0%)         20m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests  Limits
  --------           --------  ------
  cpu                0 (0%)    0 (0%)
  memory             0 (0%)    0 (0%)
  ephemeral-storage  0 (0%)    0 (0%)
  hugepages-1Gi      0 (0%)    0 (0%)
  hugepages-2Mi      0 (0%)    0 (0%)
Events:              <none>
```

## kubectl events

```shell
student-node ~ ➜  kubectl get events -n default  
```

The output will look like : 

```   
LAST SEEN   TYPE     REASON              OBJECT                        MESSAGE
32m         Normal   ScalingReplicaSet   deployment/web-01             Scaled up replica set web-01-8f4bd4c77 to 2
32m         Normal   SuccessfulCreate    replicaset/web-01-8f4bd4c77   Created pod: web-01-8f4bd4c77-hh75m
32m         Normal   SuccessfulCreate    replicaset/web-01-8f4bd4c77   Created pod: web-01-8f4bd4c77-tl6pp
32m         Normal   Scheduled           pod/web-01-8f4bd4c77-hh75m    Successfully assigned default/web-01-8f4bd4c77-hh75m to cluster1-node01
32m         Normal   Scheduled           pod/web-01-8f4bd4c77-tl6pp    Successfully assigned default/web-01-8f4bd4c77-tl6pp to cluster1-node02
32m         Normal   Pulling             pod/web-01-8f4bd4c77-hh75m    Pulling image "nginx:latest"
32m         Normal   Pulling             pod/web-01-8f4bd4c77-tl6pp    Pulling image "nginx:latest"
32m         Normal   Pulled              pod/web-01-8f4bd4c77-hh75m    Successfully pulled image "nginx:latest" in 6.898s (6.898s including waiting). Image size: 72950530 bytes.
32m         Normal   Created             pod/web-01-8f4bd4c77-hh75m    Created container nginx
32m         Normal   Pulled              pod/web-01-8f4bd4c77-tl6pp    Successfully pulled image "nginx:latest" in 6.961s (6.961s including waiting). Image size: 72950530 bytes.
32m         Normal   Created             pod/web-01-8f4bd4c77-tl6pp    Created container nginx
32m         Normal   Started             pod/web-01-8f4bd4c77-hh75m    Started container nginx
32m         Normal   Started             pod/web-01-8f4bd4c77-tl6pp    Started container nginx
```

## kubectl logs

```shell
student-node ~ ➜  k logs web-01-8f4bd4c77-hh75m 
```

The output will look like below : 

```   
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/10/28 03:55:35 [notice] 1#1: using the "epoll" event method
2024/10/28 03:55:35 [notice] 1#1: nginx/1.27.2
2024/10/28 03:55:35 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/10/28 03:55:35 [notice] 1#1: OS: Linux 5.4.0-1106-gcp
2024/10/28 03:55:35 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/10/28 03:55:35 [notice] 1#1: start worker processes
2024/10/28 03:55:35 [notice] 1#1: start worker process 80
2024/10/28 03:55:35 [notice] 1#1: start worker process 81
2024/10/28 03:55:35 [notice] 1#1: start worker process 82
2024/10/28 03:55:35 [notice] 1#1: start worker process 83
2024/10/28 03:55:35 [notice] 1#1: start worker process 84
2024/10/28 03:55:35 [notice] 1#1: start worker process 85
2024/10/28 03:55:35 [notice] 1#1: start worker process 86
2024/10/28 03:55:35 [notice] 1#1: start worker process 87
2024/10/28 03:55:35 [notice] 1#1: start worker process 88
2024/10/28 03:55:35 [notice] 1#1: start worker process 89
2024/10/28 03:55:35 [notice] 1#1: start worker process 90
2024/10/28 03:55:35 [notice] 1#1: start worker process 91
2024/10/28 03:55:35 [notice] 1#1: start worker process 92
2024/10/28 03:55:35 [notice] 1#1: start worker process 93
2024/10/28 03:55:35 [notice] 1#1: start worker process 94
2024/10/28 03:55:35 [notice] 1#1: start worker process 95
2024/10/28 03:55:35 [notice] 1#1: start worker process 96
2024/10/28 03:55:35 [notice] 1#1: start worker process 97
```

## kubectl logs --labels 

```shell
controlplane ~ ➜  k create deployment notes-app-deployment --image=pavansa/notes-app -r=2
```
The output will look like below : 
```
deployment.apps/notes-app-deployment created
```

```shell
controlplane ~ ➜  k edit deployments.apps notes-app-deployment 
error: deployments.apps "notes-app-deployment" is invalid
A copy of your changes has been stored to "/tmp/kubectl-edit-416172419.yaml"
error: Edit cancelled, no valid changes were saved.
```

```shell
controlplane ~ ✖ k replace --force -f /tmp/kubectl-edit-416172419.yaml
```
The output will look like below : 

```
deployment.apps "notes-app-deployment" deleted
deployment.apps/notes-app-deployment replaced
```

```shell
controlplane ~ ➜  k get pods --show-labels
```

```
NAME                                    READY   STATUS    RESTARTS   AGE   LABELS
notes-app-deployment-6486f64787-hb5s5   1/1     Running   0          10s   app=notes-app,pod-template-hash=6486f64787
notes-app-deployment-6486f64787-t5tc9   1/1     Running   0          10s   app=notes-app,pod-template-hash=6486f64787
```

To view logs spread across multiple pods and/or deployments on the basis of a common label,
use below command. 

```shell
controlplane ~ ➜  k logs -l app=notes-app
```

Output will show logs for all containers with label app-notes-app 

```
> notes-app@1.0.0 start /app
> node app.js

App is running on port 3000

> notes-app@1.0.0 start /app
> node app.js

App is running on port 3000
```

## kubectl logs --timestamps 

```shell
controlplane ~ ➜  k logs -l app=notes-app
```

Output w/p timestamps look like below : 

```
> notes-app@1.0.0 start /app
> node app.js

App is running on port 3000

> notes-app@1.0.0 start /app
> node app.js

App is running on port 3000
```

```shell
controlplane ~ ➜  k logs -l app=notes-app --timestamps
```

Output with --timestamps option will display timestamp if they are not part of default log format 

```
2024-11-19T15:50:15.199914591Z 
2024-11-19T15:50:15.199959074Z > notes-app@1.0.0 start /app
2024-11-19T15:50:15.199964570Z > node app.js
2024-11-19T15:50:15.199967347Z 
2024-11-19T15:50:15.538089674Z App is running on port 3000
2024-11-19T15:50:15.170309016Z 
2024-11-19T15:50:15.170353961Z > notes-app@1.0.0 start /app
2024-11-19T15:50:15.170367833Z > node app.js
2024-11-19T15:50:15.170372285Z 
2024-11-19T15:50:15.487163498Z App is running on port 3000
```

## kubectl logs --since 

```shell
controlplane ~ ➜  k run log-generator --image=chentex/random-logger:latest
```

Pod is created using the log generator image chentex/random-logger:latest
```
pod/log-generator created
```

```shell
controlplane ~ ➜  k get pods
```
Pod is running now 

```
NAME            READY   STATUS    RESTARTS   AGE
log-generator   1/1     Running   0          3s
```

```shell
controlplane ~ ➜  k logs log-generator --since=5s
```

Logs for last 5 seconds are seen in the container 

```
2024-11-19T17:09:19+0000 WARN A warning that should be ignored is usually at this level and should be actionable.
2024-11-19T17:09:21+0000 ERROR An error is usually an exception that has been caught and not handled.
```

```shell
controlplane ~ ➜  k logs log-generator --since=20s
```
Logs for last 20 seconds

```
2024-11-19T17:09:14+0000 ERROR An error is usually an exception that has been caught and not handled.
2024-11-19T17:09:19+0000 WARN A warning that should be ignored is usually at this level and should be actionable.
2024-11-19T17:09:21+0000 ERROR An error is usually an exception that has been caught and not handled.
2024-11-19T17:09:24+0000 ERROR An error is usually an exception that has been caught and not handled.
2024-11-19T17:09:28+0000 DEBUG This is a debug log that shows a log that can be ignored.
2024-11-19T17:09:29+0000 WARN A warning that should be ignored is usually at this level and should be actionable.
2024-11-19T17:09:34+0000 ERROR An error is usually an exception that has been caught and not handled.
```
```bash
controlplane ~ ➜  k logs log-generator --since=1h
```

Logs for last 1 hour 

```
2024-11-19T17:08:53+0000 INFO This is less important than debug log and is often used to provide context in the current task.
2024-11-19T17:08:54+0000 DEBUG This is a debug log that shows a log that can be ignored.
2024-11-19T17:08:55+0000 WARN A warning that should be ignored is usually at this level and should be actionable.
2024-11-19T17:08:58+0000 WARN A warning that should be ignored is usually at this level and should be actionable.
2024-11-19T17:09:01+0000 INFO This is less important than debug log and is often used to provide context in the current task.
2024-11-19T17:09:02+0000 ERROR An error is usually an exception that has been caught and not handled.
2024-11-19T17:09:03+0000 DEBUG This is a debug log that shows a log that can be ignored.
2024-11-19T17:09:06+0000 DEBUG This is a debug log that shows a log that can be ignored.
.
.
.
2024-11-19T17:28:57+0000 WARN A warning that should be ignored is usually at this level and should be actionable.
2024-11-19T17:29:02+0000 DEBUG This is a debug log that shows a log that can be ignored.
2024-11-19T17:29:05+0000 ERROR An error is usually an exception that has been caught and not handled.
2024-11-19T17:29:06+0000 INFO This is less important than debug log and is often used to provide context in the current task.
```

## kubectl logs --follow 

This command is used to stream the logs on the UI in realtime. 

```shell
controlplane ~ ➜  k logs log-generator --follow

2024-11-19T17:08:53+0000 INFO This is less important than debug log and is often used to provide context in the current task.
2024-11-19T17:08:54+0000 DEBUG This is a debug log that shows a log that can be ignored.
2024-11-19T17:08:55+0000 WARN A warning that should be ignored is usually at this level and should be actionable.
2024-11-19T17:08:58+0000 WARN A warning that should be ignored is usually at this level and should be actionable.
2024-11-19T17:09:01+0000 INFO This is less important than debug log and is often used to provide context in the current task.
2024-11-19T17:09:02+0000 ERROR An error is usually an exception that has been caught and not handled.
2024-11-19T17:09:03+0000 DEBUG This is a debug log that shows a log that can be ignored.
2024-11-19T17:09:06+0000 DEBUG This is a debug log that shows a log that can be ignored.
2024-11-19T17:09:07+0000 INFO This is less important than debug log and is often used to provide context in the current task.
```

## kubectl exec 

This command is similar to docker exec. It allows to run a command inside a running container. 

