## 動態路由

* [參考](https://giboss.pixnet.net/blog/post/26859072)
* version 2給ip4使用 ,version 3給ipv6使用
* OSPF每30分鐘廣播
* 屬於IGP(RIP EIGRP OSPF)都是,自治區的路由協定
* 支援VLSM(超網化),CIDR(子網路切割)
* 使用Link state
* 適合中大型的網路
* 使用三個表,neighbor表(鄰居表).LSDB表.路由表
* 路由表為LSDB表整理而成的
* OSPF協議為了降低自身的開銷，提出了以下概念： 
* AD(管理距離) 使用110
* 只用bandwith 當作他的路由指標
* OSPF package type
  1. hello : 發送neighbor表給旁邊的路由器,使用224.0.0.5群播的方式,以及hello自身的訊息,例如多久發一次或是多久沒收到就當作那台路由器掛掉之類等等
  2. DBR : 發送自己的資訊 但不完整的
  3. LSR(requset) : 向路由器發送請求取得新的LSDB表
  4. LSU(update) : 向請求的路由器發送它自身的LSDB表
  5. LSA (ACK) : 確認是否有收到發出去的LSDB表

* DR
  -  (1) DR(委任路由器)： 在各類可以多址訪問的網路中，如果存在兩台或兩台以上的路由器，該網路上要選出一個 (DR)。“委任路由器”負責與本網段內所有路由器進行LSDB的同步。
  -  (2) BDR(備用委任路由器)： 如果DR壞了則使用BDR當成現任DR
  -  (3)DROTHER : 其他的路由器就是DROTHER

* 怎麼去選 DR,BDR
  - 使用priority : 手動設定
  - router id  
  1. 使用手動設定
  2. loopback,挑ip小的
  3. 實體介面ip,挑ip大的


* 顯示OSPF Neighbor
```
R1# show ip ospf neighbor
```
* 顯示OSPF詳細資料
```
R1# show ip ospf interface
```