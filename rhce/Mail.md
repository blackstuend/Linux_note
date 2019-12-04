# Postfix

* 主要設定檔
```
# vim /etc/postfix/main.cf
```
* 一般編輯方式
```
# postconf -e "inet_interfaces = loopback-only"
# postconf -e "local_transport = error:asasd"
# postconf -e "mynetworks = 127.0.0.0/8 [::1]/128"
# postconf -e "mydestination= "
# postconf -e "myorigin = example.com"
# postconf -e "relayhost = classroom.example.com"
# postconf -e restart postfix.service
```