

# Kubernetes Cluster(*kümelenme*,*salkım*) Nedir ?

Kelime anlamından yola çıkarak açıklayacak olursak; bir üzüm salkımını düşünebilirz. Cluster mimarisinin temelindeki **Node**'lar üzüm tanelerini temsil eder. Node'lar üzerinde çalışan Kubernetes bileşenlerine göre farklı nitelik gösterirler. Üzerinde ***Control Plane***'nin çalıştığı Node'lar ***Master*** olarak nitelenir. İş yükünü üstlenen Node'lara ise ***Worker*** denmiştir. Node'lar da kendi içerisinde ayrıca kümelenir, salkımlanırlar; burada üzüm tanesi temsilini pod'lar karşılar.  Ama en nihayetinde koca bir üzüm salkımı içerisindeki üzüm taneleridir hepsi. Yine görselde belirtildiği üzere tümünün birbiriyle iletişimi vardır; aynı her bir üzüm tanesinin salkımın tümüne bağlayan organik bağın sağladığı iletişim gibi. Bu iletişim çok yönlüdür Control Plane'den Node'lara, Node'lardan Pod'lara, Pod'lardan COntrol Plane'e, API Server'dan Service'lere vb.

![image](https://user-images.githubusercontent.com/116150600/201916850-0030bbd4-139f-4b56-9bba-4b1f85ca9c27.png)






