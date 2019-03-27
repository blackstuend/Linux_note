# Sed

## 原理
* 從文本或是內容 > 每次選取一行 > 進行命令處理 >再選取一行 > 進行命令處理(直到最後一行) >將結果打印到螢幕上
* sed 每一次只會處理一行
* sed 不改變文件內容

## 文本處理
* 命令行格式
```
$ sed [option] 'command' files
```

* 腳本格式
```
$ sed -f scriptfile files
```

* option : -e ;-n
* 基本操作命令
1. -p = 打印 常跟-n 搭配

## 用sed打印出指定行數

```
$ sed -n '10p' a //打印a文件內的第10行
$ sed -n '10!p' a //打印除了第10行以外的內容
$ sed -n '10,20p' a //打印第10行到20行的內容
$ sed -n '10,20!p' a //不打印10行到20行的內容
$ sed -n '\1\,\5\p' a //印出有顯示出1的那行到顯示5的那行
$ sed -n '1~2p' a //打印每行 以間隔輸出
```

## 用sed 新增指定內容

```
$ sed '5a =====' a //在第5行底下新增======
$ sed '1,5a =========' a 在1到五行底下新增=====
$ sed '5i =====' a //在第5行前面新增 =====
$ sed '1,5i =====' a //在第1到5行前面新增 =====

```

## 用sed替代內容

```
$ sed '10c 123' b //將第10行替代成123
$ sed '1,10c 123' b //將1到10行全部變成123
```

## 用sed 進行del

```
$ sed '/hello/d' b //將b內容有hello的那行刪除
```

## 用sed 在文末新增東西

```
$ sed '$a \   hello,world. \n banana' b //在文末增加hello,world
```

## sed進行空白刪除

```
$sed '/^$/d' c //將c內容內的空白刪除
$sed -i '/^$/d c' //-i參數會將輸出值直接輸入到檔案內
$cat c |grep -v '^$' //grep 也可以將空白刪除
```

## sed進行替代

```
$ sed 's/1/a/g a 將1替換成a  
$ ifconfig enp0s3|grep inet|grep -v inet6|sed 's/inet .*r://' //將inet 後面的替換掉 .*r(這邊的r)只的是非貪婪，不然他會直接取到會後面的:
```

## sed高級替代

```
$ sed 's/1/& is apple/' b //& 會將1記起來 
```

## sed大小寫轉換

```
$ sed 's/^[a-z_-]\+/\u&/' d //可以將第一個字母轉成大寫 \u轉一個字母(大寫) \U轉所有字母(大寫) \l 轉一個字母(小寫) \L轉所有字母(小寫)
```

## sed 提早結束

* 可以用來找到第一個需要的文字

```
$ sed '/fasle/q' //找到第一個false 就結束
```

# Awk

## awk基本
* 可用於腳本
* 可用邏輯

## awk 分隔

```
$ awk -F ':' '{print $1}' /etc/passwd //以":"為分隔點 $0為整行 $1為第一個分隔字段
$ awk -F ':' '{print NR,RF}' /etc/passwd //NR是行號 NF是字段總數 
$ awk -F ':' '{print FILENAME}' /etc/passwd //FILENAME印出檔名 
```

## awk 兩種打印方式

* Print
* Printf(跟c語言一樣的方式)
```
$ awk -f ':' '{print $3}' /etc/passwd
$ awk -f ':'  '{printf("%s\n",$3)}' /etc/passwd
```

## awk 條件判斷

```
$ awk -f ':' '{if($3>100) print $3 }' /etc/passwd //UID >100的打印出來
```

## awk 的regular expression

```
$ awk '/Error/{print $1} etc/passwd ' //查找到Error的行 再去打印出$1 
$ sed -n '/Error/p' passwd |awk '{print $1}' //跟上面做的事情一樣
$ awk -F ':' '{$1~/^m.*/{print $1}' passwd //將第一個字母為m開頭的打印出來 $1~ 是指使用第一個分隔字段來進行正規表達式判斷
$ awk -F ':' '{$1!~/^m.*/{print $1}' passwd //將第一個字母為m開頭的打印出來 $1!~ 跟上面類似 但是!是否定的意思

``` 

## awk 擴展格式

* BEGIN 
* END

```
$ awk -f ':' 'BEGIN{print "LINE Col User"}{print NR,NF,$1}END{print "--------Finish-------"}' /etc/passwd
```

## awk設置變量

```
$ls -l |awk 'BEGIN{size=0}{size+=$5}END{print "size is " size}'
```

## awk 程式語言

```
$ awk -F ':' 'BEGIN{count=0}{if($3>100)NAME[count++]=1}END{for(i=0;i<count;i++)print i,NAME[i]}' /etc/passwd //印出UID >100 而且存起來 運用for迴圈將有幾個記下來
$ netstat -anp |awk '$6~/CONNECTED|LISTEN/{sum[$6]++}END{for(i in sum)print i,sum[i]}' //計算CONNETCTED 和 LISTEN的數量 sum[$6]這邊的適用物件的方式來儲存!!
```