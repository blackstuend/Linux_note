# Nagios

* [參考網址](http://www.nccst.nat.gov.tw/ArticlesDetail?lang=zh&seq=1106)
* [參考網址](http://linux.onlinedoc.tw/2016/09/centos7rhel7-nagios-for-apache.html)
* 主要是當多台server時，管理不易，所以這邊開一個nagios server來管理所有的server


```
# yum install epel-release
# yum install httpd nagios nrpe
# systemctl start httpd nagios
# systemctl start nagios
# htpasswd -c /etc/nagios/passwd nagiosadmin //設定nagios 帳號 
1234  //密碼
retrty passwd :1234 //重新打上密碼
# systemctl restart httpd 
# systemctl restart nagios 
```


* 在瀏覽器打上 192.168.0.104/nagios 輸入帳號密碼 即可到管理介面

## 更改設定檔

* 新增新的host
* 進入編輯器 ，複製上面的有的 更改內容就可以了
````
# cd /etc/nagios/object
# vim localhost.cfg
````

```
define host{
    use                    linux-server
    host_name              web1
    alias                  web1
    address                192.168.56.105 //web_server ip
    }
```

* 新增監控的內容

```
define service{
    use                    generic-service
    host_name              node01
    service_description    HTTP
    check_command          check_http //如果要檢查8080 port 則使用 check_http!-p 8080
    }
```
* 使用nrpe監控

```
define service{
    use                    generic-service
    host_name              node01
    service_description    nrpe_check_user
    check_command          check_nrpe!check_users //如果要檢查8080 port 則使用 check_http!-p 8080
    }
```

* 設定check_nrpe指令

```
# cd ./command.cfg
command_name check_nrpe
command_line $USER1/check_nrpe -H $HOSTADDRESS -c $ARG1$
```