# mininet

## Install 
[參考資料](https://github.com/intrig-unicamp/mininet-wifi)

1. step 1: $ sudo apt-get install git
2. step 2: $ git clone https://github.com/intrig-unicamp/mininet-wifi
3. step 3: $ cd mininet-wifi
4. step 4: $ sudo util/install.sh -Wlnfv

## Use

```
cd example
sudo python miniedit.py //file > export level 2 script
# sudo python test1 //進到Minist
# net //顯示目前網路拓譜
# links //顯示目前連線是否OK
# link h1 r1 down //將連線中斷(ping 不到)
# link h1 r1 up //將連線開啟
# h1 ifconfig //前面是節點，後面是執行的動作
# xterm h1 h2 //執行終端機 
```

## iperf (傳輸效能測試)

* h2: iperf -s -i 1 -u // -u = udp -s = server -i =以幾秒為單位呈現
* h1: iperf -c 10.0.0.2 -t 10 -u -b 100M //t =time -u = udp -b = speed -c =client 端
* 使用UDP 傳輸 通常會較TCP快  因為不用進行三項交握
* 原本設定的100M 通常是跑不到100M 因為傳輸會包含header 真正的內容是不含有100M

## IP設定

* #ip addr add 192.168.56.1 brd + dev en0s8 //增加ip進機器
* #ip addr del 192.168.56.1 brd + dev en0s8 //刪除ip 在哪張介面卡
* #ipconfig enp0s8 0 //將ip清除
* #ip route add default via 192.168.56.1 // 增加預設路由器經由誰
* #ip route show //顯示內定路由器
* #ip route del default via 192.168.56.1 //刪除預設路由器
* echo 1 > /proc/sys/net/ipv4/ip_
* 0.0.0.0 在伺服器上代表任一介面都可以連接
* #python -m SimpleHTTPServer 80 //簡單創造架設一個網站  
## Hping3
* sudo apt-get install hping3 curl -y

## Gnuplot 


```
#gnuplot
# plot "f1_process" title "flow1" with linespoints,  "f2_process" title "flow2" with linespoints 
#set xtics 0,1,16  //取x的間隔還有取到哪
#set xrange[0:16] //取x從多少開始到結束
#set ytics 0,10,100  //取x的間隔還有取到哪
#set yrange[0:100] //取x從多少開始到結束
#set title "XXX"
#replot
#set terminal gif
#set output "a.gif"
# replot
```