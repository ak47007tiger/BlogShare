## plan
- 渔夫的视频教程
- 渔夫的帧同步框架


## 重点
- 网络、协议
  - tcp、udp
- 序列化
- 客户端同步

## 网络数据收发
- 多线程处理方式
- 主线程和网络线程数据读写同步

## 主逻辑
- MainScript
- Launcher
  - FrameBuffer
    - DoUpdate
      - NetworkService
        - ShendPing
      - PredicCountHelper
        - DoUpdate
      - UpdatePingVal
  - SimulatorService
    - DoClientUpdate
    - Simulate
      - Step
      - DumpFrame
      - ProcessInputQueue

## ui和数据的模式
- 请求数据后打开ui
  - 预先请求好数据等ui被打开
  - 请求数据完打开ui
- 打开ui，请求数据，刷新ui

## 原理

## howto
### 怎么推进游戏逻辑
### 怎么预测
### 怎么回滚

## 帧同步网络
```
寻找房间
加入房间
同步游戏
开始回合
```
### 房间
### 消息
### 断线重连
### ping