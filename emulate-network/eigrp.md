```
# router eigrp 1
# network 10.0.0.0

```

```
# show ip eigrp neighbors
```

```
H 代表與鄰居建立的順序
INTERFACE 與鄰居關係的介面
Hold 0-15s 做一次gierp交換
uptime 與鄰居建立的時間
SRTT 平均往返時間
RTO 超時時間 5000內
Q 序列 通常是0
```

鄰居關係 使用hello報文
* 群波的方式,使用224.0.0.10,建立鄰居訊息,不傳遞路由
* 使用88 port


