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