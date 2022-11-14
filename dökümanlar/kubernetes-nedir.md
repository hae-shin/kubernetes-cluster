
# Kubernetes Nedir ?

Kubernetes (k8s) Google tarafından geliştirilen bir projedir ve açık kaynaktır. Kubernetes'in temel işlevi konteyner'da çalışmak üzere geliştirilen uygulamaları orkestra etmektir. Konteyner teknolojisini ve orkestra etmek işlevini anlatmadan önce uygulama teknolojilerindeki gelişim süreçlerinden bahsedelim ve Kubernetes teknolojisin bu gelişim sürecine nasıl dahil olduğunu anlatalım.

![image](https://user-images.githubusercontent.com/116150600/201118995-c5b4283f-97ef-4284-99f1-47bc9709b7ab.png)

Yukarıdaki görselde uygulamaların deploy edilme süreçlerindeki teknik değişiklikleri geleneksel(*traditional*), sanallaştırılmış(*virtualizel*) ve konteyner(*container*) olmak üzere çağlara ayrıldığını görüyoruz. Bu değişiklik teknolojinin genel ilerleyişinde altyapı, otomasyon ve güvenlik vb. gibi başlıklarda çok ciddi farklılıklar yaratmıştır. Kubernetesin temelleri ve varlık koşulu konteyner teknolojisine bağlıdır. 

- **Geleneksel Çağ:** Görselde açık biçimde belirtildiği üzere bir donanım o donanımı yönettiğimiz işlteim sistemi, işletim sisteminin üzerinde de bir veya daha fazla olmak üzere uygulamalar bulunuyor. Burada uygulamaların donanımın ve işletim sisteminin değişkenlerine bağımlı olduğu bir durum söz konusu. Örneğin tek fiziksel ortamda birden fazla uygulama çalıştırmak istediğinizde uygulamaların kaynak kullanımı birbirleri alehine çakışabiliyor. Bu durumda yapılacak tek şey uygulamalardan birini yeni bir fiziksel ortama aktarmak oluyor. Fakat bu hem donanım hem insan emeğinin kullanımı açısından korkunç bir verimsizlik ve angarya yaratıyor.

- **Sanallaştırma Çağı:** Yine görselden hareketle sanallaştırma teknolojisinin geliştirilmesiyle hypervisor aracılığıyla donanım kaynaklarının bölümlendirilebildiğini bu sayede tek fiziksel makinede birden fazla sanal makinenin oluşturulabildiğini ve bu makineler üzerinde birden fazla uygulamanın çalıştırılabildiğini görüyoruz. Bu hem fizkiksel hem insan kaynağını kullanımı açısından çok ciddi bir teknolojik ilerleme olsa da hala uygulama merkezli bir mimari geliştirememiş oluyor. Uygulamalar bu sayede fiziksel makinenin belirleyiciliğinden sanal makineler araclığıyla kurtulmuş olsa da işletim sistemi tarafındaki bağımlılıklara hala takılıyorlar. Örneğin aynı sanal birden fazla uygulama çalışması gerekiyor fakat uygulamalardan birisi Python 2.0 ile çalışırken diğeri Python 3.10 sürümüyle çalışıyor ya da en başta aynı PHP versiyonlarıyla çalışan uygulamalardan biri zaman içinde yapılan geliştirmelerle PHP'nin başka bir versiyonuna ihtiyaç duyuyor. Durum bu şekilde olduğunda hiç gerekmediği halde yeni bir sanal makine oluşturup uygulamayı oraya taşımanız gerekbilir. Bu durum zamanla yönetilebilir olmaktan çıkar ve verimsizlik burada da başgösterir.

- **Konteyner Çağı:** Burada ise karşımıza **Conteyner Runtime** kavramı çıkıyor. Konteyner çalışma ortamı olarak Türkçe'leştirilebilecek bu kavram uygulamalar için izole edilmiş alanlar yaratır. Bu alan işletim sistemi tarafında basit bir sistem process'si gibi çalışan konteynerlerdir. Konteynerler sayesinde aynı fiziksel ve aynı sanal makine üzerinde işletim sistemi tarafındaki hiçbir bağımlılığa takılmadan uygulamalarımızı kendi bağımlılıkları eşliğinde çalıştırabiliriz. Bir önceki örnekten yola çıkarak uygulamanızı belirli bir MySQL sürümüyle konteyner içerisinde çalıştırırken diğerini başka bir sürümle başka bir konteyner içinde çalıştırabilirsiniz ve bu iki konteyner içinde olup bitenin diğer konteynere hiçbir etkisi yoktur. Bu hem insan emeği açısından ciddi kolaylıklar yaratır hemde mevcut sistem kaynaklarınızı max verimlilikle kullanmanızı sağlar. Çünkü konteyner basit bir Linux process'idir aslında, fazlası değil.

- **Kubernetes** ise *production* aşamasında devreye giriyor ve uygulamaların *downtime*'sız, olabildiğince düşük sistem kaynağı ve insan emeği kullanımıyla çalıştırabilmeyi sağlıyor. Konteynerlerin orkestrasyonunu ve konteynerlerde çalışan uygulamaların aşağıda örneğini verdiğimiz birçok ihtiyacının karşılanmasında otomasyon ve güvenli yönetilebilirlik sağlıyor.


## Kubernetes'in Sağladıkları

- Kubernetes ***Self-Healing*** sağlar. Eğer konteynerlerinizden birisi ölürse kubernetes ortamı hemen tekrardan yenisi oluşturuyor.

- Kubernetes ***Horizontal Scaling*** sağlar. Mevcut kaynak durumunuza göre konteynerlerin sayısını arttırıp azaltabiliyor.

- Kubernetes ***Compute Scheduling*** sağlar. Mevcut kaynak ihtiyacının belirlenmesini ve konteyner özelinde kaynakların atanmasını otomatikleştirir.

- Kubernetes ***LoadBalance*** sağlar. Mevcut uygulamalar üzerindeki yükü üzerinde çalıştıkları konteynerler arasında dağıtır.

- Kubernetes ***Nameserver*** sağlar. Mevcut konteynerlere erişebilmek için DNS tanımları yapar.

- Kubernetes otomatik ***Rollout/Rollback*** sağlar. Kubernetes her yeni deployment'da veya mevcut deployment'daki her değişiklikte önceden belirlenmiş desired duruma göre otomatik olarak bütün instance'ları ve bileşenleri yeniden oluşturur. Deployment sırasında bir sorun yaşandığında ise otomatik olarak bir önceki sorunsuz çalışan deployment'a geri çekilir.

- Kubernetes gizli bilgilerin ve yapılandırma ayarlarının yönetimini sağlar. Uygulama yapaılandırma ayarları, parolalar, API anahtarları, sertifikalar vb bilgilerin yönetimini otomatikleştirir.

- Kubernetes ***Volume Management*** sağlar. Uygulamaların kullanımına göre depolama ihtiyaçlarını karşılar ve otomatikleştirir.

**Kaynak:** https://kubernetes.io/docs/concepts/overview/
