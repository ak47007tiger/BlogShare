## excel to protobuf data
- excel to csv
- parse proto
- to bytes file
  - 数组
    - 从csv查找数组长度，逐个读取根据类型写入
  - 引用类型
    - 指定开始列结束列递归执行写入二进制的过程
  - 枚举
    - 写入枚举类型的值
- 如何知道1个对象占多长的二进制，需要在对象二进制开始前给出长度
- 根据proto文件生成对应语言的序列化和反序列化代码

## 二进制数据
- 字符串解析
  - 字符串长度不定

## protocol cfg
```
1000001=Person
```

王利
18682336281