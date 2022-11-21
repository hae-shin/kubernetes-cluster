# Cluster Yapısı

İşletim sistemi olarak **Ubuntu 22.04 LTS** 'i seçtik.

Burada bir **Master** iki **Worker** olmak üzere 3 adet Node kullanacağız.

| Sunucu Rolleri | Sunucu Hostname'leri      | Kaynaklar      |	IP Adresi    |
| -------------- | --------------------      | ---------      | ---------    |
| Master Node	   | master-ubuntu-22.04-k8s	 | 3GB RAM, 2VCPU	| 192.168.1.25 |
| Worker Node	   | worker-1-ubuntu-22.04-k8s | 3GB RAM, 2VCPU	|	192.168.1.26 |
| Worker Node	   | worker-2-ubuntu-22.04-k8s | 3GB RAM, 2VCPU	|	192.168.1.27 |
