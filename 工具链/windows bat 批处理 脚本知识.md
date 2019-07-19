# windows bat 批处理 脚本知识

# 查询命令使用方法
- cmd_symbol /?
  - 命令 /?

# bat 批处理 注释
- ::
- rem

# bat 批处理运行完后等待
- pause

# set
## 设置一个批处理脚本中的变量值
- 示例
```
set var=val
echo %var%
pause
```
## 获得用户输入的值
```
set /p var=
echo %var%
pause
```
## 输入表达式
```
set /a var=(3+3)*2
rem 特殊符号运算
set /a var=1'&'2
set /a var=17'%'3
```

# echo 命令
- 显示当前ECHO的状态
  - echo
- 开启 echo on
- 开启回显包括命令本身：@ echo on
- 关闭 echo off
- 关闭回显包括命令本身：@ echo off
- 多行文本
  - ECHO 第一行 &ECHO 第二行
- 覆盖写入文件
  - ECHO 字符串 > 文件路径和文件名
- 覆盖写入多行文本
  - (ECHO 字符串 &ECHO 字符串…)>文件路径和文件名
- 追加写入文件
  - ECHO 字符串 >> 文件路径和文件名
- 追加写入多行文本
  -  (ECHO 字符串)>>文件路径和文件名
  

# call 命令
- 在命令行中调用另一个.bat文件
  - call other.bat
- 在cmd文件中调用另一个.bat文件
  - call other.bat

# xcopy 拓展的复制命令
- 复制目录和下面的文件，保持目录结构
- 不能复制系统文件、隐藏文件
- xcopy 源路径 目标路径
- 常用
  - 复制并且覆盖
    - xcopy /y srcPath dstPath
  - 复制目录和目录下文件并且对目录下的子目录和子目录的文件进行复制
    - xcopy /s srcPath dstPath
  - 复合参数
    - xcopy /s /y srcPath dstPath

# copy 复制命令
- copy srcPath dstPath

# cd 进入文件夹命令