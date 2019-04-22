# LVS(Linner Virtual Server)

## 模式
1. NAT

2. DR
* 主要是在LVS上面改寫MAC位址
3. tunnel
![Alt text](/path/to/img.jpg)
* [參考網站](https://s1.51cto.com/wyfs02/M01/8D/D9/wKioL1itAGmw5GVkAAJW6spoxAc617.png)
* [實作網站](https://blog.51cto.com/lansgg/1229421)
* [DR參考網站](https://blog.csdn.net/nimasike/article/details/53820844)
* [tunnel參考網站](https://blog.51cto.com/tengq/1900124)
## 功能
* 用作附載均衡

## 常用的scheduling

1. RR(Round Robin)
2. WRR(Wait Round Robin)
3. Least Connection