# 并发处理技术 CAS操作

## 简介
- 非阻塞的同步技术
- 没有锁

## CSA
- 原来的值A
- 内存值V
- 写入的新值B

## 算法流程
- 为a写入一个新值 B
- 读取：A = a
- 如果 A 和 a在内存中的值一样，执行写入a = B
- 如果 A 和 a在内存中的值不一样，重复这个流程

## 实例
- 1
```
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));

    return var5;
}
```
- 2
```
while(ture){
    var A = a;
    if(a.compareAndSet(A,B)){
        return;
    }
}
```

## 局限
- ABA问题：A被修改为B时，又被修改为A，CAS会回误以为该值没有被改变过
- cpu消耗
- 只能保证一个共享变量的原子性

## 适用
- 简单的并发操作
- 非阻塞的并发操作

