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

NetworkManagerin CLİ arayüzü de vardır örnek olarak aşağıda ki komutu çalıştırabilirsiniz

```
ugur@ugur:~$ nmcli device status
DEVICE  TYPE      STATE      CONNECTION 
ens33   ethernet  unmanaged  --         
lo      loopback  unmanaged  --
```

