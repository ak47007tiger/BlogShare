# Unity onBeforeRender自定义渲染前回调顺序

## 知识点
- Application.onBeforeRender
  - 一个回调列表，用来加需要回调的方法
- BeforeRenderOrder
  - 这是一个Attribute，加到方法上即可启用，可以用来指定回调方法在列表中被执行的优先级

## 代码
```
public class TestApplication : MonoBehaviour
{
    void Start()
    {
        Debug.Log("Start");
        Application.onBeforeRender += R1;
    }

    [BeforeRenderOrder(1)]
    void R1()
    {
        Debug.Log("r1");
    }
}

public class TestApplication_Copy : MonoBehaviour
{
    void Awake()
    {
        Debug.Log("Awake");
        Application.onBeforeRender += R2;
    }

    [BeforeRenderOrder(2)]
    void R2()
    {
        Debug.Log("r2");
    }
}

```

## 验证结果
- R2方法在Awake里加入回调列表
- R1方法在Start里加入回调列表
- R2比R1先加入但是，应用了BeforeRenderOrder后，R1先执行