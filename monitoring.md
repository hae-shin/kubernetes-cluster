# Monitoring Kubernetes Cluster

Kubernetes Cluster'ın monitor edilmesi önemli bir başlık. Cluster'ın belirli bir süre aralığındaki genel durumunu izleyebilmek mevcut sorunları çözmek ve ilerisi için plan yapabilmek açısından neredeyse zorunludur diyebiliriz. Monitoring aracı olarak ise Prometheus, Grafana ve Alertmanager hakim çözüm konumda. Biz ise burada daha önce kubeadm ile kurulumunu yaptığımız cluster'ı bu monitoring araçlarıyla nasıl izleriz sorusunu cevaplandıracağız.

- **Prometheus:** Belirli zaman aralıklarında Kubernetes Cluster'dan metric'ler toplar. Bu metrikler işlemci, bellek, disk performansı, network vb. başlıklarda bilgi içerir.
- **Grafana:** Prometheus'un topladığı bilgileri görselleştirir.
- **Alertmanager:** Prometheus tarafında hazırlanmış kurallara göre oluşan alarmların yönetiminde kullanılır.



![image](https://user-images.githubusercontent.com/116150600/203777938-1bb0dd5a-6bc4-44e1-8226-96fec86ce0e5.png)

 - **kube-prometheus** ise bir Kubernetes Cluster'ı izlemek için hazırlanmış komple bir çözüm olarak github'da repo olarak yer almaktadır.

<pre><code>
git clone https://github.com/prometheus-operator/kube-prometheus.git
</pre></code>
-  İlgili dizine gidip dosyaları listeliyoruz.
<pre><code>
cd kube-prometheus
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ ls
build.sh            CONTRIBUTING.md      example.jsonnet  go.mod   jsonnetfile.json           kustomization.yaml  manifests   scripts
CHANGELOG.md        developer-workspace  examples         go.sum   jsonnetfile.lock.json      LICENSE             README.md   tests
code-of-conduct.md  docs                 experimental     jsonnet  kubescape-exceptions.json  Makefile            RELEASE.md
</pre></code>
- manifests/setup dizini altında bir takım **yaml** dosyası bulunuyor. Gerekli CustomResourceDefination'ları oluşturabilmek için bu dosyaların tümünü uygulamaya alıyoruz. Ayrıca monitoring araçlarının çalışacağı namespace'i oluşturuyoruz.
<pre><code>
kubectl create -f manifests/setup
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ kubectl create -f manifests/setup
customresourcedefinition.apiextensions.k8s.io/alertmanagerconfigs.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/alertmanagers.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/podmonitors.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/probes.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/prometheuses.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/prometheusrules.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/servicemonitors.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/thanosrulers.monitoring.coreos.com created
namespace/monitoring created
</pre></code>
- monitoring namespace'ini görüntülüyoruz.
<pre><code>
kubectl get ns monitoring
</pre></code>

<pre><code>
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ kubectl get ns monitoring
NAME         STATUS   AGE
monitoring   Active   76s
</pre></code>
- Ardından manifest dizini altında **yaml** olarak bulunan monitoring araçlarını (Prometheus, Grafana, Alertmanager) ve bu araçların çalışması için gerekli kubernetes nesnelerini(configmap, service, networkpolicy vb.) oluşturuyoruz.
<pre><code>
kubectl create -f manifests/
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ kubectl create -f manifests/
alertmanager.monitoring.coreos.com/main created
networkpolicy.networking.k8s.io/alertmanager-main created
poddisruptionbudget.policy/alertmanager-main created
prometheusrule.monitoring.coreos.com/alertmanager-main-rules created
secret/alertmanager-main created
service/alertmanager-main created
serviceaccount/alertmanager-main created
servicemonitor.monitoring.coreos.com/alertmanager-main created
clusterrole.rbac.authorization.k8s.io/blackbox-exporter created
clusterrolebinding.rbac.authorization.k8s.io/blackbox-exporter created
configmap/blackbox-exporter-configuration created
deployment.apps/blackbox-exporter created
networkpolicy.networking.k8s.io/blackbox-exporter created
service/blackbox-exporter created
serviceaccount/blackbox-exporter created
servicemonitor.monitoring.coreos.com/blackbox-exporter created
secret/grafana-config created
secret/grafana-datasources created
configmap/grafana-dashboard-alertmanager-overview created
configmap/grafana-dashboard-apiserver created
configmap/grafana-dashboard-cluster-total created
configmap/grafana-dashboard-controller-manager created
configmap/grafana-dashboard-grafana-overview created
configmap/grafana-dashboard-k8s-resources-cluster created
configmap/grafana-dashboard-k8s-resources-namespace created
configmap/grafana-dashboard-k8s-resources-node created
configmap/grafana-dashboard-k8s-resources-pod created
configmap/grafana-dashboard-k8s-resources-workload created
configmap/grafana-dashboard-k8s-resources-workloads-namespace created
configmap/grafana-dashboard-kubelet created
configmap/grafana-dashboard-namespace-by-pod created
configmap/grafana-dashboard-namespace-by-workload created
configmap/grafana-dashboard-node-cluster-rsrc-use created
configmap/grafana-dashboard-node-rsrc-use created
configmap/grafana-dashboard-nodes-darwin created
configmap/grafana-dashboard-nodes created
configmap/grafana-dashboard-persistentvolumesusage created
configmap/grafana-dashboard-pod-total created
configmap/grafana-dashboard-prometheus-remote-write created
configmap/grafana-dashboard-prometheus created
configmap/grafana-dashboard-proxy created
configmap/grafana-dashboard-scheduler created
configmap/grafana-dashboard-workload-total created
configmap/grafana-dashboards created
deployment.apps/grafana created
networkpolicy.networking.k8s.io/grafana created
prometheusrule.monitoring.coreos.com/grafana-rules created
service/grafana created
serviceaccount/grafana created
servicemonitor.monitoring.coreos.com/grafana created
prometheusrule.monitoring.coreos.com/kube-prometheus-rules created
clusterrole.rbac.authorization.k8s.io/kube-state-metrics created
clusterrolebinding.rbac.authorization.k8s.io/kube-state-metrics created
deployment.apps/kube-state-metrics created
networkpolicy.networking.k8s.io/kube-state-metrics created
prometheusrule.monitoring.coreos.com/kube-state-metrics-rules created
service/kube-state-metrics created
serviceaccount/kube-state-metrics created
servicemonitor.monitoring.coreos.com/kube-state-metrics created
prometheusrule.monitoring.coreos.com/kubernetes-monitoring-rules created
servicemonitor.monitoring.coreos.com/kube-apiserver created
servicemonitor.monitoring.coreos.com/coredns created
servicemonitor.monitoring.coreos.com/kube-controller-manager created
servicemonitor.monitoring.coreos.com/kube-scheduler created
servicemonitor.monitoring.coreos.com/kubelet created
clusterrole.rbac.authorization.k8s.io/node-exporter created
clusterrolebinding.rbac.authorization.k8s.io/node-exporter created
daemonset.apps/node-exporter created
networkpolicy.networking.k8s.io/node-exporter created
prometheusrule.monitoring.coreos.com/node-exporter-rules created
service/node-exporter created
serviceaccount/node-exporter created
servicemonitor.monitoring.coreos.com/node-exporter created
clusterrole.rbac.authorization.k8s.io/prometheus-k8s created
clusterrolebinding.rbac.authorization.k8s.io/prometheus-k8s created
networkpolicy.networking.k8s.io/prometheus-k8s created
poddisruptionbudget.policy/prometheus-k8s created
prometheus.monitoring.coreos.com/k8s created
prometheusrule.monitoring.coreos.com/prometheus-k8s-prometheus-rules created
rolebinding.rbac.authorization.k8s.io/prometheus-k8s-config created
rolebinding.rbac.authorization.k8s.io/prometheus-k8s created
rolebinding.rbac.authorization.k8s.io/prometheus-k8s created
rolebinding.rbac.authorization.k8s.io/prometheus-k8s created
role.rbac.authorization.k8s.io/prometheus-k8s-config created
role.rbac.authorization.k8s.io/prometheus-k8s created
role.rbac.authorization.k8s.io/prometheus-k8s created
role.rbac.authorization.k8s.io/prometheus-k8s created
service/prometheus-k8s created
serviceaccount/prometheus-k8s created
servicemonitor.monitoring.coreos.com/prometheus-k8s created
apiservice.apiregistration.k8s.io/v1beta1.metrics.k8s.io created
clusterrole.rbac.authorization.k8s.io/prometheus-adapter created
clusterrole.rbac.authorization.k8s.io/system:aggregated-metrics-reader created
clusterrolebinding.rbac.authorization.k8s.io/prometheus-adapter created
clusterrolebinding.rbac.authorization.k8s.io/resource-metrics:system:auth-delegator created
clusterrole.rbac.authorization.k8s.io/resource-metrics-server-resources created
configmap/adapter-config created
deployment.apps/prometheus-adapter created
networkpolicy.networking.k8s.io/prometheus-adapter created
poddisruptionbudget.policy/prometheus-adapter created
rolebinding.rbac.authorization.k8s.io/resource-metrics-auth-reader created
service/prometheus-adapter created
serviceaccount/prometheus-adapter created
servicemonitor.monitoring.coreos.com/prometheus-adapter created
clusterrole.rbac.authorization.k8s.io/prometheus-operator created
clusterrolebinding.rbac.authorization.k8s.io/prometheus-operator created
deployment.apps/prometheus-operator created
networkpolicy.networking.k8s.io/prometheus-operator created
prometheusrule.monitoring.coreos.com/prometheus-operator-rules created
service/prometheus-operator created
serviceaccount/prometheus-operator created
servicemonitor.monitoring.coreos.com/prometheus-operator created
</pre></code>
- monitoring namespace'indeki podları görüntülemek için;
<pre><code>
kubectl get pods -n monitoring
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ kubectl get pods -n monitoring
NAME                                   READY   STATUS    RESTARTS      AGE
alertmanager-main-0                    2/2     Running   1 (57s ago)   79s
alertmanager-main-1                    2/2     Running   1 (57s ago)   79s
alertmanager-main-2                    2/2     Running   1 (57s ago)   79s
blackbox-exporter-59cccb5797-2fphc     3/3     Running   0             3m33s
grafana-76d67c8b47-j852d               1/1     Running   0             3m31s
kube-state-metrics-6d68f89c45-9k4xg    3/3     Running   0             3m31s
node-exporter-hv9rg                    2/2     Running   0             3m31s
node-exporter-r9k9s                    2/2     Running   0             3m30s
node-exporter-zkgmw                    2/2     Running   0             3m30s
prometheus-adapter-757f9b4cf9-ffn9n    1/1     Running   0             3m29s
prometheus-adapter-757f9b4cf9-l6x7f    1/1     Running   0             3m29s
prometheus-k8s-0                       2/2     Running   0             75s
prometheus-k8s-1                       1/2     Running   0             74s
prometheus-operator-67f59d65b8-6zzxr   2/2     Running   0             3m29s
</pre></code>
- monitoring namespace'indeki service'leri görüntülemek için;
<pre><code>
kubectl get svc -n monitoring
</pre></code>
<pre><code>
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ kubectl get svc -n monitoring
NAME                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
alertmanager-main       ClusterIP   10.108.222.85    <none>        9093/TCP,8080/TCP            9m1s
alertmanager-operated   ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP   6m47s
blackbox-exporter       ClusterIP   10.96.187.142    <none>        9115/TCP,19115/TCP           9m1s
grafana                 ClusterIP   10.109.185.203   <none>        3000/TCP                     9m
kube-state-metrics      ClusterIP   None             <none>        8443/TCP,9443/TCP            8m59s
node-exporter           ClusterIP   None             <none>        9100/TCP                     8m59s
prometheus-adapter      ClusterIP   10.108.151.237   <none>        443/TCP                      8m57s
prometheus-k8s          ClusterIP   10.99.223.82     <none>        9090/TCP,8080/TCP            8m58s
prometheus-operated     ClusterIP   None             <none>        9090/TCP                     6m43s
prometheus-operator     ClusterIP   None             <none>        8443/TCP                     8m57s
</pre></code>
- Uygulamalarımızı dışarıdan erişilebilir hale getirmek için service'lerin type'ını NodePort olarak belirliyoruz;
<pre><code>
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ kubectl --namespace monitoring patch svc prometheus-k8s -p '{"spec": {"type": "NodePort"}}'
service/prometheus-k8s patched
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ kubectl --namespace monitoring patch svc alertmanager-main -p '{"spec": {"type": "NodePort"}}'
service/alertmanager-main patched
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ kubectl --namespace monitoring patch svc grafana -p '{"spec": {"type": "NodePort"}}'
service/grafana patched
</pre></code>
- NodePort ile dışarı açılmış service'leri ve hangi port'ta yayın yaptıklarını görüntülemek için;
<pre><code>
haeshin@master-ubuntu-2204-k8s:~/kube-prometheus$ kubectl -n monitoring get svc  | grep NodePort
alertmanager-main       NodePort    10.108.222.85    <none>        9093:31426/TCP,8080:31696/TCP   16m
grafana                 NodePort    10.109.185.203   <none>        3000:32291/TCP                  16m
prometheus-k8s          NodePort    10.99.223.82     <none>        9090:32335/TCP,8080:31338/TCP   16m
</pre></code>



![image](https://user-images.githubusercontent.com/116150600/201350278-1fba01db-5abc-487b-8060-5f28d528c2eb.png)

- Grafana;


![image](https://user-images.githubusercontent.com/116150600/201356736-37ab76d7-8f48-4e83-8b06-083aaf3d55fc.png)

- Prometheus;

![image](https://user-images.githubusercontent.com/116150600/201357013-acb0ab02-2137-4856-8222-e74d074b0570.png)

- Alertmanager;

![image](https://user-images.githubusercontent.com/116150600/201357183-9d7fd1cc-6b98-4fed-a739-6600a065407a.png)

- Grafana'ya yeni bir Dashboard'eklemek için aşağıdaki seçenek ile dashboard import edebiliyoruz.

![image](https://user-images.githubusercontent.com/116150600/201360900-f1a1566c-a340-465e-8158-c4e82e13cee7.png)

- Aşağıdaki link'te hazır bir Dashboard olan "Node Exporter Full" dashboard'unun ID'sini görüntüleyebilir bu ID ile dashboard'ı source'unu Prometheus olarak belirleyip import edebiliriz.

**Link**: https://grafana.com/grafana/dashboards/1860-node-exporter-full/

- Dashboard'da her bir Node özelinde grafikleri görüntüleyebiliriz. Söz konusu dashboard Node'ların (CPU, RAM, Disk, Network Trafiği vb.) gibi Prometheus tarafından toplanan bilgilerini grafikleştiriyor.

![image](https://user-images.githubusercontent.com/116150600/201361195-79ec12b9-8079-4265-816d-2e2df223af76.png)

