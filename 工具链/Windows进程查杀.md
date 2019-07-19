# Windows 进程查杀
- 使用情景
  - 从任务管理器找不到想要杀死的进程
  - 从任务管理器进程太多，很难找
  - 解决端口占用问题

## 根据端口查找网络连接 查找指定端口的进程
- netstat -nao|findstr "1080"
- 这个命令可以查找进程的网络连接信息
- 可以用来 查找指定端口的进程
- 例子
- 查找端口为1080的网络连接
- ```netstat -nao|findstr "1080"```

## 查找进程
- tasklist|findstr "content"
- 从进程列表中查找包含指定字符串的进程
- 例子
  - 查找包含"5336"的进程
  - ```tasklist|findstr "6336"```

## 结束进程
- taskkill /pid pid -f
- 例子
  - 杀死进程id为 5366 的进程
  - ```taskkill /pid 5366 -f```