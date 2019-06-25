# Unity Test Runner
## 概述
- 入口菜单：Window > General > Test Runner
- NUnit：Microsoft .NET的开源单元测试框架
- UnityTestAttribute
  - 可以在测试时跳过一些帧

## Known issues and limitations 问题和限制
- WebGL and WSA 不支持 UnityTestAttribute
- UnityTest 不支持 Parameterized tests
  - Parameterized tests
    - 在测试用例类中提供数据几个作为输入数据
    - 数据集中一般不止一组数据，框架会用这些数据多次测试被测试的类

## How to use Unity Test Runner 使用方法
- 流程
  - 如果工程中没有测试，Create Test Script in current folder
  - 创建测试脚本：Assets > Create > C# Test Script
  - 当使用“Run on <Platform>”时Unity才会包含测试框架需要的程序集
- Testing in Edit mode
  - 合法的测试脚本的位置
    - Project Editor folder
    - 仅供Editor使用的程序集，并且引用测试框架程序集(DLL)
      - 这个是可以在一个窗口里面配置，指明哪些文件夹引用测试框架，可以放置测试脚本
    - 在Project Editor folder下的预编译的程序集(DLL)
- Testing in Play mode
  - unity可以控制所有最终的dll都引入测试框架的dll，设置完重启生效
    - 导致工程变大，构建时间增加
  - 构建最终的工程时需要移除引用的测试框架

# Writing and executing tests in Unity Test Runner

## UnityTestAttribute

## UnityPlatformAttribute

## LogAssert

## MonoBehaviourTest

## Running tests on platforms

## Running from the command line

## Comparison utilities
- UnityEngine.TestTools.Utils 可以用来比较基本数据类型，float，Vector系列
- 可以创建原始类型，比如一个空cube

# 总结