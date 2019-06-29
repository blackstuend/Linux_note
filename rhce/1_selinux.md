# Seliunx set Enforcing

```
# vim /etc/selinux/config
```
* Selinux modes
1. Enforcing:強制模式,依照設定來限制檔案存取
2. permissive:寬容模式,不限制檔案存取,但會根據檢查來記錄訊息
3. disabled:停用模式,直接關閉selinux

* 將selinux 那條改成 selinux=enforcing

* 臨時切換selinux 模式 

```
# setenforce 0 //0 是 permissive
# setenforce 1 //1 是 enforcing
```

* Get selinux status

```
# sestatus
```

* Get selinux current mode

```
# getenforce
```