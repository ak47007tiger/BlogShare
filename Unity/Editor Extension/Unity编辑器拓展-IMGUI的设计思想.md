## IMGUI说明
- 全称Immediate Mode GUI
- 这个体系的ui控件没有状态，每次更新绘制都是由窗口将数据对象传到控件，然后控件直接返回新的值来更新数据对象
- 适用范围
  - 制作编辑器
  - 游戏内工具型的ui，比如Game Manager功能的ui

## 特征
- 无状态
- 对动画支持弱

## 编程范式
```
//比如有一个复杂的控件，这个控件接受输入并且返回输出的值
class DataModel{
    ...
}

class XXEditorWindow{
    DataModel dataModel = new DataModel()

    void OnGUI(){
        DataModel = GUI_CustomControl(dataModel)
    }

    DataModel GUI_CustomControl(DataModel dataModel){
        // process input, layout, show information, setup DataModel
        ...
        return dataModel
    }
}

```

## 参考资料
- https://caseymuratori.com/blog_0001
- https://github.com/ocornut/imgui