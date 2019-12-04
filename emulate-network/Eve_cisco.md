* arp攻擊
* 中間人攻擊

* 安裝eve-ng client  [網址](https://www.eve-ng.net/downloads/windows-client-side-pack)

* Eve 如果有錯誤 要先fix

```
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```
# 幫client增加ip

```
# ip 192.168.56.1 255.255.255.0
```

# 增加vlan (在switch 上面進行)

````
switch > enable
switch > conf t 
switch(config) # vlan 10
switch(config) # vlan 20
do show vlan brief
switch(config-vlan)# int e0/0
switch(config-if)# switch
# switchport mode access
# switch 
# switchport access vlan 10
# 
````

# Vlan 範圍0~4095

* 0還有4095為系統保留vlan ,不可使用者使用,所有接口默認在vlan 1上面
* 2~-1001 ethernet 常用vlan 範圍
* vlan 接口有access trunk
1. access 可以看到標籤
2. trunk 看不到標籤  


# vlan trunk mode

* 有ISL(standard)　還有802.1Q 兩種協定
* 增加trunk

```
# switch(config) > int e0/0
# switch(config-if) > switchport trunk encapsulation do
```

* 限制vlan 的進入

```
# switch(config-if) > switchport trunk allowed vlan 10  //allow vlan 10 to access
```
* 查看trunk
```
#switch > show interface trunk  //Native vlan =原生的vlan 如果是1 代表是默認
# switch > show interface trunk 
```

## vlan(trunk) 的架構

* 802.1Q vlan 有2個byte = 16個bit = 有2 bit為保留 14個為儲存vlan 的值 所以有2^14個值=4096 = 0~4095

## vlan的trunk native

* 兩台switch之間常會用trunk mode 來傳遞資料 如果沒有設定default vlan 每一次都會打上標籤使得效能下降
* 我們可以將switch 的trunk mode 的default 設定成想要的vlan，就不會在那個vlan 上面打上標籤
```
# switch (config)> int e0/0
# switch(config-if) > switchport native vlan 10
```

## vlan trunk protocol (vtp)
* 需要先將switch 都是trunk mode 
* cisco 專有的
* 由於如果交換機很多或是vlan很多，使用vtp可以幫忙處理
* vtp的模式
1. server :能創建 刪除 修改vlan 
2. client :不能創建 刪除 修改 能學習轉發
3. transparent:能創建 刪除 不能學習

## vtp使用
[參考](https://www.jannet.hk/zh-Hant/post/vlan-trunking-protocol-vtp/)
```
switch(config) > do show vtp status 
switch(config) > vtp mode server 

```

## DTP
[參考](https://www.jannet.hk/zh-Hant/post/dynamic-trunking-protocol-dtp/)


## VTP