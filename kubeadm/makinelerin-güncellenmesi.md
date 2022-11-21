
# Makinelerin Güncellenmesi

Kubernetes Cluster kurulumu için gerekli paketler ve bağımlılıklar için Ubuntu sunucularımızın her birinin güncellenmiş ve yükseltilmiş olması gerekiyor.
<pre><code>
sudo apt-get update 
sudo apt-get upgrade -y
</pre></code>
Güncelleme ve yükseltme sonrası sunucuların yeniden başlatılması gerekiyor.
<pre><code>
sudo reboot -f
</pre></code>
