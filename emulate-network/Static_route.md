
# 靜態路由
```
# show ip route
# debug ip icmp //當有imcp封包進來時，則會conosole.log出來
# ping 12.1.1.2 source 1.1.1.1
```

* 新增路由

```
# ip route (紀錄的位址) (紀錄位址的mask) (走哪個Interface)
```

* 靜態路由 route 與 route 互聯 用serial port(只會一對一) ，如果是用乙太網路連 ，可能會連到switch
* serial port 可以用 ip route address mask 網路卡號 ，
* 乙太網路 用 ip route address mask ip位址

# 浮動路由

* R2 R3為network 當R2斷掉時，使R3接上，而平常都是用R2來連接上network，這邊使用管理距離 越短越優先，如果沒設定
那管理距離預設為1
* 當她ping 任何不再路由表上的時候 會由0.0.0.0 來接管
，轉ping成12.1.1.2 或 13.1.1.2

![static_route](./static_route.png)
````
# ip route 0.0.0.0 0.0.0.0 12.1.1.2  //主線路
# ip route 0.0.0.0 0.0.0.0 13.1.1.2 100  //100為管理距離 越低越優先
````
# 刪除所有路由

```
Router # clear ip route *
```


## 查看 ip protocol

```
Router # show ip protocol
```
# 動態路由

* IGP ，自治系統內自己跑的協定, EGP 是 自治系統與自治系統跑的協定
* IGDP 主要 用 RIP 協定
* RIP 有 RIP1 RIP2
* Distance Veror(DV)是指路由器會透過相鄰的路由器會得到資訊 
* OSP link state : 路由器透露flooding 了解整個網路架構，後透過演算法計算不同網路會經過不同的路徑

RIP1 | RIP2 |
-----|:----:|
有類路由|無類路由
AD:120| AD:120
廣播更新|群波更新
不支援自動彙整|支援自動彙整

# rip操作
* 當三個都操作完 使用 show ip route ，即可
動態的拿到rip學到的網路

![123](./active_route.png)
## R1
```
Route(config) #router rip 
Route(config-router) #version 2
Route(config-router) #no auto-summary //不自動彙整
Route(config-router) #nework 12.1.1.0 
```
## R2
```
Route(config) #router rip 
Route(config-router) #version 2
Route(config-router) #no auto-summary //不自動彙整
Route(config-router) #nework 12.1.1.0 
Route(config-router) #nework 23.1.1.0
```
## R3
```
Route(config) #router rip 
Route(config-router) #version 2
Route(config-router) #no auto-summary //不自動彙整
Route(config-router) #nework 23.1.1.0 //不自動彙整
```

# Rip Auto-summary

* 當他不做彙整的時候，會把學到的全部丟進路由表裡面
* 如果進行彙整，那會把路由表的網路變成超網
* 怎麼樣才能彙整成超網，假設有4個ip
1. 192.168.1.0/24
2. 192.168.2.0/24
3. 192.168.3.0/24
4. 192.168.4.0/24
* 彙整成的超網會變成192.168.0.0/22 把.1~.4都形成一個網路 遮罩也變成255.255.255.252.0

![](./active_rip_route.png)

