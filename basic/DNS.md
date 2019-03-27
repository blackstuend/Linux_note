# DNS
* 使用udp協定 ,port=53 不加密
* 進行名稱解析
* Domain name > IP(正向解析)
* IP > Domain name(反向解析)
* DNS Spoofing(DNS詐騙 駭客在client端查詢Ip時，攔截封包，並傳假的封包回去假ip)
* DNS也可以用作DDos攻擊(偽裝成被害者的ip，不斷送出DNS request，DNS會一直送封包給被害者的電腦，直到被攻陷)
* [智能DNS](https://www.imooc.com/learn/768)
* [Linux Bind](https://www.imooc.com/learn/723)
## DNS 紀錄
* 一個網域的名稱最後有1點(.)，稱作FQDN(Fully qualified domain name)，也是最完整的名稱，而現在DNS都會幫我們最後加上.
* A紀錄 是 domain name 轉換 成 ipv4的ip AAAA紀錄 則是轉成 ipv6的ip 
* PTR紀錄 則是 ip 轉為 domain name (反向解析)
* MX紀錄 mail server
* C NAME 紀錄 為 電腦的別名 
## Linux內部DNS
```
# /etc/hosts //顯示出內部預設的dns
```
## 路由器
* 裡面會有DNS cache(如果先前有登入，會先快取住),DNS forward(幫忙轉送給其他DNS server)
## Windows查找IP

```
# nslookup www.nqu.edu.tw 8.8.8.8 //前面打上domain name 後面打上DNS server 也可以不打
```
## Linux查找 IP

```
# dig www.pchome.com.tw
# dig www.nqu.edu.tw
# dig @8.8.8.8 www.nqu.edu.tw //前面@打上dns 後面打domain name 
# dig www.nqu.edu.tw ns //查詢此網站的dns  //查找到之後再去查詢他的dns server
# dig -x 8.8.8.8 // 可以反查domain name 
```
## 查找selinux是否關閉

```
# getenforce
# cat /etc/selinux/config
```
## 查看Port 是否有開啟

```
# netstat -tunlp | grep 53 //查找port 是否有開 (53)
```

## 安裝與開啟dns
```
#　yum install bind bind-utils bind-chroot
# systemctl start named //開啟dns
# systemctl staus named //查看dns狀態
# vim /etc/named.conf //修改dns設定檔 ，
options {
        listen-on port 53  { any; }; //此處也調成any ,預設是127.0.0.1,對於哪張網卡有開啟dns 
        //listen-on-v6 port 53 { ::1; }; 
        directory          "/var/named";
        dump-file          "/var/named/data/cache_dump.db";
        statistics-file    "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        allow-query        { localhost; 192.168.100.0/24;any }; //此處調成any 允許誰來連
        recursion yes;

        dnssec-enable yes;
        dnssec-validation yes;
        dnssec-lookaside auto;
        bindkeys-file "/etc/named.iscdlv.key";

        managed-keys-directory "/var/named/dynamic";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";
};

```
# 內部DNS 設定自己的網域
* gedit /etc/named.rfc1912.zones //新增下面那行,新增一個新的網址

```
zone "test.com" IN{
        type master;
        file "name.test";
        allow-update {none};
}
* gedit /var/test.name
```

```
$TTL    1D
@	IN SOA       @ rname.invalid. (
		0              ; serial (d. adams)
		1D              ; refresh
		1H             ; retry
		1W              ; expiry
		3H )            ; minimum
        NS      @
        A      192.168.56.101
www     A       192.168.56.101
ftp     A       192.168.56.100
```
* 反向解析

```
# gedit /etc/named.rfc1912.zones //新增下面那行,新增一個新的網址
zone "56.168.192.in-addr.arpa" IN {
        type master;
        file "named.test.ip";
        allow-update {none};
```

```
$TTL    1D
@	IN SOA     dns.test.com  @ rname.invalid. (
		0            
		1D           
		1H            
		1W             
		3H )
@	IN NS dns.test.com
101	IN PTR www.test.com
100	IN PTR ftp.test.com
```