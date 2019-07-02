# git 关联本地分支和远程分支

## 添加远程分支
- git remote add 地址 远程仓库别名
```
//加一个远程仓库并且命名为github
git remote add xxx.xxx.xx(远程仓库地址) github
```

## 关联
- git branch --set-upstream-to=origin/remote_branch  your_branch
- 说明
  - git branch --set-upstream-to=远程仓库别名/远程分支名 本地分支名
- 实例
```
//把本地主分支关联到远程仓库的主分支
git branch --set-upstream-to=github/master master

//把本地主分支关联到远程仓库的b2分支
git branch --set-upstream-to=github/b2 master

//把本地b2分支关联到远程仓库的rb2分支
git branch --set-upstream-to=github/rb2 b2
```