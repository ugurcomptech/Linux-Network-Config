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
