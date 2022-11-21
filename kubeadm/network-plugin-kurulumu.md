## Network plugin kurulumu
flannel network plugin'i kullanacağız.
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ wget https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
--2022-11-10 12:44:16--  https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4583 (4.5K) [text/plain]
Saving to: ‘kube-flannel.yml’

kube-flannel.yml                         100%[================================================================================>]   4.48K  --.-KB/s    in 0.001s  

2022-11-10 12:44:16 (6.88 MB/s) - ‘kube-flannel.yml’ saved [4583/4583]


</pre></code>
haeshin@master-ubuntu-2204-k8s:~$ ls
kube-flannel.yml
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl apply -f kube-flannel.yml
namespace/kube-flannel created
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.apps/kube-flannel-ds created
</pre></code>

<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl get pods -n kube-flannel
NAME                    READY   STATUS    RESTARTS   AGE
kube-flannel-ds-nqcqs   1/1     Running   0          60s
</pre></code>

<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl get nodes -o wide
NAME                     STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
master-ubuntu-2204-k8s   Ready    control-plane   30m   v1.25.3   192.168.1.25   <none>        Ubuntu 22.04.1 LTS   5.15.0-52-generic   containerd://1.6.9
</pre></code>
