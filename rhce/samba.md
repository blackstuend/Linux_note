# Samba

* 安裝samba
````
# yum install -y samba samba-client
````
* 啟動samba

```
# systemctl start smb nmb
# systemctl enable smb nmb
```
* 創建共享目錄

```
# mkdir /common
```
* 設定selinux ,將common 下所有文件 打上samba_share_t 的標籤
```
# semange fcontext -a -t 'samba_share_t' '/common(/.*)?'
# restorecon -RvF /common/
```

* 新增權限
```
# chmod o+w /common/
# ls -ld /common/ //檢查
```

* 修改設定檔

```
# vim /etc/samba/smb.conf
```

```
workgroup = STAFF
[common]
    comment = this is a test
    path = /common
    browseable = yes
    wrire list = brian
    host allow = 172.25.0.
```

* 設置完,重啟服務

```
# systemctl restart smb nmb
```

* 設定防火牆

```
# firewall-cmd --add-service=samba --permanent
# firewall-cmd --reload
# firewall-cmd --list-serivce
```

* 增添用戶

```
# useradd -s /sbin/nologin rob
# useradd -s /sbin/nologin brian
# smbpasswd -a rob
# smbpasswd -a brian
```