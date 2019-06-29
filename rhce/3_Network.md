# Add ipv6 address

## Nmcli
* Control NetworkManager's tool 

* Check current network interface
```
# nmcli connection show
# nmcli connection show eth0
```
* check device
```
# nmcli device status
```

* Add ipv6 address
1. server 

```
# nmcli connection modify eth0 ipv6.addresses    fddb:fe2a:ab1e::c0a8:1/64 ipv6.method manual
# systemctl restart NetworkManager
# nmcli con up "eth0"
```

2.clinet


```
# nmcli connection modify eth0 ipv6.address fddb:fe2a:ab1e::c0a8:2/64 ipv6.method manual
# systemctl restart NetworkManager 
# nmcli con up "eth0"
```

* Auto connection
```
# nmcli con mod eth0 connection.autoconnect yes
```
* Try

```
# ping6 fddb:fe2a:ab1e::c0a8:2
# ping6 fddb:fe2a:ab1e::c0a8:1
```

## Nmcli add type team 鏈路聚合

* Add team master
```
# nmcli con add type team con-name team0 ifname team0 config '{"runner":{"name":"activebackup"}}'
```

* Add connection with enp0s8 and enp0s9,slave on team0
 
```
# nmcli con add type team-slave con-name team0-port1 ifname enp0s8 master team0
# nmcli con add type team-slave con-name team0-port2 ifname enp0s9 master team0
```

* Add ipv4 address 

```
# nmcli con mod team0 ipv4.addresses 192.168.0.101/24 ipv4.method manual
```

* Up all interface

```
# nmcli con up team0
# nmcli con up team0-port1
# nmcli con up team0-port2
```
* try

```
# ping 192.168.0.101
# ping 192.168.0.102
```