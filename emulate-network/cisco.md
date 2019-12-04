# 專業術語

## Router中的常用術語
1. POST(power on seflt test) //開機自檢系統
2. start-up configuartion //放在nvram(非揮發性記憶體)
3. running configuration //放在ram 當關機時會將裡面的內容寫入start-up configuration
4. out-band //透過console port 一台router只有一個console port
5. in band //透過talent ssh
![text](https://i.ytimg.com/vi/zWIrfS7dEgk/maxresdefault.jpg)

## Cisco Ios

1. 用戶模式 user mode 查看基本設定
2. 特權模式(privige mode)查看更詳細，並實現管理設備，例如設置IOS鏡像設置。
3. 配置模式(configuration mode) 也稱全局模式，可實現設備配置如修改主機名 ip等等

指令大全 https://home.gamer.com.tw/creationDetail.php?sn=3022918
下面設置皆為out of band


```
router > enable //進入特權模式
router# configure terminal  //進入配置模式
router(config)#　enable password cisco //變更enable密碼
router(config)# int f0/0 //進入f0/0的介面卡
router(config=if)# ip address 192.168.1.1 255.255.255.0 //新增ip 
router(config=if)# no shutdown  //執行
router# show ip int breif //顯示所以介面卡資訊
router# erase starup-config
router# write //將running configur 寫入 start-ip configure 
router# copy running-configure startup-config //跟上面一樣，有些機器只能這樣做
router# show running-config
router(config)# hostname R1 //更改名稱
router(config) no hostname //只要加上no 就可以取消剛剛的設置
router# show int f0/0 //顯示指定網卡router
router(config)#username tom password tom123 //新增新的user與密碼 讓之後連進來的telent使用username來連 而不是root
router(config)default range e0/0-1 //初始化界面使用範圍
```


實現in of band，設置連線位置還有密碼
```
router(config) # line vty 0 4  // 0 4指0 1 2 3 4 個人可以使用telnet 所以允許可以有5個人登入
router(config-line) passowrd 1234
router(config-line) no password 
router(config)#username tom password tom123 //新增新的user與密碼 讓之後連進來的telent使用username來連 而不是root
router(config-line) login local //使用local的user帳密來登入

```
接著就可以使用telnet連進去了!!

## 出錯時
* 按下shift + ctrl + 6 ，即可把程序強制停止
## 破解Cisco密碼
[參考](https://blog.xuite.net/tolarku/blog/20365059-%E3%80%90CCNA%E3%80%91Cisco+Router+%E5%BF%98%E8%A8%98%E5%AF%86%E7%A2%BC+-+%E5%AF%86%E7%A2%BC%E5%BE%A9%E5%8E%9F)
先重新開機然後按下ctrl +break
```
rommon 1 > confreg 0x2142 //不使用start-config開機
reboot //重新開機後

router > enable  
router # copy start run //將之前的設置條回來

router(config) # enable password 1234 //再將新的密碼打上
router(config) # enable secret 1234 //將密碼以暗文顯示，直皆使用password 會以明文顯示
router # configure termial //進入config
router(config) # config-register 0x2102 //將startup開機調整回來
route(config) # do ping 192.168.1.1
```

* 複製cisco裡面的bin檔到server
```
# copy flash :tftp: (name) 
192.168.2.1
a.bin
```
* 刪除flash 與show 出flash


```
# show flash:
# del flash:(name)
```
* 從伺服器取得bin檔案
```
# copy tftp: flash:
192.168.2.1 //伺服器ip
a.bin
```
*

```
# (config) boot system flash:a.bin
# write
# reload
```