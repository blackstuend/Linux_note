# Load Balancing(LB)
* 常使用方式
1.  LVS
2. haproxy
3. nginx (easy to use)
4. keepalived (health check)
# Docker 

1. Docker-compose 
2. Docker-SWAM

# 監控系統
1. Zabbix
2. Nagious

# 自動管理系統
1. saltstack
2. ansible
3. puppet

# 遠端桌面(VNC)

1. 設定值 > 顯示 > 遠端顯示 //遠端桌面Port 3389
2. 在電腦裡用netstat -an |find 3389 //檢查3389是否被占用
3. 在windows下打mstsc(遠端桌面) 輸入 ip :3389 

# google ssh authenticator(用google認證centos 7)
 [參考](https://kenwu0310.wordpress.com/2016/12/09/centos-7-ssh-%E9%9B%99%E5%9B%A0%E7%B4%A0%E8%AA%8D%E8%AD%89-using-google-authenticator/)
1. git clone https://github.com/google/google-authenticator-libpam
2. yum install pam-devel
* installation
1../bootstrap.sh
2. ./configure
3. make
4. make install
5. mv /usr/local/lib/security/pam_google_authenticator.* /usr/lib64/security/
6. vim /etc/ssh/sshd_config
   //ChallengeResponseAuthentication yes
7.  systemctl restart sshd
8. google-authenticator