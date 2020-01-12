## 原理
## 

## 概览
- 基础
- 优化luajit
  - 本地编译
  - 加密

## lua语法

## lua面向对象

## lua内置对象

## how to
- ide
- debug
- lua加载
  - custom loader
- 热更新原理
  - Mono提供的一套C#的API函数，对已经编译过的.Net体系生成的DLL文件进行修改
  - 新代码查询lua环境是否有替换执行的方法，使用这个dll进行打包发布
- 热更新hotfix方案
  - 需要打补丁的使用hotfix
  - 适用于预先确定需要热更
  - 可以用c#编写，需要修改代码则启动补丁热更
- 热更新lua方案
  - 需要热更的地方直接用lua写
  - 更强大，但是要写很多lua代码
- lua call csharp
  - 函数资源怎么管理的，如何进行注册和销毁的