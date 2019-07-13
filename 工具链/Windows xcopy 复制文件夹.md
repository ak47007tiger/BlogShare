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
  - 复制过程中不包含某些文件
    - `xcopy /s/y/exclude:excludefileslist.txt`
    - excludefileslist.txt
      - 这个文件里面是列表，标记不想复制的文件是什么，执行xcopy的时候指出这个过滤文件的路径即可
      - 上面的命令表明过滤文件和命令执行的位置相同
      - 下面的例子展示了复制文件夹夹的时候不要复制里面后缀为.log的文件
      ```
      .log\
      ```