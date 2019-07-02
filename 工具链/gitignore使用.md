# gitignore说明使用和例子 忽略文件的方法

## 位置
- .gitignore文件可以出现在任何文件夹下，不一定非要是根目录
- 可以有多个这样的文件来配置不同的目录的忽略规则

## 注释
```
# 注释内容 忽略所有的a.o文件
a.o
```

## 忽略文件
- 忽略单一文件
```
# 忽略从当前目录开始的a目录下的d.txt文件
/a/d.txt
```
- 忽略某个后缀名文件
```
# 忽略所有后缀为.a的文件
*.a
# 忽略所有后缀为.d或者.o的文件
*.[do]
```

## 忽略文件夹和其中的文件
- 忽略单一文件夹
```
# 忽略从根目录开始的文件夹c下的文件夹d
/c/d/
```
- 忽略某个名称的文件夹
```
# 忽略所有的build文件夹
build/
```

## 不忽略文件
```
# 所有的a.cfg文件都不忽略
!a.cfg
# 
# 所有的后缀为.out的文件都不忽略
!*.out

# 忽略build文件夹但是保留其中的a.txt文件
/build/
!/build/a.txt
```

## 通配符
```
# 忽略从当前文件夹开始的doc文件夹下和其子文件夹下所有文件后缀为 .pdf 的文件
doc/**/*.pdf
```

## 参考资料
- https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository

## 各种工程常用的模板
- https://github.com/github/gitignore