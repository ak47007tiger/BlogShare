# Linux 复制文件命令 cp 例子

## 复制单个文件
- ```cp a.txt b.txt```

## 复制多个文件
- 将3个文本复制到d文件夹
- ```cp a.txt b.txt c.txt d/```

## 复制文件夹
- ```cp -R a/ b```

## 复制多个文件夹
- ```cp a b c d```
- 会把a b c三个文件夹复制到d文件夹下

## 备份文件
- 使用 -b
- ```cp -b a.txt b.txt```

## 复制文件时如果会覆盖文件，使用确认提示
- 使用 -i
- ```cp -i a.txt b.txt```

## 创建hard link
- 使用 -l
- cp -l a.txt b.txt
- 此时打开b.txt并且改了，a.txt的内容也会变化

## 复制文件属性
- 使用 --preserve
- ```cp --preserve a.txt b.txt```

## 显示正在复制的文件
- 使用 -v
- ```cp -r -v a b```

## 参考链接
https://shapeshed.com/unix-cp/#how-to-copy-multiple-directories