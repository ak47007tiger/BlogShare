## 概述
- 本文研究创建编辑器窗口所必要的知识

## 贴代码
```
using UnityEditor;
using UnityEngine;

namespace DC.DCIMGUIBox
{
    public class ChildWindowWindow : EditorWindow
    {
        [MenuItem("DC/IMGUI/ChildWindowWindow")]
        public static void Open()
        {
            var window = GetWindow<ChildWindowWindow>();
            window.minSize = new Vector2(400, 400);
        }

        Rect windowRect = new Rect(20, 20, 400, 50);

        Rect windowRect2 = new Rect(320, 120, 120, 50);

        void OnGUI()
        {
            BeginWindows();
            windowRect = GUILayout.Window(0, windowRect, ChildWindow, "CW1");

            windowRect2 = GUILayout.Window(1, windowRect2, ChildWindow, "CW2");
            EndWindows();
        }

        void ChildWindow(int windowID)
        {
            GUI.DragWindow(new Rect(0, 0, windowRect.width, 16));

            if (GUILayout.Button("Btn " + windowID))
            {
                Debug.Log(windowID);
            }
        }

    }
}
```

## 设置窗口属性
```
// 创建之后可以设置支持的属性
var window = GetWindow<ChildWindowWindow>();
window.minSize = new Vector2(400, 400);
```

## 子窗口
- 就是调一个api传如一个类似OnGUI的绘制子窗口的函数
- 传入窗口的id和大小等属性，在回调函数里绘制ui即可，搞你需要的控件上去
- 根据需要可以封装子窗口为对象，这样大小也可以调整，不然子窗口太小不一定显示的全
```
void OnGUI()
{
    BeginWindows();
    windowRect = GUILayout.Window(0, windowRect, ChildWindow, "CW1");

    windowRect2 = GUILayout.Window(1, windowRect2, ChildWindow, "CW2");
    EndWindows();
}

void ChildWindow(int windowID)
{
    GUI.DragWindow(new Rect(0, 0, windowRect.width, 16));

    if (GUILayout.Button("Btn " + windowID))
    {
        Debug.Log(windowID);
    }
}
```

## 菜单
```
// 这样就可以在unity的菜单栏创建一个路径为DC/IMGUI/ChildWindowWindow的菜单了
[MenuItem("DC/IMGUI/ChildWindowWindow")]
public static void Open()
{
    var window = GetWindow<ChildWindowWindow>();
    window.minSize = new Vector2(400, 400);
}
```

## 右键菜单(上下文菜单)
- 右键上下文菜单是unity右键菜单的子集，显示的是unity右键菜单或者右键菜单中的某一组菜单
- 给右键菜单加条目的方法
```
// 和菜单类似，但是要以Assets开头
[MenuItem("Assets/DC/xxx")]
```
- 监听右键事件
```
Event evt = Event.current;
if (evt.type == EventType.ContextClick){
    ...
}
```
- 显示右键菜单
```
// 关键的信息2个，位置，菜单路径
EditorUtility.DisplayPopupMenu(new Rect(mousePos.x, mousePos.y, 0, 0), "Assets/DC", null);
```
- 完整代码
```
using UnityEditor;
using UnityEngine;

namespace DC.DCIMGUIBox
{
    public class PopMenuWindow_01 : EditorWindow
    {
        [MenuItem("DC/IMGUI/PopMenuWindow_01")]
        public static void Open()
        {
            var window = GetWindow<PopMenuWindow_01>();
            window.minSize = new Vector2(800, 600);
        }

        int index = 0;
        string[] options = { "Rigidbody", "Box Collider", "Sphere Collider" };

        void OnGUI()
        {
            Event evt = Event.current;
            Rect contextRect = new Rect(10, 10, 100, 100);
            EditorGUI.DrawRect(contextRect, Color.blue);
            if (evt.type == EventType.ContextClick)
            {
                Vector2 mousePos = evt.mousePosition;
                if (contextRect.Contains(mousePos))
                {
                    EditorUtility.DisplayPopupMenu(new Rect(mousePos.x, mousePos.y, 0, 0), "Assets/DC", null);
                }
            }

            var areaRect = new Rect(0, 200, 200, 40);
            EditorGUI.DrawRect(areaRect, Color.blue);
            index = EditorGUI.Popup(areaRect, "Component:", index, options);

        }
    }
}
```