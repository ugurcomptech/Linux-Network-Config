# Linux-Network-Config


Bu yazımızda linux network yapılandırmalarını yapacağız. 

## Ip Adresini görüntüleme

`ip a` komutu, Linux'ta ağ arabirimlerinin ve IP adreslerinin ayrıntılı bilgilerini gösteren bir komuttur.

```
ugur@ugur:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:89:8f:42 brd ff:ff:ff:ff:ff:ff
    altname enp2s1
    inet 192.168.1.170/24 brd 192.168.1.255 scope global ens33 ## bizim IP adresimiz
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe89:8f42/64 scope link 
       valid_lft forever preferred_lft forever
ugur@ugur:~$ 
```

`sudo ifconfig` komutu, Linux sistemlerinde ağ arabirimlerinin durumunu ve yapılandırma bilgilerini gösteren bir komuttur. Bu komut, ağ arabirimlerinin IP adresi, ağ maskesi, MAC adresi ve diğer ayrıntıları gibi bilgileri sağlar.
```
ugur@ugur:~$ sudo ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.170  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::20c:29ff:fe89:8f42  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:89:8f:42  txqueuelen 1000  (Ethernet)
        RX packets 592  bytes 188297 (183.8 KiB)
        RX errors 0  dropped 34  overruns 0  frame 0
        TX packets 157  bytes 20795 (20.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 35  bytes 3569 (3.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 35  bytes 3569 (3.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

 `ip r` komutu default gatewayi ve IP adresini görmeye yarayan bir komuttur.

```
ugur@ugur:~$ ip r
default via 192.168.1.1 dev ens33 onlink 
192.168.1.0/24 dev ens33 proto kernel scope link src 192.168.1.170 
```

## Network Manager

Network Manageri kullanmak için aşağıda ki komut ile bunu indiriyoruz.

```
ugur@ugur:~$ sudo apt install network-manager
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
network-manager is already the newest version (1.30.6-1+deb11u1).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

` systemctl status NetworkManager` yazarak da görüntüleyebilirsiniz.

```
ugur@ugur:~$ systemctl status NetworkManager
● NetworkManager.service - Network Manager
     Loaded: loaded (/lib/systemd/system/NetworkManager.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2023-12-23 05:31:12 EST; 13min ago
       Docs: man:NetworkManager(8)
   Main PID: 570 (NetworkManager)
      Tasks: 3 (limit: 5317)
     Memory: 14.6M
        CPU: 299ms
     CGroup: /system.slice/NetworkManager.service
```

NetworkManagerin CLI arayüzü de vardır örnek olarak aşağıda ki komutu çalıştırabilirsiniz

```
ugur@ugur:~$ nmcli device status
DEVICE  TYPE      STATE      CONNECTION 
ens33   ethernet  unmanaged  --         
lo      loopback  unmanaged  --
```


Eğer Wireless yani kablosuzdan bağlıysanız aşağıda ki paketi indirebilirsiniz

```
ugur@ugur:~$ sudo apt install wireless-tools
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following NEW packages will be installed:
  wireless-tools
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 113 kB of archives.
After this operation, 304 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bullseye/main amd64 wireless-tools amd64 30~pre9-13.1 [113 kB]
Fetched 113 kB in 1s (225 kB/s)        
Selecting previously unselected package wireless-tools.
(Reading database ... 292670 files and directories currently installed.)
Preparing to unpack .../wireless-tools_30~pre9-13.1_amd64.deb ...
Unpacking wireless-tools (30~pre9-13.1) ...
Setting up wireless-tools (30~pre9-13.1) ...
Processing triggers for man-db (2.9.4-2) ...
```

Aşağıda ki komutu yazarak da görebilirsiniz.

```
ugur@ugur:~$ sudo iwconfig
lo        no wireless extensions.

ens33     no wireless extensions.
```


## Linux IP Yapılandırmaları

Şuan ki IP adresimiz **192.168.1.170** olarak gözükmektedir. Bu benim tarafımdan verilmiş static bir IP adresidir. Şimdi bunu nasıl düzenleyebileceğimizi görelim.

```
ugur@ugur:~$ sudo ifconfig ens33
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.170  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::20c:29ff:fe89:8f42  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:89:8f:42  txqueuelen 1000  (Ethernet)
        RX packets 1568  bytes 610232 (595.9 KiB)
        RX errors 0  dropped 92  overruns 0  frame 0
        TX packets 256  bytes 29082 (28.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```


Terminalden `sudo nano /etc/network/interfaces` dosya yoluna gidip orada yapılandırmaları yapacağız.

![image](https://github.com/ugurcomptech/Linux-Network-Config/assets/133202238/6bab18ed-cf94-44fc-a65b-11b44920ea96)


Siz de içi boş şekilde gelecek alt satıra gelip aşağıda ki komutları yazacağız

```
# ens33
auto ens33  # Ağ arabirimi otomatik olarak etkinleştirilir
iface ens33 inet static  # Arayüz, statik IP yapılandırması kullanacak şekilde ayarlanır
    address 192.168.1.170  # Arayüzün IP adresi
    netmask 255.255.255.0  # Alt ağ maskesi
    gateway 192.168.1.1  # Varsayılan Ağ Geçidi
    dns-nameservers 8.8.8.8  # DNS Sunucuları
```

Bunu yaptıktan sorna terminalde 

şu iki komuttan birini çalıştırabilirsiniz

```
sudo ifdown ens33 && sudo ifup ens33
```

```
sudo systemctl restart networking.service # bunu kullanmanız önerilir
```











