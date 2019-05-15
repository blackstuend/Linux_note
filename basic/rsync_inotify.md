## rsync
[參考](https://www.itread01.com/content/1511251328.html)
[參考](https://blog.gtwang.org/linux/rsync-local-remote-file-synchronization-commands/)


* 常用指令

```
# rsync -avzh --delete --progress root@192.168.56.103:/rsync  //-a 封裝 -r 遞規 -z 啟用壓縮
```
## inotify

[參考](https://www.google.com/search?ei=TI3bXNTEEImxmAXvpJZI&q=inotify+centos&oq=inotify+centos&gs_l=psy-ab.3..35i39j0i203j0i30l5j0i5i30l2j0i5i10i30.944.2608..2772...2.0..0.115.806.7j2......0....1..gws-wiz.......0i71j0i67j0j33i160.jTsh4joMF9w)
[參考](https://www.netadmin.com.tw/article_content.aspx?sn=1510150002&jump=2)

* 常用指令
```
# inotifywait -rm -e create,modify,delet /rsync  // -r 遞規 -m 為持續監控 -e 後面打command create,modify 之類
```