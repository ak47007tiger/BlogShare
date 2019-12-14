## ProtoBuff简介
```
用于序列化结构数据的技术。使用特定的生成的代码，用不同的语言读写不同的数据流。
```

## 资源下载
- https://github.com/protocolbuffers/protobuf

## 学习资料
- https://developers.google.com/protocol-buffers/docs/overview?hl=zh-CN

## 工具准备
- win64的电脑
- protobuf源码
- 对应版本的visual studio
- python环境，本文用python做批处理。因为我要编译一个目录下所有的文件，并且保持目录结构输出编译后的文件
- 下载编译工具 https://github.com/protocolbuffers/protobuf/releases
  - windows64的选win64版本的下载

## 生成unity使用protobuff的DLL
- 使用visual studio打开对应语言的工程，比如我的地址是 protobuf\csharp\src\Google.Protobuf.sln
  - 打开这个目录下面的visual studio解决方案，生成解决方案
  - 我用的net45版本的DLL

## 编译出csharp源码
### 编写proto的配置文件，proto的语法在学习资料里面有
- Person.proto
```
syntax="proto3";
package protobuf;

message Person {
  int32 id = 1;
  string name = 2;
  string email = 3;
}
```
- Hello.proto
```
syntax="proto3";
package protobuf;

message Hello {
  int32 id = 1;
  string name = 2;
  string email = 3;
}
```
### 使用工具准备里面下载的编译工具编译出源码
- 我写的批处理用的python脚本
```
# -*- coding: utf-8 -*-
import subprocess, os, sys
"""
example
protoc.exe -I=%srcDir% --csharp_out=%dstDir% %srcDir%/Person.proto

保持src目录中的文件目录结构编译到dst中

"""

proto_exe_path = sys.argv[1]
src_dir = sys.argv[2]
dst_dir = sys.argv[3]

def getOutputDir(srcDir,dstDir,filePath):
  outputPath = filePath.replace(srcDir, dstDir).replace("\\","/")
  outDir = outputPath[:outputPath.rfind('/')]
  return outDir

def getRelativePath(srcDir, filePath):
  rPath = filePath[len(srcDir) + 1:]
  return rPath

def getAllProtoFiles(dir, extension):
  pathList = []
  for file in os.listdir(dir):
    childFile = os.path.join(dir,file)
    if os.path.isdir(childFile):
      pathList.extend(getAllProtoFiles(childFile, extension))
    elif os.path.splitext(childFile)[1] == extension:
      pathList.append(childFile.replace('\\','/'))
  return pathList

def complieProto(srcDir,dstDir,filePath):
  outputDir = getOutputDir(srcDir, dstDir, filePath)
  if not os.path.exists(outputDir):
    os.makedirs(outputDir)
  cmdStr = '{0} -I={1} --csharp_out={2} {3}'.format(proto_exe_path, srcDir, outputDir, filePath).replace('/','\\')
  return cmdStr

files = getAllProtoFiles(src_dir,".proto")
print "count src files: ", len(files)
cmdFile = open("generate_complie.bat","w")
for filePath in files:
  cmdStr = complieProto(src_dir, dst_dir, filePath)
  # os.system(cmdStr)
  cmdFile.write(cmdStr)
  cmdFile.write('\n')
cmdFile.write("pause\n")
cmdFile.close()
print("generate complete!")
subprocess.call("generate_complie.bat")
```

- 我写的调用python脚本的bat脚本，proto_exe_path是下载的编译程序所在位置
```
set proto_exe_path="D:/hpl_projects/github_projects/protoc-3.9.1-win64/bin/protoc.exe"
set src_dir="D:/hpl_projects/github_projects/prototest/src"
set dst_dir="D:/hpl_projects/github_projects/prototest/output"
python proto_batch.py %proto_exe_path% %src_dir% %dst_dir%
pause
```

## 在Unity中使用
- 复制编译出DLL到unity下的Assets/Plugings/里面
- 复制编译出的源码到untiy工程，可以用脚本做自动拷贝操作
- demo代码如下
```
using Google.Protobuf;
using Protobuf;
using UnityEngine;

public class TestProtoBuf : MonoBehaviour
{
    void Start()
    {
        Person p = new Person();
        p.Id = 1;
        p.Name = "hello";
        p.Email = "xx@gmail";

        var byteArray = p.ToByteArray();
        var p2 = (Person)Person.Descriptor.Parser.ParseFrom(byteArray);
        Debug.Log(p2.Id);
        Debug.Log(p2.Name);
        Debug.Log(p2.Email);
    }
}
```

## 完