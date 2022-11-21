# kubelet, kubectl ve kubeadm 'in kurulumu

Güncellemenin ardından sunucuları yeniden başlattıysak Kubernetes'in repo'sunu ve gerekli bazı araçları tüm Node'lara ekliyoruz.

<pre><code>
sudo apt install curl apt-transport-https -y
curl -fsSL  https://packages.cloud.google.com/apt/doc/apt-key.gpg|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/k8s.gpg
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
</pre></code>

Güncellemeyi tekrar yapıp gerekli paketleri yüklüyoruz.
<pre><code>
sudo apt update
sudo apt install wget curl vim git kubelet kubeadm kubectl -y
</pre></code>

Link: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

Ardından kubelet, kubeadm ve kubectl için güncellemeleri durduruyoruz.
</pre></code>
haeshin@master-ubuntu-2204-k8s:/proc$ sudo apt-mark hold kubelet kubeadm kubectl
kubelet set on hold.
kubeadm set on hold.
kubectl set on hold.
</pre></code>

Aşağıdaki gibi versiyon kotrolü yapabiliriz
<pre><code>
haeshin@worker-2-ubuntu-2204-k8s:~$ kubectl version --client && kubeadm version
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.3", GitCommit:"434bfd82814af038ad94d62ebe59b133fcb50506", GitTreeState:"clean", BuildDate:"2022-10-12T10:57:26Z", GoVersion:"go1.19.2", Compiler:"gc", Platform:"linux/amd64"}
Kustomize Version: v4.5.7
kubeadm version: &version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.3", GitCommit:"434bfd82814af038ad94d62ebe59b133fcb50506", GitTreeState:"clean", BuildDate:"2022-10-12T10:55:36Z", GoVersion:"go1.19.2", Compiler:"gc", Platform:"linux/amd64"}
</pre></code>


**Kaynek:** https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl


