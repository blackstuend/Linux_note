# Stop all thing 

```
# systemctl stop firewalld
# systemctl disable firewalld
# vim /etc/selinux/config  // 將eforcing 改為 disabled
# yum install 
# systemctl enable sshd
# reboot
```

# SSh


```
# yum install ssh -y
# systemctl enable sshd 
* 關機
```

* 再制一個進行ssh連線
* 製作rsa 加密,兩台都要做

```
$ ssh-keygen -t rsa //全按enter
$ cd .ssh
$ scp id_rsa.pub user@192.168.56.102:/home/user/.ssh/authorized_keys
$ ssh user@192.168.56.102
```

# Use Network Stop Networkmanager

```
# systemctl stop NetworkManager
# systemctl disable NetworkManager
# chkconfig network on
# cd /etc/sysconfig/network-scripts
# rm -rf ifcfg-Auto_Ethernet
# systemctl start network
```

# FTP

```
# yum install ftp
# yum install vsftpd
# cd /etc/vsftpd
# vim vsftpd.conf  // 將anon改為啟動 , UP註解取消掉
# chmod 777 /var/ftp/pub 
```

* Client ftp
```
# ftp 192.168.56.101  //輸入 anonymous 密碼 直接enter
# cd pub 
# get or put file
```

# nfs
參考 : https://www.phpini.com/linux/rhel-centos-7-install-nfs-server
```
# yum install nfs-utils
# mkdir /var/nfsshare
# chmod -R 777 /var/nfsshare/
// 開啟 /etc/exports 檔案, 加入以下內容:
/var/nfsshare 192.168.56.102(rw,sync,no_root_squash,no_all_squash)
# systemctl enable rpcbind
# systemctl enable nfs-server
# systemctl enable nfs-lock
# systemctl enable nfs-idmap
# systemctl start rpcbind
# systemctl start nfs-server
# systemctl start nfs-lock
# systemctl start nfs-idmap
```


* client 端
```
# yum install nfs-utils
# mkdir -p /mnt/nfs/var/nfsshare
# mount -t nfs 192.168.56.101:/var/nfsshare /mnt/nfs/var/nfsshare/
```


# Http

```
# yum install -y httpd
# systemctl start httpd
# gedit /etc/httpd/conf.d/userdir.conf
//將 userdir disabled 取消掉
//將 userdir public_html 使用
# systemctl reload htppd
# mkdir /home/user/public_html
//將網頁放進public_html
//更改權限(資料夾a+x 文件 a+r)
# cd /home
# chmod a+x user
# cd user
# chmod a+x public_html
# cd public_html
# chmod a+x index.html
```

* http mysql install 

```
# yum install php -y
# yum install php-mysql
# yum install epel-release
# sudo yum install mariadb-server   //mariadb為新的php
# sudo systemctl enable mariadb
# sudo systemctl start mariadb
# sudo mysql_secure_installation
# mysql -u root -p
```

* mysql創建資料庫

```
create database testdb;
create user 'user'@'localhost' identified by 'user';
grant all on testdb.* to 'user'@'localhost';
show databases; //顯示創建的Databases
use testdb;
create table Persons (
  name varchar(255),
  id int
);
show tables;
insert into Persons(name,id) values ('tom',1); //加入資料表
select * from Persons; //查看資料表
```

# Samba

```
# yum install -y samba //安裝Samba
# mkdir /sharedpath 
# gedit /etc/samba/smb.conf
```

* 修改 smb.conf

```
[global]
  workgroup =WORKGROUP
[myshare]
  path=/sharedpath
  read only = no
  browsable= yes
  comment = test
```

* 使用samba

```
# systemctl start smb
# systemctl start nmb
# pdbedit -a user
```

* 進到windows 底下執行 \\192.168.56.102

# dhcp 

```
# yum install dhcp dhcp-devel -y
# ip addr add 192.168.10.254/24 brd + dev enps08
# echo "1" > /proc/sys/net/ipv4/ip_forward 
# iptables -t nat --append POS -s 192.168.10.0/24 -o enp0s3 -j MASQUERADE
# gedit /etc/dhcp/dhcpd.conf
```
* 內容

```
subnet 192.168.10.0 netmask 255.255.255.0 {
    option routers 192.168.10.254;
    option subnet-mask 255.255.255.0;
    option domain-name-servers 8.8.8.8;
    range 192.168.10.100 192.168.10.200
}
```

* 客戶端
```
# dhclient enps08
````

# Squid (proxy)

```
# yum install -y squid
# cd /etc/squid
gedit squid.conf
cache_dir ufs /var/spool/squid 100 16 256  //把這行的#拿掉
加入內容
acl denyurl dstdomain "/etc/squid/urldeny.txt"
http_access denyurl
//新增一個檔案
# vim denyurl.txt
//內容
.www.yahoo.com
.nqu.edu.tw
//儲存
systemctl restart squid

```

* 讓瀏覽器加入Proxy

* 讓瀏覽器加入proxy >先開firefox>edit>preference>advanced>network>settings 加入127.0.0.1 port = 3128