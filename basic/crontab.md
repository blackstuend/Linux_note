# Crontab

## 安裝

```
# yum install cronie crontab 
# systemctl start crontab
# systemctl enable crontab
```
## 基本用法

```
$ crontab -l //列出
$ crontab -e //寫入 這邊是一般系統用戶來使用 ,資料存在/var/spool/cron/user 內
# vi /etc/crontab //這邊是用root來操作
```

## Crontab日誌

```
$ cat /var/log/cron
```

## 系統變量

* 存放在/etc/profile
* 一般使用的變量存放在/user/.bash_profile

## 清空系統日誌

* 日誌存放在/var/log
```
# crontab -e
* * * * * echo /dev/null > /var/log/messages //每小時 將messages裡面的資料清空 /dev/null 是一個空文件 
```