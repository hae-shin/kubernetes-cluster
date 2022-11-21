# sysctl'in Yapılandırılması

/etc/sysctl.d/kubernetes.conf dosyasının oluşturup aşağıdaki satırları ekliyoruz.
<pre><code>
sudo vi /etc/sysctl.d/kubernetes.conf
</pre></code>
<pre><code>
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
</pre></code>
Ardından değişiklikleri görebilmesi için sysctl'i reload ediyoruz.
<pre><code>
sudo sysctl --system
</pre></code>
