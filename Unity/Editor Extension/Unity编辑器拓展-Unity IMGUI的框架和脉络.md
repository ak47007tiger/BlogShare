## 小知识
- Unity公司编写了基于IMGUI设计思想的ui框架并用其制作了UnityEditor
- 早期的Unity的用户ui也多是用IMGUI实现
- IMGUI性能比ngui和ugui低，现在的新项目应该没人再用ngui了
- 现在Unity又出了新的ui方案，统合了过去unity官方支持的ui
- 我估计离世面上的ui框架更新换代不远了，又是一波技术进步

## Unity IMGUI的框架
- 在Monobehaviour或者Window子类里定义OnGUI方法，在OnGUI方法里面就可以使用Unity的api创建ui
- 复写Editor类的OnInspectorGUI方法也有类似效果，这个常用于自定义某个Mono脚本的编辑器

### 核心类：创建窗口、控件，进行布局，修改样式
  - EditorWindow，自定义窗口必须是EditorWindow的子类，使用GetWindow<XXWindow>创建
  - GUI，没有自动布局，可以创建控件，可以设置获取GUI的状态
  - GUILayout，有自动布局，可以创建控件
  - GUILayoutUtility，对GUILayout拓展
  - EditorGUI，功能更多的GUI
  - EditorGUILayout，功能更多的GUILayout
  - EditorGUIUtility，对GUILayout的拓展
  - Handles，绘制场景中的3D ui
- 自动布局是指什么
  - 可以指定可伸缩的尺寸信息让控件自动进行布局
  - 使用GUILayout下的方法创建GUILayoutOption来设置布局属性
- 在编辑器里创建GUISkin，创建控件的时候传到控件里就可以改变样式

## 实例
- 更多例子可以在下方参考资料中unity官方文档找到
```
using UnityEditor;
using UnityEngine;

namespace DC.DCIMGUIBox
{
    public class LayoutAndAreaWindow_01 : EditorWindow
    {
        [MenuItem("DC/IMGUI/LayoutAndAreaWindow_01")]
        public static void Open()
        {
            var window = GetWindow<LayoutAndAreaWindow_01>();
            window.minSize = new Vector2(800, 600);
        }

        public void OnGUI()
        {
            GUILayout.BeginArea(new Rect(0, 0, 200, 200));
            EditorGUI.DrawRect(new Rect(0, 0, 100, 100), Color.blue);
            if (Event.current.type == EventType.MouseDown)
            {
                Debug.Log("box a" + Event.current.mousePosition);
            }

            GUILayout.EndArea();


            GUILayout.BeginArea(new Rect(210, 0, 200, 200));
            EditorGUI.DrawRect(new Rect(0, 0, 100, 100), Color.red);
            if (Event.current.type == EventType.MouseDown)
            {
                Debug.Log("box b" + Event.current.mousePosition);
            }

            GUILayout.EndArea();
        }
    }
}
```

## 参考资料
- https://docs.unity3d.com/Manual/GUIScriptingGuide.html