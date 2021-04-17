### 事件处理
- 分类
  - 键盘事件
  - 鼠标事件
- 获取事件
```
var evt = Event.current;
```

### FAQ
- Input的事件机制在编辑器模式下还生效吗？
  - 不生效了

### 代码
- 不是我想水文章，这个模块的机制相当单纯，但是开发的时候有很多业务事件处理并不单纯。
- 下面的代码展示了编辑器下处理事件的方式
```
using System;
using UnityEditor;
using UnityEngine;

namespace DC.DCIMGUIBox
{
    public class EventProcessWindow : EditorWindow
    {
        [MenuItem("DC/IMGUI/EventProcessWindow")]
        public static void Open()
        {
            var window = GetWindow<EventProcessWindow>();
            window.minSize = new Vector2(800, 600);
        }

        private string content = "";

        private Vector2 scrollVPos;

        public void OnGUI()
        {
            var evt = Event.current;

            content += string.Format("evt.type {0}, {1}\n", Enum.GetName(typeof(EventType), evt.type),
                Enum.GetName(typeof(EventType), evt.rawType));

            if (content.Length > 1000)
            {
                content = content.Substring(content.Length - 1000);
            }

            if (evt.type == EventType.MouseDrag)
            {
                Debug.Log(evt.mousePosition);
            }

            scrollVPos = GUILayout.BeginScrollView(scrollVPos, GUILayout.Height(400));
            GUILayout.TextArea(content);
            GUILayout.EndScrollView();

            if (GUILayout.Button("Test Input"))
            {
                Debug.Log("Input.mousePosition " + Input.mousePosition);
            }

            if (Input.GetKeyDown(KeyCode.A))
            {
                Debug.Log("key down a");
            }
        }
    }
}
```
