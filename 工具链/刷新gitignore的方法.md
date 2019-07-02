# 刷新gitignore并使其生效的方法
## 问题
- 已经把文件加入git仓库
- 又已经**加进去但是没有提交的文件**想要忽略
- 如何修改gitignore使其生效

## 操作
- 编辑gitignore文件
- 从缓存中移除要忽略的文件
- 提交gitignore文件

## 情景
- 执行了 git add A.log 后，想要忽略掉 A.log
```
1 git rm --cached A.log
2 git add .gitignore
3 git commit -m"更新了gitignore"
```
- 即使在第2步执行 ```git add .``` 这个命令，A.log也不会被加到git仓库了

## 说明
- git rm --cached A.log
  - 不从磁盘删除文件，只是移出改变跟踪区
- git rm A.log 从磁盘删除文件