
git分支
------------
###1.命名
#####形式
`(remote)/(branch)`
#####组成
**master**：运行`git init`时默认的`起始分支`名字
**origin**： `git clone`时默认的`远程仓库`名字

`git clone -o booyah`:更改默认的远程分支名字为booyah/master
`git remore add team1`:添加一个新的远程仓库引用到当前的项目
###2.命令
`git fetch origin`:这个命令查找'origin'是哪一个服务器，从中抓取本地 没有的数据，并且更新本地的数据库，**移动**`origin/master`指针指向新的、更新后的位置。

