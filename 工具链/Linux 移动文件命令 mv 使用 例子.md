# Linux 移动文件命令 mv 使用 例子
- 移动文件
- 移动文件夹
- 文件冲突处理

## 移动文件
- mv a.txt b.txt
  - a.txt 被重命名为 b.txt

## 移动文件到文件夹
- mv a.txt d
  - d是文件夹
  - a会被移动到文件夹d

## 移动多个文件
- mv a.txt b.txt c.txt d
- mv *.txt d
  - 移动所有后缀为.txt的文件到文件夹d
  
## 移动文件夹d到文件夹e
- mv d e

## 移动文件时发生文件覆盖使用提示
- mv -i a.txt b.txt
  - 如果已经有b.txt会提示是否覆盖

## 跳过会被覆盖的文件
- mv -n a.txt b.txt
 - 如果有b.txt，跳过
  
## 仅覆盖移动比被覆盖的文件新的文件
- mv -u a.txt b/a.txt
  - 如果b/a.txt比a.txt更新，文件不会被移动

## 移动时如果冲突备份文件
- mv -b a.txt b.txt
  - 如果文件b.txt已经存在，备份b.txt为 b.txt~ 然后移动a.txt为b.txt
- 指定被备份的文件的后缀
  - mv -S .bak -b a.txt b.txt
    - 此时被备份的文件的后缀被设置为.bak即会出现b.txt.bak