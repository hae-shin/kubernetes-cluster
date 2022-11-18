# Host Dosyalarının Düzenlenmesi

Her Node için ayrı ayrı **/etc/hosts** dosyası düzenlenmeli Node'ların hostname üzerinden birbirine erişebilmeleri için aşağıdaki üç satır dosyanın sonuna eklenmelidir.

<pre><code>
sudo vi /etc/hosts
</pre></code>

<pre><code>
192.168.1.25 master-ubuntu-2204-k8s
192.168.1.26 worker-1-ubuntu-2204-k8s
192.168.1.27 worker-2-ubuntu-2204-k8s
</pre></code>

Değişiklikten sonra ping komutu ile kontrol edilebilir.

<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ ping worker-1-ubuntu-22.04-k8s
PING worker-1-ubuntu-22.04-k8s (192.168.1.26) 56(84) bytes of data.
64 bytes from worker-1-ubuntu-22.04-k8s (192.168.1.26): icmp_seq=1 ttl=64 time=0.630 ms
64 bytes from worker-1-ubuntu-22.04-k8s (192.168.1.26): icmp_seq=2 ttl=64 time=0.784 ms
64 bytes from worker-1-ubuntu-22.04-k8s (192.168.1.26): icmp_seq=3 ttl=64 time=0.565 ms
^C
--- worker-1-ubuntu-22.04-k8s ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2023ms
rtt min/avg/max/mdev = 0.565/0.659/0.784/0.091 ms
haeshin@master-ubuntu-2204-k8s:~$ ping worker-2-ubuntu-22.04-k8s
PING worker-2-ubuntu-22.04-k8s (192.168.1.27) 56(84) bytes of data.
64 bytes from worker-2-ubuntu-22.04-k8s (192.168.1.27): icmp_seq=1 ttl=64 time=0.922 ms
64 bytes from worker-2-ubuntu-22.04-k8s (192.168.1.27): icmp_seq=2 ttl=64 time=0.659 ms
64 bytes from worker-2-ubuntu-22.04-k8s (192.168.1.27): icmp_seq=3 ttl=64 time=0.773 ms
^C
--- worker-2-ubuntu-22.04-k8s ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2506ms
rtt min/avg/max/mdev = 0.659/0.784/0.922/0.107 ms
</pre></code>
