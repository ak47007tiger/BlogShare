```
dotnet --info
dotnet --version
use 4.6 protobuf.dll

git restore method
```
```
Assembly 'Assets/Blogs/UseProto/Plugins/Google.Protobuf.dll' will not be loaded due to errors:
Unable to resolve reference 'System.Memory'. Is the assembly missing or incompatible with the current platform?
Reference validation can be disabled in the Plugin Inspector.

```



## 序列化协议要解决的问题
- 值类型
- 自定义类型
- 引用类型
- array
- map
- nested