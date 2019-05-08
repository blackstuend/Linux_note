# Spinning tree protocol (stp)
* [參考網站](https://www.jannet.hk/zh-Hant/post/spanning-tree-protocol-stp/)
* 邏輯上形成tree的架構，但是實際上是迴圈的方式，同時也避免brodadcast storm

## PVST+
* CSICO預設的STP設定
* 選擇root port時，會找相對短的路徑來選擇
* switch root 選擇會挑選switch priority，挑選最小的
* switch priority 計算是 預設的32768 + 上裡面的vlan
