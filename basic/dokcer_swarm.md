# Docker Swarm
* 控制其他台電腦執行docker程序並監控
* 分配各式工作給其他電腦

## 預處理
* 先加入/etc/hosts,將每個電腦的ip加進去


vm1:
```
# vi /etc/hosts
192.168.56.3 vm1 
192.168.56.4 vm2 
192.168.56.5 vm3 
```

```
# scp /etc/hosts root@vm2:/etc/hosts
# scp /etc/hosts root@vm3:/etc/hosts
```


* 不用密碼執行連SSH, 使用ssh-keygen ,Set ssh-key to other computers

```
# ssh-keygen
# ssh-copy-id root@vm2
# ssh-copy-id root@vm3
```


## 實作
* docker manager 初始化 ,後面並宣告自己的ip作為管理者
```
# docker swarm init --advertise-addr 192.168.56.3 
```
* 另外兩台加入進192.168.56.3的
vm2:
```
 # docker swarm join --token SWMTKN-1-13z87yql1f2jlnipurpbx53b9xocm8awqdlamdtvjxlph7adf8-28guoxrse21f1p4ubk5fs4dza 192.168.56.3:2377

```

vm3:
```
 # docker swarm join --token SWMTKN-1-13z87yql1f2jlnipurpbx53b9xocm8awqdlamdtvjxlph7adf8-28guoxrse21f1p4ubk5fs4dza 192.168.56.3:2377
```

## docker swarm 操作

* 離開docker swarm 的群組

```
# docker swarm leave
```

## Docker node 操作
*  docker swarm 裡的管理的node
```
# docker node ls
```

* docker Manager 關閉node

```
# docker node update --availability drain vm1
```
* docker Manager 開啟node 
```
# docker node  update --availability active vm1
```

## docker service 操作

* 啟動docker服務

```
# docker service create --name web --replics 2 httpd 
```

* 將docker 一次開啟多個

```
# docker service scale web=5
```

## Docker overlay
* Network Overlay:讓外面連進來都經過一個network 來幫我們做分配

![OVERlay](./overlay.PNG)
* 如上圖,可以讓連進的ip都是經由s1轉發
* 使用資料庫做連結時較方便使用

* 創造一個network(overlay)
```
# docker network create --driver overlay mynet
```


* 開啟一個web服務
```
# docker service create --replicas 2 --name s1 --network mynet httpd
```

* 開啟另一台虛擬機,使用相同的network

```
# docker service create --replicas --name s2 --network mynet pingbo/whoami 
```

* 兩邊就能夠互ping
    * 進入pingbo/whoami測試看看

## replicas 和 mode global
* 一次創造多個副本
* Replicas
    * 可以創造無數個vm
    * 但是會重複在各台電腦上
    * 如果有其中一台電腦關閉或是當機則會將vm轉移去其他還活著的電腦
* 範例
```
# docker service create --name web --replicas 5 httpd
```
* Global
    * 不需要指定數量
    * 重複在各台電腦上,有幾台就呈現幾台
    * 如果有其中一台當機或關閉,則vm跟著消失,直到電腦恢復,不會轉移到其他台電腦上
* 範例
```
# docker service create --name web --mode global httpd
```

# Docker swarm update and roll back
## Update:
* 如果今天要將執行的container都進行更新到新版本,又不想讓他關閉

* Example
    * 一開始使用httpd:latest 後面 更新為 httpd:alpine
* 創立httpd:latest
```
# docker service create --name web --replicas 2 httpd:latest
```
* 更新
```
# docker service update --image httpd:alpine web
```

## RollBack
* 還原Update上一的版本

```
# docker service rollback web
```

# Docker node Label
* 幫每一台電腦設定標籤,可以用來指定哪一台電腦要開幾個vm
* 先顯示各台電腦的名稱
```
# docker node ls
```

* 將電腦設定標籤(vm1)
```
# docker node update --label-add env=test vm1
```
* 設電腦定標籤(vm1)
```
# docker node update --label-add env=product vm2
```
* 啟動vm,指定數量在對應的label的電腦
    * 指定label中env為test的
    * 注意: label後面接變數,而且要使用== 或是!= 判斷式來判斷
```
# docker service create --constraint node-labels.env==test --replicas 2 --name web httpd
```
* 將剛剛跑在測試的vm遷移到另一台電腦label env=product的
* 將vm的label的標籤拿掉
```
# docker service update --constraint-rm node.labels.env==test web

```
* 加入新的label為env=product,然後找出對應的
```
# docker service update --constraint-add node.labels.env==product web

```