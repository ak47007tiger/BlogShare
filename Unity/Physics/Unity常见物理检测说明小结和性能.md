## 文档地址
- https://docs.unity3d.com/ScriptReference/Physics.html

## 常用物理检测方法说明
- 形状组别
  - Ray
  - Line
  - Box
  - Capsule
  - Sphere
- 功能组别
  - CheckXXX：XXX是否有和其它collider重叠
  - OverlapXXX：获得所有和XXX重叠的collider
  - OverlapXXXNonAlloc：获得所有和XXX重叠的collider，但是分配新的数组，使用缓存
  - XXXCast：获得XXX形状的碰撞信息，可以用来检测是否碰撞并且获取RaycastHit
  - XXXCastAll：获得所有的
  - XXXCastNonAlloc：使用给的缓存存储获得的RaycastHit对象

## 性能说明
### 形状维度
- 消耗从小到大：Box < Sphere < Capsule
### 功能维度
- 消耗从小变大：CheckXXX < OverlapXXX < XXXCast

## 一个demo，使用buff来获得碰撞检测的结果
```
public void TestPhysics()
{
    var buff = new RaycastHit[50];
    var realCnt = Physics.BoxCastNonAlloc(Vector3.zero, new Vector3(10, 1, 8), Vector3.forward, buff);

    for (var i = 0; i < realCnt; i++)
    {
        Debug.Log("get "+ buff[i].transform.gameObject.name);
    }
}
```