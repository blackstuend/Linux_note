# Spinning tree protocol (stp)
* [參考網站](https://www.jannet.hk/zh-Hant/post/spanning-tree-protocol-stp/)
* 邏輯上形成tree的架構，但是實際上是迴圈的方式，同時也避免brodadcast storm

## PVST
* CSICO預設的STP設定
* BID : priority + mac address
* PID : port id
* priority 預設為32768
* 更改priority
    * 更改priority必須為4096的倍數
```
# switch(config)spanning-tree vlan 1 priority 40960
```
* ![spanning_tree](./spaning_tree.png)
1. 會先找尋1台路由器當作ROOT
    * 選擇Root的條件
        - 選擇switch的BID最小的
2. 找尋DP(指定端口)
    * DP指定接口為可通的接口
    * ROOT自己上面的接口皆為DP
    * 如上圖假設Switch1為root,s1的接口皆為DP
    * s1.e0/1,s1.e0/0 為DP
    * s2.e0/1 和 s3.e0/0也為DP
3. 找尋RP
    * RP是root port
    * RP為其他switch被root接到的接口
    * s3.e0/0 , s2.e0/1 皆為從root(s1)接來的接口為RP
4. 要找出switch2 和 switch3 哪個接口為bp(block bp),哪個為DP
    * BP為阻塞接口
    * 要選出DP以及BP的條件順序
        1. cost
        2. bid
        3. pid
    * 而此時的cost相同所以看BID
        * switch2 的 bid 相較 switch3 較低,所以s2.e0/0為dp,switch3.e0/1為bp

* 最終成像

![](stp_finally.PNG)
