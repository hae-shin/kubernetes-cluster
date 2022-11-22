## Kubernetes Dashboard Kurulum


<pre><code>
wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.1/aio/deploy/recommended.yaml
</pre></code>

<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.1/aio/deploy/recommended.yaml
--2022-11-10 14:50:30--  https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.1/aio/deploy/recommended.yaml
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.111.133, 185.199.109.133, 185.199.110.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.111.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7621 (7.4K) [text/plain]
Saving to: ‘recommended.yaml’

recommended.yaml                         100%[================================================================================>]   7.44K  --.-KB/s    in 0.002s  

2022-11-10 14:50:30 (3.06 MB/s) - ‘recommended.yaml’ saved [7621/7621]
</pre></code>

<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ ls
kube-flannel.yml  recommended.yaml
</pre></code>

<pre><code>
kubectl apply -f recommended.yaml 
</pre></code>

<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl apply -f recommended.yaml 
namespace/kubernetes-dashboard created
serviceaccount/kubernetes-dashboard created
service/kubernetes-dashboard created
secret/kubernetes-dashboard-certs created
secret/kubernetes-dashboard-csrf created
secret/kubernetes-dashboard-key-holder created
configmap/kubernetes-dashboard-settings created
role.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard created
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
deployment.apps/kubernetes-dashboard created
service/dashboard-metrics-scraper created
deployment.apps/dashboard-metrics-scraper created
</pre></code>
<pre><code>
kubectl get pods -n kubernetes-dashboard
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl get pods -n kubernetes-dashboard
NAME                                         READY   STATUS    RESTARTS   AGE
dashboard-metrics-scraper-64bcc67c9c-jvhfv   1/1     Running   0          49s
kubernetes-dashboard-66c887f759-4qdkv        1/1     Running   0          49s
</pre></code>
<pre><code>
kubectl get svc -n kubernetes-dashboard
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl get svc -n kubernetes-dashboard
NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
dashboard-metrics-scraper   ClusterIP   10.100.12.31     <none>        8000/TCP   6m1s
kubernetes-dashboard        ClusterIP   10.107.108.167   <none>        443/TCP    6m1s
</pre></code>
<pre><code>
kubectl --namespace kubernetes-dashboard patch svc kubernetes-dashboard -p '{"spec": {"type": "NodePort"}}'
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl --namespace kubernetes-dashboard patch svc kubernetes-dashboard -p '{"spec": {"type": "NodePort"}}'
service/kubernetes-dashboard patched
</pre></code>
<pre><code>
get svc -n kubernetes-dashboard kubernetes-dashboard -o yaml
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl get svc -n kubernetes-dashboard kubernetes-dashboard -o yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"k8s-app":"kubernetes-dashboard"},"name":"kubernetes-dashboard","namespace":"kubernetes-dashboard"},"spec":{"ports":[{"port":443,"targetPort":8443}],"selector":{"k8s-app":"kubernetes-dashboard"}}}
  creationTimestamp: "2022-11-10T14:50:49Z"
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
  resourceVersion: "12601"
  uid: 0e4cdcb1-9cbb-4eb9-a7ae-e4437b573777
spec:
  clusterIP: 10.107.108.167
  clusterIPs:
  - 10.107.108.167
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 31121
    port: 443
    protocol: TCP
    targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}
</pre></code>
<pre><code>
kubectl get svc -n kubernetes-dashboard
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl get svc -n kubernetes-dashboard
NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)         AGE
dashboard-metrics-scraper   ClusterIP   10.100.12.31     <none>        8000/TCP        7m50s
kubernetes-dashboard        NodePort    10.107.108.167   <none>        443:31121/TCP   7m50s
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ vim nodeport_dashboard_patch.yaml
haeshin@master-ubuntu-2204-k8s:~$ cat nodeport_dashboard_patch.yaml 
spec:
  ports:
  - nodePort: 32000
    port: 443
    protocol: TCP
    targetPort: 8443
</pre></code>
<pre><code>
kubectl -n kubernetes-dashboard patch svc kubernetes-dashboard --patch "$(cat nodeport_dashboard_patch.yaml)"
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl -n kubernetes-dashboard patch svc kubernetes-dashboard --patch "$(cat nodeport_dashboard_patch.yaml)"
service/kubernetes-dashboard patched
</pre></code>
<pre><code>
kubectl get deployments -n kubernetes-dashboard
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl get deployments -n kubernetes-dashboard 
NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
dashboard-metrics-scraper   1/1     1            1           10m
kubernetes-dashboard        1/1     1            1           10m
</pre></code>
<pre><code>
kubectl get pods -n kubernetes-dashboard
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl get pods -n kubernetes-dashboard
NAME                                         READY   STATUS    RESTARTS   AGE
dashboard-metrics-scraper-64bcc67c9c-jvhfv   1/1     Running   0          11m
kubernetes-dashboard-66c887f759-4qdkv        1/1     Running   0          11m
</pre></code>
<pre><code>
kubectl get service -n kubernetes-dashboard 
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ kubectl get service -n kubernetes-dashboard 
NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)         AGE
dashboard-metrics-scraper   ClusterIP   10.100.12.31     <none>        8000/TCP        11m
kubernetes-dashboard        NodePort    10.107.108.167   <none>        443:32000/TCP   11m
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ vim admin-haeshin.yml
haeshin@master-ubuntu-2204-k8s:~$ cat admin-haeshin.yml 
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-haeshin
  namespace: kube-system
haeshin@master-ubuntu-2204-k8s:~$ kubectl apply -f admin-haeshin.yml 
serviceaccount/admin-haeshin created
haeshin@master-ubuntu-2204-k8s:~$ vim admin-haeshin-rbac.yml
haeshin@master-ubuntu-2204-k8s:~$ cat admin-haeshin-rbac.yml 
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-haeshin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: admin-haeshin
    namespace: kube-system
</pre></code>

haeshin@master-ubuntu-2204-k8s:~$ kubectl apply -f admin-haeshin-rbac.yml 
clusterrolebinding.rbac.authorization.k8s.io/admin-haeshin created
haeshin@master-ubuntu-2204-k8s:~$ ls
admin-haeshin-rbac.yml  admin-haeshin.yml  kube-flannel.yml  nodeport_dashboard_patch.yaml  recommended.yaml

haeshin@master-ubuntu-2204-k8s:~$ kubectl create token admin-haeshin -n kube-system
eyJhbGciOiJSUzI1NiIsImtpZCI6ImtBa2lJYWtXQW50Sks0MjUteXFLZTkxRW93b2lwQkIxamxleGt2VDgzeGsifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjY4MDk3Mzc4LCJpYXQiOjE2NjgwOTM3NzgsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJhZG1pbi1oYWVzaGluIiwidWlkIjoiNDk4MTAzNDUtODM2NS00ZGY4LTg2MTMtMTZhNGQ5OTExMjk1In19LCJuYmYiOjE2NjgwOTM3NzgsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTphZG1pbi1oYWVzaGluIn0.aotmASqgx9yxpOrNF8JQgTaJv50LTMNFL4BfXzJBGvwb6tNKmk8h3F55w4gVZt1ZPbW3igqxGMogWKCgvAZkwr6sKzUyJOtPzo3KX7fDKlCsYhsZkRdx5A7zgTdzbyZYBVlYCpmibqrlLaae56QudUF-6tuPOa0kBRAHh815SVC5cT1pDYNVgBMGySqOJXqV9G47T7K47TMP72cW1d6Mjw8RZ_qx9LZZGNzmdTiROKGiAWsk0almzq-1ij5s2ZlYIS8-kano4VEtJU1eP9Yayf4ONviElR6yRUawqQPgg_8VIeOweoZvX7XLExbjRSEz9740cqWNuPRFvWqFLcWzTw



</pre></code>

![image](https://user-images.githubusercontent.com/116150600/201135821-a88dc50a-284c-491a-948c-d9e08c199e7c.png)

![image](https://user-images.githubusercontent.com/116150600/201136093-59f1bb6f-0149-43f8-bb79-3de642f31117.png)
