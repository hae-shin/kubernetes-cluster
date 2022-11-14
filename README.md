# Kubernetes Cluster

![image](https://github.com/hae-shin/kubernetes-cluster/blob/main/kubernetes.png)

### Merhabalar

> Bu makalede ***Kubernetes Cluster*** kavramı üzerine eğilip Kubernetes'in neler sağladığından Kubernetes'in temel belişenlerinden bahsedeceğiz. *kubeadm* ve *kubespray* gibi araçlarla kurulumunu gerçekleştirip nasıl monitor edildiğini uygulamalı olarak göstereceğiz. 

# Başlıklar

<!-- docs/_sidebar.md -->

* Başlangıç
    * [Kubernetes Nedir ?](./dökümanlar/kubernetes-nedir.md)
    * [Kubernetes Cluster Nedir ?](./dökümanlar/kubernetes-cluster-nedir.md)

* Kubernetes Bileşenleri
    * [Control Plane Bileşenleri](./dökümanlar/control-plane.md)
        * [api-server](./dökümanlar/api-server.md)
        * [etcd](./dökümanlar/control-plane?id=etcd.md)
        * [controller-manager](./dökümanlar/controller-manager.md)
        * [scheduler](./dökümanlar/scheduler.md)
    * [Node Bileşenleri](./dökümanlar/worker-node.md)
        * [kubelet](./dökümanlar/kubelet.md)
        * [kube-proxy](./dökümanlar/kube-proxy.md)
* Cluster Kurulumu
    * [**kubeadm**](./kubeadm/kubeadm.md)
    * [**monitoring**](./kubeadm/monitoring.md)
    * [**kubespray**](./kubespray/kubespray.md)
