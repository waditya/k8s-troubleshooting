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

```shell
controlplane ~ ➜  k run nginx --image=nginx

pod/nginx created
```

### List the contents in the container 

```shell
controlplane ~ ➜  k exec nginx -- ls

bin
boot
dev
docker-entrypoint.d
docker-entrypoint.sh
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```
### View the content of a file inside the container 

```shell
controlplane ~ ➜  k exec nginx -- cat /usr/share/nginx/html/index.html

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

### Get an interactive shell

```bash
controlplane ~ ➜  k exec -i -t nginx -- /bin/sh
# ls

bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  usr

# 
# cd bin
# 
# ls -lart

total 27376
-rwxr-xr-x 1 root root        30 Jan 29  2020  rgrep
-rwxr-xr-x 1 root root      4577 Apr 10  2022  znew
-rwxr-xr-x 1 root root      1842 Apr 10  2022  zmore
-rwxr-xr-x 1 root root      2206 Apr 10  2022  zless
-rwxr-xr-x 1 root root      8103 Apr 10  2022  zgrep
-rwxr-xr-x 1 root root      2081 Apr 10  2022  zforce
-rwxr-xr-x 1 root root        29 Apr 10  2022  zfgrep
-rwxr-xr-x 1 root root        29 Apr 10  2022  zegrep
-rwxr-xr-x 1 root root      6460 Apr 10  2022  zdiff
-rwxr-xr-x 1 root root      1678 Apr 10  2022  zcmp
-rwxr-xr-x 1 root root      1984 Apr 10  2022  zcat
-rwxr-xr-x 2 root root      2346 Apr 10  2022  uncompress
-rwxr-xr-x 1 root root     98136 Apr 10  2022  gzip
-rwxr-xr-x 1 root root      6447 Apr 10  2022  gzexe
-rwxr-xr-x 2 root root      2346 Apr 10  2022  gunzip
lrwxrwxrwx 1 root root        22 Jun 17  2022  nawk -> /etc/alternatives/nawk
-rwxr-xr-x 1 root root    158376 Jun 17  2022  mawk
lrwxrwxrwx 1 root root        21 Jun 17  2022  awk -> /etc/alternatives/awk
-rwxr-xr-x 1 root root      6241 Jul  2  2022  deb-systemd-invoke
-rwxr-xr-x 1 root root     24358 Jul  9  2022  deb-systemd-helper
-rwxr-xr-x 1 root root     39760 Sep 20  2022  yes
-rwxr-xr-x 1 root root     39792 Sep 20  2022  whoami
-rwxr-xr-x 1 root root     60432 Sep 20  2022  who
-rwxr-xr-x 1 root root     52280 Sep 20  2022  wc
-rwxr-xr-x 1 root root    151344 Sep 20  2022  vdir
-rwxr-xr-x 1 root root     39824 Sep 20  2022  users
-rwxr-xr-x 1 root root     39760 Sep 20  2022  unlink
-rwxr-xr-x 1 root root     48080 Sep 20  2022  uniq
-rwxr-xr-x 1 root root     43952 Sep 20  2022  unexpand
-rwxr-xr-x 1 root root     43888 Sep 20  2022  uname
-rwxr-xr-x 1 root root     35696 Sep 20  2022  tty
-rwxr-xr-x 1 root root     56208 Sep 20  2022  tsort
-rwxr-xr-x 1 root root     43920 Sep 20  2022  truncate
-rwxr-xr-x 1 root root     35664 Sep 20  2022  true
-rwxr-xr-x 1 root root     56208 Sep 20  2022  tr
-rwxr-xr-x 1 root root    109616 Sep 20  2022  touch
-rwxr-xr-x 1 root root     48632 Sep 20  2022  timeout
-rwxr-xr-x 1 root root     60304 Sep 20  2022  test
-rwxr-xr-x 1 root root     43984 Sep 20  2022  tee
-rwxr-xr-x 1 root root     76944 Sep 20  2022  tail
-rwxr-xr-x 1 root root    113712 Sep 20  2022  tac
-rwxr-xr-x 1 root root     39824 Sep 20  2022  sync
-rwxr-xr-x 1 root root     52184 Sep 20  2022  sum
-rwxr-xr-x 1 root root     85008 Sep 20  2022  stty
-rwxr-xr-x 1 root root     60336 Sep 20  2022  stdbuf
-rwxr-xr-x 1 root root     97488 Sep 20  2022  stat
-rwxr-xr-x 1 root root     60984 Sep 20  2022  split
-rwxr-xr-x 1 root root    118456 Sep 20  2022  sort
-rwxr-xr-x 1 root root     43888 Sep 20  2022  sleep
-rwxr-xr-x 1 root root     60400 Sep 20  2022  shuf
-rwxr-xr-x 1 root root     64656 Sep 20  2022  shred
-rwxr-xr-x 1 root root     64464 Sep 20  2022  sha512sum
-rwxr-xr-x 1 root root     64464 Sep 20  2022  sha384sum
-rwxr-xr-x 1 root root     60368 Sep 20  2022  sha256sum
-rwxr-xr-x 1 root root     60368 Sep 20  2022  sha224sum
-rwxr-xr-x 1 root root     56272 Sep 20  2022  sha1sum
-rwxr-xr-x 1 root root     60336 Sep 20  2022  seq
-rwxr-xr-x 1 root root     43984 Sep 20  2022  runcon
-rwxr-xr-x 1 root root     56240 Sep 20  2022  rmdir
-rwxr-xr-x 1 root root     72752 Sep 20  2022  rm
-rwxr-xr-x 1 root root     52144 Sep 20  2022  realpath
-rwxr-xr-x 1 root root     52112 Sep 20  2022  readlink
-rwxr-xr-x 1 root root     43952 Sep 20  2022  pwd
-rwxr-xr-x 1 root root    138480 Sep 20  2022  ptx
-rwxr-xr-x 1 root root     64432 Sep 20  2022  printf
-rwxr-xr-x 1 root root     35664 Sep 20  2022  printenv
-rwxr-xr-x 1 root root     81008 Sep 20  2022  pr
-rwxr-xr-x 1 root root     48176 Sep 20  2022  pinky
-rwxr-xr-x 1 root root     43888 Sep 20  2022  pathchk
-rwxr-xr-x 1 root root     43920 Sep 20  2022  paste
-rwxr-xr-x 1 root root     80912 Sep 20  2022  od
-rwxr-xr-x 1 root root     68624 Sep 20  2022  numfmt
-rwxr-xr-x 1 root root     43920 Sep 20  2022  nproc
-rwxr-xr-x 1 root root     43920 Sep 20  2022  nohup
-rwxr-xr-x 1 root root    113776 Sep 20  2022  nl
-rwxr-xr-x 1 root root     43888 Sep 20  2022  nice
-rwxr-xr-x 1 root root    142968 Sep 20  2022  mv
-rwxr-xr-x 1 root root     43952 Sep 20  2022  mktemp
-rwxr-xr-x 1 root root     72912 Sep 20  2022  mknod
-rwxr-xr-x 1 root root     68784 Sep 20  2022  mkfifo
-rwxr-xr-x 1 root root     97552 Sep 20  2022  mkdir
lrwxrwxrwx 1 root root         6 Sep 20  2022  md5sum.textutils -> md5sum
-rwxr-xr-x 1 root root     52176 Sep 20  2022  md5sum
-rwxr-xr-x 1 root root    151344 Sep 20  2022  ls
-rwxr-xr-x 1 root root     39760 Sep 20  2022  logname
-rwxr-xr-x 1 root root     72824 Sep 20  2022  ln
-rwxr-xr-x 1 root root     39760 Sep 20  2022  link
-rwxr-xr-x 1 root root     56304 Sep 20  2022  join
-rwxr-xr-x 1 root root    159544 Sep 20  2022  install
-rwxr-xr-x 1 root root     48144 Sep 20  2022  id
-rwxr-xr-x 1 root root     39760 Sep 20  2022  hostid
-rwxr-xr-x 1 root root     48080 Sep 20  2022  head
-rwxr-xr-x 1 root root     43920 Sep 20  2022  groups
-rwxr-xr-x 1 root root     43920 Sep 20  2022  fold
-rwxr-xr-x 1 root root     48016 Sep 20  2022  fmt
-rwxr-xr-x 1 root root     35664 Sep 20  2022  false
-rwxr-xr-x 1 root root     85200 Sep 20  2022  factor
-rwxr-xr-x 1 root root    117808 Sep 20  2022  expr
-rwxr-xr-x 1 root root     43952 Sep 20  2022  expand
-rwxr-xr-x 1 root root     48536 Sep 20  2022  env
-rwxr-xr-x 1 root root     43856 Sep 20  2022  echo
-rwxr-xr-x 1 root root    175440 Sep 20  2022  du
-rwxr-xr-x 1 root root     39760 Sep 20  2022  dirname
-rwxr-xr-x 1 root root     52144 Sep 20  2022  dircolors
-rwxr-xr-x 1 root root    151344 Sep 20  2022  dir
-rwxr-xr-x 1 root root    102200 Sep 20  2022  df
-rwxr-xr-x 1 root root     89240 Sep 20  2022  dd
-rwxr-xr-x 1 root root    121904 Sep 20  2022  date
-rwxr-xr-x 1 root root     48112 Sep 20  2022  cut
-rwxr-xr-x 1 root root    122032 Sep 20  2022  csplit
-rwxr-xr-x 1 root root    151152 Sep 20  2022  cp
-rwxr-xr-x 1 root root     48048 Sep 20  2022  comm
-rwxr-xr-x 1 root root    142384 Sep 20  2022  cksum
-rwxr-xr-x 1 root root     72752 Sep 20  2022  chown
-rwxr-xr-x 1 root root     64496 Sep 20  2022  chmod
-rwxr-xr-x 1 root root     68656 Sep 20  2022  chgrp
-rwxr-xr-x 1 root root     68720 Sep 20  2022  chcon
-rwxr-xr-x 1 root root     44016 Sep 20  2022  cat
-rwxr-xr-x 1 root root     56208 Sep 20  2022  basenc
-rwxr-xr-x 1 root root     43856 Sep 20  2022  basename
-rwxr-xr-x 1 root root     48016 Sep 20  2022  base64
-rwxr-xr-x 1 root root     48016 Sep 20  2022  base32
-rwxr-xr-x 1 root root     60400 Sep 20  2022  b2sum
-rwxr-xr-x 1 root root     43888 Sep 20  2022  arch
-rwxr-xr-x 1 root root     68496 Sep 20  2022 '['
lrwxrwxrwx 1 root root         8 Dec 19  2022  ypdomainname -> hostname
lrwxrwxrwx 1 root root         8 Dec 19  2022  nisdomainname -> hostname
-rwxr-xr-x 1 root root     22680 Dec 19  2022  hostname
lrwxrwxrwx 1 root root         8 Dec 19  2022  domainname -> hostname
lrwxrwxrwx 1 root root         8 Dec 19  2022  dnsdomainname -> hostname
lrwxrwxrwx 1 root root         4 Jan  5  2023  sh -> dash
-rwxr-xr-x 1 root root    125640 Jan  5  2023  dash
-rwxr-xr-x 1 root root    126424 Jan  5  2023  sed
-rwxr-xr-x 1 root root     72136 Jan  8  2023  xargs
-rwxr-xr-x 1 root root    224848 Jan  8  2023  find
-rwxr-xr-x 1 root root      1827 Jan  8  2023  debconf-show
-rwxr-xr-x 1 root root      2995 Jan  8  2023  debconf-set-selections
-rwxr-xr-x 1 root root       647 Jan  8  2023  debconf-escape
-rwxr-xr-x 1 root root      1719 Jan  8  2023  debconf-copydb
-rwxr-xr-x 1 root root       608 Jan  8  2023  debconf-communicate
-rwxr-xr-x 1 root root     11541 Jan  8  2023  debconf-apt-progress
-rwxr-xr-x 1 root root      2859 Jan  8  2023  debconf
-rwxr-xr-x 1 root root    203152 Jan 24  2023  grep
-rwxr-xr-x 1 root root        41 Jan 24  2023  fgrep
-rwxr-xr-x 1 root root        41 Jan 24  2023  egrep
-rwxr-xr-x 1 root root     56400 Feb  3  2023  sdiff
-rwxr-xr-x 1 root root     68752 Feb  3  2023  diff3
-rwxr-xr-x 1 root root    155216 Feb  3  2023  diff
-rwxr-xr-x 1 root root     52176 Feb  3  2023  cmp
-rwxr-xr-x 1 root root     35136 Feb 26  2023  ngettext
-rwxr-xr-x 1 root root      5188 Feb 26  2023  gettext.sh
-rwxr-xr-x 1 root root     35136 Feb 26  2023  gettext
-rwxr-xr-x 1 root root     35136 Feb 26  2023  envsubst
-rwxr-xr-x 1 root root     14584 Mar  5  2023  lsattr
-rwxr-xr-x 1 root root     14584 Mar  5  2023  chattr
lrwxrwxrwx 1 root root         6 Mar 23  2023  sg -> newgrp
-rwsr-xr-x 1 root root     68248 Mar 23  2023  passwd
-rwsr-xr-x 1 root root     48896 Mar 23  2023  newgrp
-rwxr-xr-x 1 root root     53024 Mar 23  2023  login
-rwxr-xr-x 1 root root     32512 Mar 23  2023  lastlog
-rwsr-xr-x 1 root root     88496 Mar 23  2023  gpasswd
-rwxr-xr-x 1 root root     23072 Mar 23  2023  faillog
-rwxr-sr-x 1 root shadow   31184 Mar 23  2023  expiry
-rwsr-xr-x 1 root root     52880 Mar 23  2023  chsh
-rwsr-xr-x 1 root root     62672 Mar 23  2023  chfn
-rwxr-sr-x 1 root shadow   80376 Mar 23  2023  chage
-rwxr-xr-x 1 root root    474112 Mar 26  2023  gpgv
lrwxrwxrwx 1 root root        14 Apr  3  2023  pidof -> /sbin/killall5
-rwxr-xr-x 1 root root     30968 May  7  2023  tset
-rwxr-xr-x 1 root root     26896 May  7  2023  tput
-rwxr-xr-x 1 root root     22768 May  7  2023  toe
-rwxr-xr-x 1 root root     92512 May  7  2023  tic
-rwxr-xr-x 1 root root     18672 May  7  2023  tabs
lrwxrwxrwx 1 root root         4 May  7  2023  reset -> tset
lrwxrwxrwx 1 root root         3 May  7  2023  infotocap -> tic
-rwxr-xr-x 1 root root     63808 May  7  2023  infocmp
-rwxr-xr-x 1 root root     14584 May  7  2023  clear
lrwxrwxrwx 1 root root         3 May  7  2023  captoinfo -> tic
-rwxr-xr-x 1 root root     59712 May 11  2023  update-alternatives
-rwxr-xr-x 1 root root     88560 May 11  2023  dpkg-trigger
-rwxr-xr-x 1 root root     63824 May 11  2023  dpkg-statoverride
-rwxr-xr-x 1 root root    129520 May 11  2023  dpkg-split
-rwxr-xr-x 1 root root      4186 May 11  2023  dpkg-realpath
-rwxr-xr-x 1 root root    162384 May 11  2023  dpkg-query
-rwxr-xr-x 1 root root     21206 May 11  2023  dpkg-maintscript-helper
-rwxr-xr-x 1 root root    158264 May 11  2023  dpkg-divert
-rwxr-xr-x 1 root root    170512 May 11  2023  dpkg-deb
-rwxr-xr-x 1 root root    318096 May 11  2023  dpkg
-rwxr-xr-x 1 root root     59784 May 25  2023  apt-mark
-rwxr-xr-x 1 root root     27972 May 25  2023  apt-key
-rwxr-xr-x 1 root root     51592 May 25  2023  apt-get
-rwxr-xr-x 1 root root     26944 May 25  2023  apt-config
-rwxr-xr-x 1 root root     22920 May 25  2023  apt-cdrom
-rwxr-xr-x 1 root root     88456 May 25  2023  apt-cache
-rwxr-xr-x 1 root root     18752 May 25  2023  apt
-rwxr-xr-x 1 root root       946 Jul 28  2023  which.debianutils
lrwxrwxrwx 1 root root        23 Jul 28  2023  which -> /etc/alternatives/which
-rwxr-xr-x 1 root root     14520 Jul 28  2023  tempfile
-rwxr-xr-x 1 root root     10487 Jul 28  2023  savelog
-rwxr-xr-x 1 root root     27560 Jul 28  2023  run-parts
-rwxr-xr-x 1 root root     14664 Jul 28  2023  ischroot
-rwxr-xr-x 2 root root   3804432 Nov 25  2023  perl5.36.0
-rwxr-xr-x 2 root root   3804432 Nov 25  2023  perl
-rwxr-xr-x 1 root root    531984 Jan 20  2024  tar
lrwxrwxrwx 1 root root         4 Mar 29  2024  rbash -> bash
-rwxr-xr-x 1 root root     14488 Mar 29  2024  clear_console
-rwxr-xr-x 1 root root      6865 Mar 29  2024  bashbug
-rwxr-xr-x 1 root root   1265648 Mar 29  2024  bash
-rwxr-xr-x 1 root root    280800 Sep 17 19:29  curl
-rwxr-xr-x 1 root root   1815512 Oct  2 15:59  njs
lrwxrwxrwx 1 root root         7 Oct 18 12:56  x86_64 -> setarch
-rwxr-xr-x 1 root root     31504 Oct 18 12:56  whereis
-rwxr-xr-x 1 root root     72024 Oct 18 12:56  wdctl
-rwxr-xr-x 1 root root     39224 Oct 18 12:56  wall
-rwxr-xr-x 1 root root     31032 Oct 18 12:56  utmpdump
-rwxr-xr-x 1 root root     84520 Oct 18 12:56  unshare
-rwsr-xr-x 1 root root     35128 Oct 18 12:56  umount
-rwxr-xr-x 1 root root     63808 Oct 18 12:56  uclampset
-rwxr-xr-x 1 root root     63808 Oct 18 12:56  taskset
-rwsr-xr-x 1 root root     72000 Oct 18 12:56  su
-rwxr-xr-x 1 root root     47424 Oct 18 12:56  setterm
-rwxr-xr-x 1 root root     14648 Oct 18 12:56  setsid
-rwxr-xr-x 1 root root     80192 Oct 18 12:56  setpriv
-rwxr-xr-x 1 root root     27216 Oct 18 12:56  setarch
-rwxr-xr-x 1 root root     47416 Oct 18 12:56  scriptreplay
-rwxr-xr-x 1 root root     55608 Oct 18 12:56  scriptlive
-rwxr-xr-x 1 root root     71992 Oct 18 12:56  script
-rwxr-xr-x 1 root root     14648 Oct 18 12:56  rev
-rwxr-xr-x 1 root root     72000 Oct 18 12:56  resizepart
-rwxr-xr-x 1 root root     14648 Oct 18 12:56  renice
-rwxr-xr-x 1 root root     22840 Oct 18 12:56  rename.ul
-rwxr-xr-x 1 root root     39760 Oct 18 12:56  prlimit
-rwxr-xr-x 1 root root    121152 Oct 18 12:56  partx
lrwxrwxrwx 1 root root        23 Oct 18 12:56  pager -> /etc/alternatives/pager
-rwxr-xr-x 1 root root     35368 Oct 18 12:56  nsenter
-rwxr-xr-x 1 root root     35136 Oct 18 12:56  namei
-rwxr-xr-x 1 root root     18744 Oct 18 12:56  mountpoint
-rwsr-xr-x 1 root root     59704 Oct 18 12:56  mount
-rwxr-xr-x 1 root root     59712 Oct 18 12:56  more
-rwxr-xr-x 1 root root     18744 Oct 18 12:56  mesg
-rwxr-xr-x 1 root root     35200 Oct 18 12:56  mcookie
-rwxr-xr-x 1 root root     84288 Oct 18 12:56  lsns
-rwxr-xr-x 1 root root     67904 Oct 18 12:56  lsmem
-rwxr-xr-x 1 root root     96576 Oct 18 12:56  lslogins
-rwxr-xr-x 1 root root     72400 Oct 18 12:56  lslocks
-rwxr-xr-x 1 root root     35312 Oct 18 12:56  lsirq
-rwxr-xr-x 1 root root    100672 Oct 18 12:56  lsipc
-rwxr-xr-x 1 root root    123192 Oct 18 12:56  lsfd
-rwxr-xr-x 1 root root    129344 Oct 18 12:56  lscpu
-rwxr-xr-x 1 root root    207168 Oct 18 12:56  lsblk
-rwxr-xr-x 1 root root     56216 Oct 18 12:56  logger
lrwxrwxrwx 1 root root         7 Oct 18 12:56  linux64 -> setarch
lrwxrwxrwx 1 root root         7 Oct 18 12:56  linux32 -> setarch
lrwxrwxrwx 1 root root         4 Oct 18 12:56  lastb -> last
-rwxr-xr-x 1 root root     51520 Oct 18 12:56  last
-rwxr-xr-x 1 root root     76096 Oct 18 12:56  ipcs
-rwxr-xr-x 1 root root     35136 Oct 18 12:56  ipcrm
-rwxr-xr-x 1 root root     35200 Oct 18 12:56  ipcmk
-rwxr-xr-x 1 root root     35136 Oct 18 12:56  ionice
lrwxrwxrwx 1 root root         7 Oct 18 12:56  i386 -> setarch
-rwxr-xr-x 1 root root     51600 Oct 18 12:56  hardlink
-rwxr-xr-x 1 root root     35136 Oct 18 12:56  getopt
-rwxr-xr-x 1 root root     35216 Oct 18 12:56  flock
-rwxr-xr-x 1 root root     85600 Oct 18 12:56  findmnt
-rwxr-xr-x 1 root root     35184 Oct 18 12:56  fincore
-rwxr-xr-x 1 root root     35136 Oct 18 12:56  fallocate
-rwxr-xr-x 1 root root     88656 Oct 18 12:56  dmesg
-rwxr-xr-x 1 root root     31040 Oct 18 12:56  delpart
-rwxr-xr-x 1 root root     67904 Oct 18 12:56  chrt
-rwxr-xr-x 1 root root     55616 Oct 18 12:56  choom
-rwxr-xr-x 1 root root     31040 Oct 18 12:56  addpart
-rwxr-xr-x 1 root root    976136 Oct 27 14:16  openssl
-rwxr-xr-x 1 root root      6841 Oct 27 14:16  c_rehash
-rwxr-xr-x 1 root root     23064 Nov  1 12:42  zdump
-rwxr-xr-x 1 root root     15351 Nov  1 12:42  tzselect
-rwxr-xr-x 1 root root     23232 Nov  1 12:42  pldd
-rwxr-xr-x 1 root root    298912 Nov  1 12:42  localedef
-rwxr-xr-x 1 root root     47272 Nov  1 12:42  locale
-rwxr-xr-x 1 root root      5406 Nov  1 12:42  ldd
lrwxrwxrwx 1 root root        27 Nov  1 12:42  ld.so -> /lib64/ld-linux-x86-64.so.2
-rwxr-xr-x 1 root root     64648 Nov  1 12:42  iconv
-rwxr-xr-x 1 root root     36320 Nov  1 12:42  getent
-rwxr-xr-x 1 root root     27136 Nov  1 12:42  getconf
drwxr-xr-x 1 root root      4096 Nov 11 00:00  ..
drwxr-xr-x 1 root root      4096 Nov 12 02:03  .
```


## kubectl port-forward 

This is used to forward traffic from local port to a remote port.
This can be used to access services not exposed on internet or to debug services.

```shell
controlplane ~ ➜  k get svc

NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   99m

controlplane ~ ➜  k get svc -o yaml

apiVersion: v1
items:
- apiVersion: v1
  kind: Service
  metadata:
    creationTimestamp: "2024-11-19T16:31:02Z"
    labels:
      component: apiserver
      provider: kubernetes
    name: kubernetes
    namespace: default
    resourceVersion: "234"
    uid: d62012c3-8d05-4aaa-adb5-213132ad2792
  spec:
    clusterIP: 10.96.0.1
    clusterIPs:
    - 10.96.0.1
    internalTrafficPolicy: Cluster
    ipFamilies:
    - IPv4
    ipFamilyPolicy: SingleStack
    ports:
    - name: https
      port: 443
      protocol: TCP
      targetPort: 6443
    sessionAffinity: None
    type: ClusterIP
  status:
    loadBalancer: {}
kind: List
metadata:
  resourceVersion: ""
  ```

  ```shell
controlplane ~ ➜  curl -IL http://10.109.185.39:80

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 1032
ETag: W/"408-qZoCxCBllOwneE5XEkaFaesRwUs"
Date: Tue, 19 Nov 2024 18:23:32 GMT
Connection: keep-alive
Keep-Alive: timeout=5
```

```bash
controlplane ~ ➜  k port-forward -n uat svc/notes-app-svc 8000:80

Forwarding from 127.0.0.1:8000 -> 3000
Handling connection for 8000
Handling connection for 8000
Handling connection for 8000
Handling connection for 8000
Handling connection for 8000
```

On a seperate terminal tab, run below command : 

```bash 
controlplane ~ ✖ curl -IL localhost:8000

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 1032
ETag: W/"408-qZoCxCBllOwneE5XEkaFaesRwUs"
Date: Tue, 19 Nov 2024 18:25:42 GMT
Connection: keep-alive
Keep-Alive: timeout=5


controlplane ~ ➜  curl -IL localhost:8000

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 1032
ETag: W/"408-qZoCxCBllOwneE5XEkaFaesRwUs"
Date: Tue, 19 Nov 2024 18:39:03 GMT
Connection: keep-alive
Keep-Alive: timeout=5


controlplane ~ ➜  curl -IL localhost:8000

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 1032
ETag: W/"408-qZoCxCBllOwneE5XEkaFaesRwUs"
Date: Tue, 19 Nov 2024 18:39:04 GMT
Connection: keep-alive
Keep-Alive: timeout=5


controlplane ~ ➜  curl -IL localhost:8000

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 1032
ETag: W/"408-qZoCxCBllOwneE5XEkaFaesRwUs"
Date: Tue, 19 Nov 2024 18:39:05 GMT
Connection: keep-alive
Keep-Alive: timeout=5
```

## kubectl auth can-i 

```bash
controlplane ~ ➜  k auth can-i list pods -n default

yes
```

```bash
controlplane ~ ➜  k auth whoami

ATTRIBUTE   VALUE
Username    kubernetes-admin
Groups      [kubeadm:cluster-admins system:authenticated]
```
```bash
controlplane ~ ➜  k get roles

No resources found in default namespace.
```
```bash
controlplane ~ ➜  k create role kubernetes-developer --verb=create,get,list --resource="*"

role.rbac.authorization.k8s.io/kubernetes-developer created
```

```bash
controlplane ~ ➜  k get roles

NAME                   CREATED AT
kubernetes-developer   2024-11-19T18:50:24Z

controlplane ~ ➜  k get roles kubernetes-developer -o yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  creationTimestamp: "2024-11-19T18:50:24Z"
  name: kubernetes-developer
  namespace: default
  resourceVersion: "13197"
  uid: ca41f3ef-ac70-4a08-ba8a-0229fdc82bdc
rules:
- apiGroups:
  - ""
  resources:
  - '*'
  verbs:
  - create
  - get
  - list

controlplane ~ ➜ k create rolebinding k8s-developer-sa-rolebinding --role=kubernetes-developer --serviceaccount=default:k8s-developer

rolebinding.rbac.authorization.k8s.io/k8s-developer-sa-rolebinding created

controlplane ~ ➜  k auth can-i create pod
yes

controlplane ~ ➜  k auth can-i create pod --as=system:serviceaccount:default:k8s-developer
yes 

controlplane ~ ➜  k auth can-i create pod --as=jane
no 
```

```bash
controlplane ~ ➜ k auth can-i create pod --as=system:serviceaccount:default:k8s-developer --v=10
```
Output of the above commands shows the details foe the access.

```
I1119 19:06:33.006487   56442 loader.go:395] Config loaded from file:  /root/.kube/config
I1119 19:06:33.007043   56442 cached_discovery.go:113] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/servergroups.json
I1119 19:06:33.007254   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/flowcontrol.apiserver.k8s.io/v1beta3/serverresources.json
I1119 19:06:33.007279   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/authentication.k8s.io/v1/serverresources.json
I1119 19:06:33.007333   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/discovery.k8s.io/v1/serverresources.json
I1119 19:06:33.007710   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/apiregistration.k8s.io/v1/serverresources.json
I1119 19:06:33.007718   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/storage.k8s.io/v1/serverresources.json
I1119 19:06:33.007776   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/coordination.k8s.io/v1/serverresources.json
I1119 19:06:33.007776   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/policy/v1/serverresources.json
I1119 19:06:33.007837   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/events.k8s.io/v1/serverresources.json
I1119 19:06:33.007838   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/certificates.k8s.io/v1/serverresources.json
I1119 19:06:33.007875   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/flowcontrol.apiserver.k8s.io/v1/serverresources.json
I1119 19:06:33.007878   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/batch/v1/serverresources.json
I1119 19:06:33.007339   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/apiextensions.k8s.io/v1/serverresources.json
I1119 19:06:33.007915   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/autoscaling/v1/serverresources.json
I1119 19:06:33.007332   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/node.k8s.io/v1/serverresources.json
I1119 19:06:33.007919   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/admissionregistration.k8s.io/v1/serverresources.json
I1119 19:06:33.008140   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/networking.k8s.io/v1/serverresources.json
I1119 19:06:33.008234   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/rbac.authorization.k8s.io/v1/serverresources.json
I1119 19:06:33.008268   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/scheduling.k8s.io/v1/serverresources.json
I1119 19:06:33.008282   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/authorization.k8s.io/v1/serverresources.json
I1119 19:06:33.008275   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/autoscaling/v2/serverresources.json
I1119 19:06:33.008645   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/apps/v1/serverresources.json
I1119 19:06:33.008745   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/v1/serverresources.json
I1119 19:06:33.009006   56442 cached_discovery.go:113] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/servergroups.json
I1119 19:06:33.009147   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/autoscaling/v2/serverresources.json
I1119 19:06:33.009165   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/flowcontrol.apiserver.k8s.io/v1beta3/serverresources.json
I1119 19:06:33.009196   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/authentication.k8s.io/v1/serverresources.json
I1119 19:06:33.009243   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/node.k8s.io/v1/serverresources.json
I1119 19:06:33.009245   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/authorization.k8s.io/v1/serverresources.json
I1119 19:06:33.009248   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/events.k8s.io/v1/serverresources.json
I1119 19:06:33.009267   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/coordination.k8s.io/v1/serverresources.json
I1119 19:06:33.009273   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/storage.k8s.io/v1/serverresources.json
I1119 19:06:33.009275   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/policy/v1/serverresources.json
I1119 19:06:33.009293   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/apiregistration.k8s.io/v1/serverresources.json
I1119 19:06:33.009302   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/apps/v1/serverresources.json
I1119 19:06:33.009306   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/certificates.k8s.io/v1/serverresources.json
I1119 19:06:33.009332   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/discovery.k8s.io/v1/serverresources.json
I1119 19:06:33.009338   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/autoscaling/v1/serverresources.json
I1119 19:06:33.009346   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/apiextensions.k8s.io/v1/serverresources.json
I1119 19:06:33.009334   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/flowcontrol.apiserver.k8s.io/v1/serverresources.json
I1119 19:06:33.009375   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/networking.k8s.io/v1/serverresources.json
I1119 19:06:33.009382   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/admissionregistration.k8s.io/v1/serverresources.json
I1119 19:06:33.009400   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/scheduling.k8s.io/v1/serverresources.json
I1119 19:06:33.009413   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/rbac.authorization.k8s.io/v1/serverresources.json
I1119 19:06:33.009418   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/batch/v1/serverresources.json
I1119 19:06:33.009436   56442 cached_discovery.go:77] returning cached discovery info from /root/.kube/cache/discovery/controlplane_6443/v1/serverresources.json
I1119 19:06:33.010194   56442 round_trippers.go:466] curl -v -XGET  -H "Accept: application/json, */*" -H "Impersonate-User: system:serviceaccount:default:k8s-developer" -H "User-Agent: kubectl/v1.31.0 (linux/amd64) kubernetes/9edcffc" 'https://controlplane:6443/api/v1'
I1119 19:06:33.010563   56442 round_trippers.go:495] HTTP Trace: DNS Lookup for controlplane resolved to [{192.24.239.13 }]
I1119 19:06:33.010745   56442 round_trippers.go:510] HTTP Trace: Dial to tcp:192.24.239.13:6443 succeed
I1119 19:06:33.016496   56442 round_trippers.go:553] GET https://controlplane:6443/api/v1 200 OK in 6 milliseconds
I1119 19:06:33.016512   56442 round_trippers.go:570] HTTP Statistics: DNSLookup 0 ms Dial 0 ms TLSHandshake 4 ms ServerProcessing 1 ms Duration 6 ms
I1119 19:06:33.016517   56442 round_trippers.go:577] Response Headers:
I1119 19:06:33.016530   56442 round_trippers.go:580]     X-Kubernetes-Pf-Flowschema-Uid: f42a019f-a8a3-4566-b6e7-f97a7c2bfec9
I1119 19:06:33.016537   56442 round_trippers.go:580]     X-Kubernetes-Pf-Prioritylevel-Uid: 1ad36276-ea1a-4d25-b93a-3e3b59edf609
I1119 19:06:33.016548   56442 round_trippers.go:580]     Date: Tue, 19 Nov 2024 19:06:33 GMT
I1119 19:06:33.016555   56442 round_trippers.go:580]     Audit-Id: e69d9941-5a4c-47ad-bd84-59cb379f099b
I1119 19:06:33.016558   56442 round_trippers.go:580]     Cache-Control: no-cache, private
I1119 19:06:33.016561   56442 round_trippers.go:580]     Content-Type: application/json
I1119 19:06:33.016722   56442 request.go:1351] Response Body: {"kind":"APIResourceList","groupVersion":"v1","resources":[{"name":"bindings","singularName":"binding","namespaced":true,"kind":"Binding","verbs":["create"]},{"name":"componentstatuses","singularName":"componentstatus","namespaced":false,"kind":"ComponentStatus","verbs":["get","list"],"shortNames":["cs"]},{"name":"configmaps","singularName":"configmap","namespaced":true,"kind":"ConfigMap","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["cm"],"storageVersionHash":"qFsyl6wFWjQ="},{"name":"endpoints","singularName":"endpoints","namespaced":true,"kind":"Endpoints","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["ep"],"storageVersionHash":"fWeeMqaN/OA="},{"name":"events","singularName":"event","namespaced":true,"kind":"Event","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["ev"],"storageVersionHash":"r2yiGXH7wu8="},{"name":"limitranges","singularName":"limitrange","namespaced":true,"kind":"LimitRange","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["limits"],"storageVersionHash":"EBKMFVe6cwo="},{"name":"namespaces","singularName":"namespace","namespaced":false,"kind":"Namespace","verbs":["create","delete","get","list","patch","update","watch"],"shortNames":["ns"],"storageVersionHash":"Q3oi5N2YM8M="},{"name":"namespaces/finalize","singularName":"","namespaced":false,"kind":"Namespace","verbs":["update"]},{"name":"namespaces/status","singularName":"","namespaced":false,"kind":"Namespace","verbs":["get","patch","update"]},{"name":"nodes","singularName":"node","namespaced":false,"kind":"Node","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["no"],"storageVersionHash":"XwShjMxG9Fs="},{"name":"nodes/proxy","singularName":"","namespaced":false,"kind":"NodeProxyOptions","verbs":["create","delete","get","patch","update"]},{"name":"nodes/status","singularName":"","namespaced":false,"kind":"Node","verbs":["get","patch","update"]},{"name":"persistentvolumeclaims","singularName":"persistentvolumeclaim","namespaced":true,"kind":"PersistentVolumeClaim","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["pvc"],"storageVersionHash":"QWTyNDq0dC4="},{"name":"persistentvolumeclaims/status","singularName":"","namespaced":true,"kind":"PersistentVolumeClaim","verbs":["get","patch","update"]},{"name":"persistentvolumes","singularName":"persistentvolume","namespaced":false,"kind":"PersistentVolume","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["pv"],"storageVersionHash":"HN/zwEC+JgM="},{"name":"persistentvolumes/status","singularName":"","namespaced":false,"kind":"PersistentVolume","verbs":["get","patch","update"]},{"name":"pods","singularName":"pod","namespaced":true,"kind":"Pod","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["po"],"categories":["all"],"storageVersionHash":"xPOwRZ+Yhw8="},{"name":"pods/attach","singularName":"","namespaced":true,"kind":"PodAttachOptions","verbs":["create","get"]},{"name":"pods/binding","singularName":"","namespaced":true,"kind":"Binding","verbs":["create"]},{"name":"pods/ephemeralcontainers","singularName":"","namespaced":true,"kind":"Pod","verbs":["get","patch","update"]},{"name":"pods/eviction","singularName":"","namespaced":true,"group":"policy","version":"v1","kind":"Eviction","verbs":["create"]},{"name":"pods/exec","singularName":"","namespaced":true,"kind":"PodExecOptions","verbs":["create","get"]},{"name":"pods/log","singularName":"","namespaced":true,"kind":"Pod","verbs":["get"]},{"name":"pods/portforward","singularName":"","namespaced":true,"kind":"PodPortForwardOptions","verbs":["create","get"]},{"name":"pods/proxy","singularName":"","namespaced":true,"kind":"PodProxyOptions","verbs":["create","delete","get","patch","update"]},{"name":"pods/status","singularName":"","namespaced":true,"kind":"Pod","verbs":["get","patch","update"]},{"name":"podtemplates","singularName":"podtemplate","namespaced":true,"kind":"PodTemplate","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"storageVersionHash":"LIXB2x4IFpk="},{"name":"replicationcontrollers","singularName":"replicationcontroller","namespaced":true,"kind":"ReplicationController","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["rc"],"categories":["all"],"storageVersionHash":"Jond2If31h0="},{"name":"replicationcontrollers/scale","singularName":"","namespaced":true,"group":"autoscaling","version":"v1","kind":"Scale","verbs":["get","patch","update"]},{"name":"replicationcontrollers/status","singularName":"","namespaced":true,"kind":"ReplicationController","verbs":["get","patch","update"]},{"name":"resourcequotas","singularName":"resourcequota","namespaced":true,"kind":"ResourceQuota","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["quota"],"storageVersionHash":"8uhSgffRX6w="},{"name":"resourcequotas/status","singularName":"","namespaced":true,"kind":"ResourceQuota","verbs":["get","patch","update"]},{"name":"secrets","singularName":"secret","namespaced":true,"kind":"Secret","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"storageVersionHash":"S6u1pOWzb84="},{"name":"serviceaccounts","singularName":"serviceaccount","namespaced":true,"kind":"ServiceAccount","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["sa"],"storageVersionHash":"pbx9ZvyFpBE="},{"name":"serviceaccounts/token","singularName":"","namespaced":true,"group":"authentication.k8s.io","version":"v1","kind":"TokenRequest","verbs":["create"]},{"name":"services","singularName":"service","namespaced":true,"kind":"Service","verbs":["create","delete","deletecollection","get","list","patch","update","watch"],"shortNames":["svc"],"categories":["all"],"storageVersionHash":"0/CO1lhkEBI="},{"name":"services/proxy","singularName":"","namespaced":true,"kind":"ServiceProxyOptions","verbs":["create","delete","get","patch","update"]},{"name":"services/status","singularName":"","namespaced":true,"kind":"Service","verbs":["get","patch","update"]}]}
I1119 19:06:33.017759   56442 request.go:1351] Request Body: {"kind":"SelfSubjectAccessReview","apiVersion":"authorization.k8s.io/v1","metadata":{"creationTimestamp":null},"spec":{"resourceAttributes":{"namespace":"default","verb":"create","resource":"pods"}},"status":{"allowed":false}}
I1119 19:06:33.017821   56442 round_trippers.go:466] curl -v -XPOST  -H "User-Agent: kubectl/v1.31.0 (linux/amd64) kubernetes/9edcffc" -H "Accept: application/json, */*" -H "Content-Type: application/json" -H "Impersonate-User: system:serviceaccount:default:k8s-developer" 'https://controlplane:6443/apis/authorization.k8s.io/v1/selfsubjectaccessreviews'
I1119 19:06:33.019066   56442 round_trippers.go:553] POST https://controlplane:6443/apis/authorization.k8s.io/v1/selfsubjectaccessreviews 201 Created in 1 milliseconds
I1119 19:06:33.019083   56442 round_trippers.go:570] HTTP Statistics: GetConnection 0 ms ServerProcessing 1 ms Duration 1 ms
I1119 19:06:33.019088   56442 round_trippers.go:577] Response Headers:
I1119 19:06:33.019097   56442 round_trippers.go:580]     X-Kubernetes-Pf-Prioritylevel-Uid: 1ad36276-ea1a-4d25-b93a-3e3b59edf609
I1119 19:06:33.019104   56442 round_trippers.go:580]     Content-Length: 639
I1119 19:06:33.019111   56442 round_trippers.go:580]     Date: Tue, 19 Nov 2024 19:06:33 GMT
I1119 19:06:33.019118   56442 round_trippers.go:580]     Audit-Id: d6a06847-7a8b-4bb6-9185-772b6ee48f1e
I1119 19:06:33.019125   56442 round_trippers.go:580]     Cache-Control: no-cache, private
I1119 19:06:33.019131   56442 round_trippers.go:580]     Content-Type: application/json
I1119 19:06:33.019134   56442 round_trippers.go:580]     X-Kubernetes-Pf-Flowschema-Uid: f42a019f-a8a3-4566-b6e7-f97a7c2bfec9
I1119 19:06:33.019147   56442 request.go:1351] Response Body: {"kind":"SelfSubjectAccessReview","apiVersion":"authorization.k8s.io/v1","metadata":{"creationTimestamp":null,"managedFields":[{"manager":"kubectl","operation":"Update","apiVersion":"authorization.k8s.io/v1","time":"2024-11-19T19:06:33Z","fieldsType":"FieldsV1","fieldsV1":{"f:spec":{"f:resourceAttributes":{".":{},"f:namespace":{},"f:resource":{},"f:verb":{}}}}}]},"spec":{"resourceAttributes":{"namespace":"default","verb":"create","resource":"pods"}},"status":{"allowed":true,"reason":"RBAC: allowed by RoleBinding \"k8s-developer-sa-rolebinding/default\" of Role \"kubernetes-developer\" to ServiceAccount \"k8s-developer/default\""}}
```