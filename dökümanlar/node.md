
# Node Nedir ?

Node'lar Control Plane tarafından yönetilirler. Uygulamalar üzerinde koştuğu konteyner çalışma ortamını sağlarlar. İş yükünü üstlenen Node'lara Worker Node denir. Control Plane de bir Node üzerinde çalışır. Cluster ölçeğindeki yönetici rolü göz önünde bulundurularak üzerinde çalıştığı Node'un Master ünvanı almasını sağlar. 

![kubelet](https://github.com/hae-shin/kubernetes-cluster/blob/main/d%C3%B6k%C3%BCmanlar/kubelet.md) ve ![kubeproxy](https://github.com/hae-shin/kubernetes-cluster/blob/main/d%C3%B6k%C3%BCmanlar/kube-proxy.md) bileşenlerini bünyesinde barındırır.

- ***Master Node:*** Worker Node'lardan, service'lerden, pod'lardan ve diğer bileşenlerden gelen API isteklerini karşılar, yönetir ve gereğini yapar. Cluster'ın beynidir. Control Plane olarak etcd, controller-manager ve scheduler 'ı çalıştırır.

- ***Woker Node:*** Konteynerlerin çalıştıkları ortamdır. Aynı zamanda Control Plane'e üzerinde çalışan konteynerler ve güncel durum hakkında sürekli bilgi verir. kubectl ve kubeproxy aracılığı ile API server'la haberleşir.



![image](https://user-images.githubusercontent.com/116150600/202446806-18d1cf19-1d86-4ddf-83e8-5991497dc914.png)
