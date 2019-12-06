## 本文介绍unity shader 内置变量 _ProjectionParams的各个数据含义
- 源码
```
// x = 1 or -1 (-1 if projection is flipped)
// y = near plane
// z = far plane
// w = 1/far plane
float4 _ProjectionParams;
```
- x 表明是不是反向投射
- y 近剪裁面在view空间(相机空间)的z值，数值上等于相机设置中的近剪裁面的值
- z 远剪裁面在view空间(相机空间)的z值，数值上等于相机设置中的远剪裁面的值
- w 1/z的值