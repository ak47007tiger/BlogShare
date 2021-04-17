### 拖拽
- 2种做法
  - 使用ChildWindow
  - 自定义控件处理鼠标拖拽事件处理完成对控件位置的改变

### 效果图
- 点击add创建子窗口
- 在选中窗口后把当前选中窗口和之前选中窗口用线链接起来
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417110512509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrNDcwMDd0aWdlcg==,size_16,color_FFFFFF,t_70#pic_center)


### 本文介绍使用ChildWindow的拖拽
```
using System;
using System.Collections.Generic;
using UnityEditor;
using UnityEngine;

namespace DC.DCIMGUIBox
{
    public class WindowNodeData
    {
        public Rect rect;

        public bool selected;
    }

    public class DragWindow : EditorWindow
    {
        [MenuItem("DC/IMGUI/DragWindow")]
        public static void Open()
        {
            var window = GetWindow<DragWindow>();
            window.minSize = new Vector2(800, 600);
        }

        public List<WindowNodeData> nodeDataList = new List<WindowNodeData>();

        List<Tuple<WindowNodeData, WindowNodeData>> lines = new List<Tuple<WindowNodeData, WindowNodeData>>();

        private WindowNodeData lastSelectedWindow = null;

        public void OnGUI()
        {
            GUILayout.BeginHorizontal();
            if (GUILayout.Button("add"))
            {
                nodeDataList.Add(new WindowNodeData() {rect = new Rect(100, 100, 200, 100)});
            }

            GUILayout.EndHorizontal();

            var col = new Color(0.8f, 0.3f, 0.3f);
            for (int i = 0; i < lines.Count; i++)
            {
                var line = lines[i];
                var startPos = line.Item1.rect.center;
                var endPos = line.Item2.rect.center;
                DrawConnectLine(new Vector3(startPos.x, startPos.y), new Vector3(endPos.x, endPos.y), col);
            }

            BeginWindows();

            for (int i = 0; i < nodeDataList.Count; i++)
            {
                nodeDataList[i].rect = GUILayout.Window(i, nodeDataList[i].rect, WindowNode, i.ToString());
            }

            EndWindows();
        }

        public static void DrawConnectLine(Vector3 startPos, Vector3 endPos, Color color)
        {
            var startTangent = new Vector3(startPos.x + Mathf.Sign(endPos.x - startPos.x) * 10, startPos.y, startPos.z);

            var endTangent = new Vector3(endPos.x + Mathf.Sign(endPos.y - startPos.y) * 10, endPos.y, endPos.z);

            Handles.DrawBezier(startPos, endPos, startTangent, endTangent, color, CustomControls.texture2d, 1);
        }

        public void OnNodeSelected(WindowNodeData data)
        {
            for (int i = 0; i < nodeDataList.Count; i++)
            {
                if (nodeDataList[i] == data)
                {
                    continue;
                }

                nodeDataList[i].selected = false;
            }

            Repaint();

            if (lastSelectedWindow != null)
            {
                lines.Add(new Tuple<WindowNodeData, WindowNodeData>(lastSelectedWindow, data));
            }

            lastSelectedWindow = data;
        }

        public void WindowNode(int windowId)
        {
            var data = nodeDataList[windowId];

            GUI.DragWindow(new Rect(0, 0, data.rect.width, 16));
            var curEvt = Event.current;

            if (curEvt.type == EventType.MouseDown && curEvt.button == 0 &&
                new Rect(0, 16, data.rect.width, data.rect.height - 16).Contains(curEvt.mousePosition))
            {
                data.selected = true;
                OnNodeSelected(data);
            }

            if (data.selected)
            {
                EditorGUI.DrawRect(new Rect(0, 0, data.rect.width, data.rect.height), new Color(0.6f, 0, 0.4f, 0.3f));
            }

            GUILayout.Label(windowId.ToString());
        }
    }
}
```