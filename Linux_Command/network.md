# network

## 配置DNS
[root@PMS-Campaingn2 ~]# less /etc/resolv.conf 
nameserver ~

## 查看Ip地址
> ip addr (show) 

## Centos7开机之后连不上网ens33mtu 1500 qdisc noop state DOWN group default qlen 1000
```
查看Ip地址发现ens32网卡状态没有Ip信息。
解决、先停止网卡，设置disable，然后启动，发现网卡启动了
[root@localhost ~]# systemctl stop NetworkManager
[root@localhost ~]# systemctl disable NetworkManager
Removed symlink /etc/systemd/system/multi-user.target.wants/NetworkManager.service.
Removed symlink /etc/systemd/system/dbus-org.freedesktop.NetworkManager.service.
Removed symlink /etc/systemd/system/dbus-org.freedesktop.nm-dispatcher.service.
[root@localhost ~]# service network restart
[root@localhost ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens32: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:b7:40:50 brd ff:ff:ff:ff:ff:ff
    inet 192.168.145.10/24 brd 192.168.145.255 scope global ens32
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:feb7:4050/64 scope link 
       valid_lft forever preferred_lft forever
```

## 

