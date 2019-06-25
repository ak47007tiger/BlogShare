# Unity C# Job System 介绍 指南 手册 笔记

# 概述
- 用于做多线程，可以提高性能
- 配合 Entity Component System (ECS) 使用
- 配合 Burst compiler

# 概念
## job system
- 一个受管理的job间可以有依赖关系的多线程的任务执行框架
- job
  - 一个执行单元，传入job system并根据配置被安排执行
- job dependencies
  - 不同job可以有顺序上的依赖，比如a在b之后才能执行

# job system和多线程安全
- 当2个线程可能同时读写一段内存时，竞争条件出现，可能导致难以查询的bug
- job system通过复制数据的方式使不会出现同时访问同一地址的情况
  - 这些数据一般是结构体

# 机制
- c# job system 和 Unity’s native job system 交互，限制线程数量避免线程资源竞争

# 好处
- 提高帧率
- 省电

# NativeContainer
## 简述
- 共享内存，用来解决数据是副本的问题
- 可以让job和主线程访问同一段内存数据
## 类型
- NativeArray
- ECS拓展的类型
  - NativeList
  - NativeHashMap
  - NativeMultiHashMap
  - NativeQueue
  
## NativeContainer and the safety system
- DisposeSentinel：处理内存释放问题
- AtomicSafetyHandle：处理内存写入竞争问题
  - 多个job并行写入同一个NativeContainer时
  - job和主线程写入同一个NativeContainer时
  - 同时读取的job不会导致异常
- 可以通过job依赖配置解决读写竞争问题
- 可以同时读写的job会降价性能
  - 如果是只读的job就只配置读取能力
- 静态数据的访问不受安全系统保护
- 配置进读取的方法
```
[ReadOnly]
public NativeArray<int> input;
```

## NativeContainer Allocator
- 使用不同的Allocator是为了在不同的使用情景下获得最好的性能
- 类型
  - Temp，最快，适用于 <=1 帧的时间内，并且一定要在方法返回前释放，不可以传给Jobs
  - TempJob，比Temp慢，适用于 <=4 帧的时间内，大部分小型job使用
  - Persistent，最慢，整个应用的生命周期时间都可以用，性能敏感的地方避免使用这种类型

# Creating jobs
- 步骤
  - 创建struct : IJob
  - 增加成员变量
  - 创建Execute方法
    - 在一个cup上运行1次
- 注意点
  - 通过在job中通过NativeContainer方法主线程的数据
```
// Job adding two floating point values together
public struct MyJob : IJob
{
    public float a;
    public float b;
    public NativeArray<float> result;

    public void Execute()
    {
        result[0] = a + b;
    }
}
```

# Scheduling jobs
- 步骤
  - 创建job
  - 准备数据
  - 调用Schedule
- 注意
  - 一旦调用Schedule，job就会被加入job队列。不能打断一个job
  - Schedule只能在主线程调用
- 示例
```
// Create a native array of a single float to store the result. This example waits for the job to complete for illustration purposes
NativeArray<float> result = new NativeArray<float>(1, Allocator.TempJob);

// Set up the job data
MyJob jobData = new MyJob();
jobData.a = 10;
jobData.b = 10;
jobData.result = result;

// Schedule the job
JobHandle handle = jobData.Schedule();

// Wait for the job to complete
handle.Complete();

// All copies of the NativeArray point to the same memory, you can access the result in "your" copy of the NativeArray
float aPlusB = result[0];

// Free the memory allocated by the result array
result.Dispose();
```

# JobHandle and dependencies
- job依赖单个job设置
```
JobHandle firstJobHandle = firstJob.Schedule();
secondJob.Schedule(firstJobHandle);
```
- job依赖多个job设置
```
NativeArray<JobHandle> handles = new NativeArray<JobHandle>(numJobs, Allocator.TempJob);

// Populate `handles` with `JobHandles` from multiple scheduled jobs...

JobHandle jh = JobHandle.CombineDependencies(handles);
```
- JobHandle.ScheduleBatchedJobs
  - 立即执行多个jobs
  - job一般被放到一个队列，短暂的延迟后会被工作者线程执行
  - 这个方法会带来一定的性能开销
- Waiting for jobs in the main thread
  - JobHandle.Complete
    - 使主线程等待工作者线程执行完一个job
    - 被依赖的job也会执行

# ParalleFor jobs
- 概述
  - 通过拆分任务使多个线程同时执行这些任务，先完成任务的线程会继续帮助没完成任务的线程分担任务
- 步骤
  - 实现 IJobParallelFor 接口
  - 添加方法 public void Execute (int index)
    - 主要是通过index来拆分任务
- 机制
  - 通过index拆分任务，让多线程并行执行
  - unity中有native job，这些native job执行被拆分的job

# ParalleForTransform jobs
- 类似paralle job，不同在于这个专门处理Transform
- 不同于parallel job的步骤
  - 实现接口 IJobParallelForTransform
  - 创建方法 public void Execute(int index, Jobs.TransformAccess transform);
- TransformAccess
  - 这个结构包含了处理一个transform的必要信息，位置，旋转，缩放

# c# Job System tips and troubleshooting
- Do not access static data from a job
  - 不能访问静态数据，比如单例对象，静态变量
- Flush scheduled batches
  - 如果想立即执行job，使用JobHandle.ScheduleBatchedJobs，但会带来性能损耗
  - 其它情况使用 JobHandle.Complete
  - 在ECS中batch是被系统自动flushed的，不需要手动刷
- Don’t try to update NativeContainer contents
  - 正确操作：复制到一个变量，更改，赋值回去
  - 代码
    ```
    MyStruct temp = myNativeArray[i];
    temp.memberVariable = 0;
    myNativeArray[i] = temp;
    ```
  - 原因： https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/ref-returns
- Call JobHandle.Complete to regain ownership
  - 通过 JobHandle.Complete 获得 NativeContainer的所有权，否则会内存泄漏
  - 当前帧的job可以依赖上一帧的job
- Use Schedule and Complete in the main thread
  - 只在主线程调用 Schedule 和 Complete
  - 不在job中 Schedule 其它 job
- Use Schedule and Complete at the right time
  - Schedule可以任何时候调用，Complete在需要job结果的时候调用
  - 在没有操作的时机调用Schedule
    - 每帧结束前获得job结果，在下一帧开始前使用结果
    - 当没有其它耗时任务
- Mark NativeContainer types as read-only
  - 对NativeContainer使用read-only的权限可以提高性能
- Check for data dependencies
  - Unity Profile window -> "WaitForJobGroup" 表明主线程因工作者线程阻塞了
  - 通过JobHandle.Complete被调用的地方追查到导致等待的代码
- Debugging jobs
  - 使用Job的Run方法可以在主线程执行job
- Do not allocate managed memory in jobs
  - 在job中申请收到托管的内存是非常的慢，而且不能使用Unity Burst Compiler来提高性能
  - Burst可以使用特定平台的功能来提升性能
- 原始链接：https://docs.unity3d.com/Manual/JobSystemTroubleshooting.html