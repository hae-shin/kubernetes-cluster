## swap Alanının Devredışı Bırakılması

Aşağıdaki komutla tüm Node'larda swap alanını devredışı bırakmamız gerekiyor.
<pre><code>
sudo swapoff -a 
</pre></code>
Yine aşağıdaki komutla swap alnının sıfırlanıp sıfırlanmadığını kontrol edebilirsiniz.
<pre><code>
haeshin@master-ubuntu-2204-k8s:/proc$ free -h
               total        used        free      shared  buff/cache   available
Mem:           2.9Gi       186Mi       2.5Gi       1.0Mi       268Mi       2.6Gi
Swap:             0B          0B          0B
</pre></code>
Yapılan değişikliği kalıcılaştırmak için /etc/fstab dosyasını düzenleyip swap satırını yoruma almalıyız.
<pre><code>
haeshin@master-ubuntu-2204-k8s:~$ sudo cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/ubuntu-vg/ubuntu-lv during curtin installation
/dev/disk/by-id/dm-uuid-LVM-rTj4l11pVn7sbKJOHXKBwFacO9dABZFAuSpWPP2y0YtYDGdfAUrgNlhsBdInWKmA / ext4 defaults 0 1
# /boot was on /dev/sda2 during curtin installation
/dev/disk/by-uuid/a8ad5fbe-816a-4d75-ad98-7710f1bacedf /boot ext4 defaults 0 1
/swap.img       none    swap    sw      0       0
 </pre></code>

**Kaynak:** https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#before-you-begin
