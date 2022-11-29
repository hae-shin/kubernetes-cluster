# kubelet Nedir ?

Her Node'da kubelet vardır. [apiserver](https://github.com/hae-shin/kubernetes-cluster/blob/main/d%C3%B6k%C3%BCmanlar/kube-apiserver.md)''ı sürekli izler, ondan gelecek emirleri bekler ve onları uygular. Podları oluşturabilmek için Üzerinde çalıştığı Node'un konteyner çalışma ortamıyla haberleşir. Pod'un durumuyla ilgili bilgileri apiserver'a iletir.


![image](https://user-images.githubusercontent.com/116150600/202446806-18d1cf19-1d86-4ddf-83e8-5991497dc914.png)

**Kaynak:** https://kubernetes.io/docs/concepts/overview/components/#kube-proxy
