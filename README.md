# Kubernetes Cluster

![image](https://github.com/hae-shin/kubernetes-cluster/blob/main/kubernetes.png)

### Merhabalar

> Bu makalede ***Kubernetes Cluster*** kavramı üzerine eğilip Kubernetes'in neler sağladığından Kubernetes'in temel belişenlerinden bahsedeceğiz. *kubeadm* ve *kubespray* gibi araçlarla cluster'ın kurulumunu gerçekleştirip nasıl monitor edildiğini uygulamalı olarak göstereceğiz. 

# Başlıklar

<!-- docs/_sidebar.md -->

* Başlangıç
    * [Kubernetes Nedir ?](./dökümanlar/kubernetes-nedir.md)
    * [Kubernetes Cluster Nedir ?](./dökümanlar/kubernetes-cluster-nedir.md)

* Kubernetes Bileşenleri
    * [Control Plane ve Bileşenleri](./dökümanlar/control-plane.md)
        * [kube-apiserver](./dökümanlar/kube-apiserver.md)
        * [etcd](./dökümanlar/etcd.md)
        * [kube-scheduler](./dökümanlar/kube-scheduler.md)
        * [kube-controller-manager](./dökümanlar/kube-controller-manager.md)
        * [cloud-controller-manager](./dökümanlar/cloud-controller-manager.md)
    * [Node ve Bileşenleri](./dökümanlar/node.md)
        * [kubelet](./dökümanlar/kubelet.md)
        * [kube-proxy](./dökümanlar/kube-proxy.md)
* Cluster Kurulumu
    * [**kubeadm**](./kubeadm/kubeadm.md)
       * [**Gerekli Sistem Kaynakları**](./kubeadm/gereklisistemkaynakları.md)
       * [**Cluster Yapısı**](./kubeadm/clusteryapısı.md)
       * [**Host Dosyalarının Düzenlenmesi](./kubeadm/hostdosyalarınındüzenlenmesi.md)
    * [**monitoring**](./kubeadm/monitoring.md)
    * [**kubespray**](./kubespray/kubespray.md)
