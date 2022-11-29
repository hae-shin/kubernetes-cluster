## Control Plane'nin başlatılması (Master Node'da)

İlk olarak br_netfilter module'unun duğru biçimde yüklenip yüklenmediğini kontrol edelim.
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ lsmod | grep br_netfilter
br_netfilter           32768  0
bridge                307200  1 br_netfilter
</pre></code>
Ardından kubelet servisini aktifleştirelim.
<pre><code>
sudo systemctl enable kubelet
</pre></code>
Sonraki aşamada ise Control Plane'nin bileşenlerini(etcd, kube-apiserver vb.) kubeadm ile çekiyoruz.
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ sudo kubeadm config images pull
[config/images] Pulled registry.k8s.io/kube-apiserver:v1.25.3
[config/images] Pulled registry.k8s.io/kube-controller-manager:v1.25.3
[config/images] Pulled registry.k8s.io/kube-scheduler:v1.25.3
[config/images] Pulled registry.k8s.io/kube-proxy:v1.25.3
[config/images] Pulled registry.k8s.io/pause:3.8
[config/images] Pulled registry.k8s.io/etcd:3.5.4-0
[config/images] Pulled registry.k8s.io/coredns/coredns:v1.9.3
</pre></code>
Eğer birden fazla cri socket bulunuyorsa "--cri-socket" parametresi ile kullanmak istediğinizi belirtmelisiniz.
<pre><code>
sudo kubeadm config images pull --cri-socket /var/run/crio/crio.sock
sudo kubeadm config images pull --cri-socket /run/containerd/containerd.sock
sudo kubeadm config images pull --cri-socket /run/cri-dockerd.sock 
</pre></code>

kubeadm ile cluster kurulumunu başlatırken bazı parametreler kullanabiliriz aşağıdaki gibi; gerçekleştireceğiz.

- **--control-plane-endpoint :**  control-plane'e bağlı tüm Node'lar için ortak bir endpoint oluştur DNS ihtiyacını karşılar.
- **--pod-network-cidr :** CIDR üzerinde pod'lar için network oluşturur.
- **--cri-socket :** Eğer birden fazla konteyner çalışma ortamı kullanıyorsanız onlardan birini seçmenizi sağlar.
- 
Biz burada sadece tek bir parametre kullanarak kurulumu başlatıyoruz.
<pre><code>
sudo kubeadm init \
  --pod-network-cidr=10.244.0.0/16
</pre></code>
Aşağıdaki çıktı cluster kurulumunun başarıyla sonuçlandığını gösteriyor.
<pre><code>
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.1.25:6443 --token rxlytp.m963obz7840h3mx4 \
        --discovery-token-ca-cert-hash sha256:2f0ee2ed94a0fc8b8679a7b6225f0b622182a145a12a3c95d21423f1971a533d 
</pre></code>
Yukarıdaki çıktıda bize söylediği gibi .kube gizli dosyasını oluşturup içerisine config'imizi atıyoruz
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ sudo cp -f /etc/kubernetes/admin.conf $HOME/.kube/config^C
haeshin@master-ubuntu-2204-k8s:~$ mkdir -p $HOME/.kube
haeshin@master-ubuntu-2204-k8s:~$ sudo cp -f /etc/kubernetes/admin.conf $HOME/.kube/config
haeshin@master-ubuntu-2204-k8s:~$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
haeshin@master-ubuntu-2204-k8s:~$ kubectl cluster-info
Kubernetes control plane is running at https://192.168.1.25:6443
CoreDNS is running at https://192.168.1.25:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
</pre></code>
