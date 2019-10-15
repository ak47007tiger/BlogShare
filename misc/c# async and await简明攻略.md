## 异步处理语法糖
- 方便进行开启异步任务并等待结果完成的操作
- async 表明该方法是异步方法
- await 表面要等待task完成再执行后续代码

## 特性
- async 修饰method
- await 
  - 修饰返回值进行等待
  - 修饰Task表明是异步任务

## 使用
- 返回void类型
```
async Task TestVoid()
{
    await Task.Run(() =>
    {
        Thread.Sleep(2000);
        Console.WriteLine("TestVoid()方法里面线程ID: {0}", Thread.CurrentThread.ManagedThreadId);
        return "我是返回值";
    });
}
```
- 返回值类型
```
async Task<int> ComputeValue()
{
    return await Task.Run(() =>
    {
        Thread.Sleep(1);
        return 0;
    });
}
```
- 开启异步，等待完成调用方式
```
//顺序执行，TestVoid里面的耗时操作完成之前后面的代码不会执行，即ComputeValue()不会执行
await TestVoid();
var valu = await ComputeValue();
//不await，TestVoid直接执行，就像开启了一个新线程不等待结果一样
TestVoid();
var valu = await ComputeValue();
```