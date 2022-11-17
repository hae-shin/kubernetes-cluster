# kube-scheduler Nedir ?

Kelime anlamından yola çıkarak açıklayalım. ***schedular*** bileşeni planlama yapar. Örneğin yeni bir uygulama deploy edildi, bu uygulamanın hangi Node veya Node'lar üzerinde çalışacağını planlayıp karar veren bileşen kube-scheduler'dır. Bu kararı uygulamanın talep ettiği kaynağı(**CPU** ve **RAM**) ve Node'ların mevcut kaynak yükünü referans alarak verir. Bunun yanında uygulama **nodeSelector**, **affinity** gibi kurallar aracılığıyla spesifik bir Node'da çalıştırılmak istenmiş olabilir bu talepleri de yine kube-scheduler karşılar.

![image](https://github.com/hae-shin/kubernetes-cluster/blob/main/kubernetes-cluster.png)


**Kaynak:** https://kubernetes.io/docs/concepts/overview/components/
