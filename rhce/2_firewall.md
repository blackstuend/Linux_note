# Firewall open ssh 

```
# firewall-cmd --add-service=ssh --permanent
# firewall-cmd --add-rich-rule 'rule family=ipv4 source address=172.17.10.0/24 service name=ssh reject' --permanent
# firewall-cmd --reload 
```

* 顯示firewall的所有規則
```
# firewall-cmd --list-all
```

## port forward 
* 題目 :　將172.25.0.0/24 5423 port 轉發到 80 port

* 注意: Tcp 和 Udp都要加上去
```
# firewall-cmd -add--rich-rule "rule family=ipv4 source address=172.25.0.0/24 forward-port port=5423 protocol=tcp to-port=80" --permanent
# firewall-cmd -add--rich-rule "rule family=ipv4 source address=172.25.0.0/24 forward-port port=5423 protocol=udp to-port=80" --permanent
```