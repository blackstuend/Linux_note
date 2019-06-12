# 環境設置


* 安裝 ansible
```
# yum install -y ansible
```
* 編輯設置檔

```
# vim /etc/ansible/hosts
[app1]
192.168.56.102

[app2]
192.168.56.104

[myapp]
192.168.56.102
192.168.56.104
```

* 如果ssh改成其他Port，可以將配置檔案後面ip加上port

````
[app3]
192.168.56.102:{port}

````
* 編輯記錄檔

```
# vim /etc/ansible/ansible.cfg
log_path = /var/log/ansible.log
```

* 操作
* [參考](https://www.itread01.com/content/1525534899.html)
1. -m module的意思 後面可以接user shell....等等
2. -a 後面接指令 動作

## Ansible playbook(ansible 腳本檔)
* 使用yaml格式
* 編輯helloworld.yaml
```
---
- hosts: app2 
  remote_user: root 

  tasks:
    - name: hello world # 工作名稱
      command: /usr/bin/wall hello world 
```
* 執行

```
# ansible-playbook -C hello.yml
```