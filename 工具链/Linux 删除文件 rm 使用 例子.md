# Linux 删除文件 rm 使用 例子
- 删除文件
- 删除文件夹

## 删文件
- 单个
  - ```rm a.txt```
- 多个
  - ```rm a.txt b.txt```
- 带删除确认的提示
  - ```rm -i a.txt```
- 删除时显示删除文件的操作
  - ```rm -v a.txt```

## 删文件夹a和里面所有的内容
- ```rm -R a/```
- 带权限的删除文件夹
  - 如果报错 rm: cannot remove 'a': Permission denied
  - 使用命令```sudo rm a/```

## 强制删除文件a、文件夹a
- ```rm -f a```
- ```rm -f a/```

## 删除文件夹a和里面所有的内容，包括其子文件夹
- ```rm -Rf a/```