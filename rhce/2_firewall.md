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