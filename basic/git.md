# Git

## git checkout
* 可用於切換branch
* 切換commit

```
$ git checkout -b new_branch //-b 則是如果沒有這個branch 就創造一個新的
$ git checkout HEAD^//切換commit 
```

## get reset 
* 有分soft,mixed,hard
* 使用上要小心會直接強制回到之前的commit,後面的資料也會不見
* 如果失敗要查詢消失的commit請使用 git reflog
* 預設是mixed
* soft 會回到 commit 之前
* mixed 回到 add之前
* hard則直接回到資料尚未變更前

模式名稱 | master 名稱 |索引 |工作目錄
--------|-------------|-----|-------|
soft    |    修改  |不修改|不修改
mixed|修改|修改|不修改|
hard|修改|修改|修改

```
$ git reset --hard 
```
