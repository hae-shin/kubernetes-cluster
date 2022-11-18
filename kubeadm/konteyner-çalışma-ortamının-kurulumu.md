# Konteyner Çalışma Ortamının Kurulumu

Pod'ların içerisinde konteynerlerin çalışabilmesi için tüm Node'larda konteyner çalışma ortamı kurulmalıdır. Üç farklı seçeneğimiz bulunuyor;

- Docker
- CRI-O
- Containerd

Biz Containerd seçeneği ile devam edeceğiz kurulumumuza.

## Modül Yüklemelerinin Kalıcılaştırıyoruz.

/etc/modules-load.d/k8s.conf dosyasını oluşturup. İçerisine aşağıdaki satırları ekliyoruz.
<pre><code>
sudo vi /etc/modules-load.d/k8s.conf
</pre></code>

<pre><code>
overlay
br_netfilter
</pre></code>

## Gerekli Paketleri Yüklüyoruz.
<pre><code>
sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
</pre></code>

## Docker Repository'sini ekliyoruz.
<pre><code>
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
</pre></code>
# Containerd'yi kuruyoruz.
<pre><code>
sudo apt update
sudo apt install -y containerd.io
</pre></code>

## Containerd yapılandırma ayarlarını oluşturuyoruz.
<pre><code>
sudo su -
mkdir -p /etc/containerd
containerd config default>/etc/containerd/config.toml
</pre></code>

## Containerd'yi yeniden başlatalım.

<pre><code>
sudo systemctl restart containerd
sudo systemctl enable containerd
systemctl status containerd
</pre></code>

![image](https://user-images.githubusercontent.com/116150600/200186159-9c11626f-6cd8-4725-bb36-36eca5dab4d1.png)

## cgroup driver

systemd cgroup driver'ını kullanmak istiyorsak /etc/containerd/config.toml dosyamıza aşağıdaki satırı eklememiz gerekiyor.
<pre><code>
          [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
            BinaryName = ""
            CriuImagePath = ""
            CriuPath = ""
            CriuWorkPath = ""
            IoGid = 0
            IoUid = 0
            NoNewKeyring = false
            NoPivotRoot = false
            Root = ""
            ShimCgroup = ""
            SystemdCgroup = true
</pre></code>
