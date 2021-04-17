### 控件、布局
- 自定义控件的2种形式
  - 组合已有的控件处理更复杂的逻辑返回更复杂的数据
  - 自己进行事件处理，绘制独特的界面
- 横向布局，让控件从左往右自动排版
- 纵向布局，让控件从上往下自动排版

### 运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210403211606469.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrNDcwMDd0aWdlcg==,size_16,color_FFFFFF,t_70#pic_center)



### 代码
```
using UnityEditor;
using UnityEngine;

namespace DC.DCIMGUIBox
{
    public class ControlsAndLayoutWindow : EditorWindow
    {
        [MenuItem("DC/IMGUI/ControlsAndLayoutWindow")]
        public static void Open()
        {
            var window = GetWindow<ControlsAndLayoutWindow>();
            window.minSize = new Vector2(800, 600);
        }

        private int toolbar;

        private bool area1Blue = true;

        private bool area2Red = true;

        public Vector2 btnListScrollPos;

        private Gradient gradient = new Gradient();

        public void OnGUI()
        {
            //开始横向布局
            GUILayout.BeginHorizontal(GUILayout.ExpandWidth(true));

            //渐变色
            gradient = EditorGUILayout.GradientField(gradient, GUILayout.Width(100));

            //工具切换
            toolbar = GUILayout.Toolbar(toolbar, new[] {"Area1", "Area2"});

            GUILayout.EndHorizontal();

            //不会自动换行
            GUILayout.BeginHorizontal("many button h", GUILayout.MaxHeight(20));

            for (int i = 0; i < 20; i++)
            {
                if (GUILayout.Button(i.ToString(), GUILayout.Width(60 + i)))
                {
                    Debug.Log("button " + i);
                }
            }

            GUILayout.EndHorizontal();

            //设置最大400宽还是会超出，不会换行
            GUILayout.BeginHorizontal("many button h", GUILayout.MaxWidth(400), GUILayout.MaxHeight(40));

            for (int i = 0; i < 20; i++)
            {
                if (GUILayout.Button(i.ToString(), GUILayout.Width(60 + i)))
                {
                    Debug.Log("button " + i);
                }
            }

            GUILayout.EndHorizontal();

            if (toolbar == 0)
            {
                //设置一个区域，从这个区域开始创建UI
                //这个方法执行后鼠标的坐标会有一个相对的变换，如果监听鼠标事件的话，鼠标位置变成相对指定的起始点的位置
                GUILayout.BeginArea(new Rect(0, 100, 200, 200));

                area1Blue = ColoredRectToggle(new Rect(0, 0, 100, 100), area1Blue, "blue", Color.blue, Color.red);

                if (Event.current.type == EventType.MouseDown)
                {
                    Debug.Log("box a" + Event.current.mousePosition);
                }

                GUILayout.EndArea();
            }
            else
            {
                GUILayout.BeginArea(new Rect(210, 100, 200, 200));

                area2Red = ColoredRectToggle(new Rect(0, 0, 100, 100), area2Red, "red", Color.red, Color.blue);

                if (Event.current.type == EventType.MouseDown)
                {
                    Debug.Log("box b" + Event.current.mousePosition);
                }

                GUILayout.EndArea();
            }

            GUI.Box(new Rect(0, 300, 100, 100), "hello box 1");

            GUILayout.BeginArea(new Rect(110, 300, 100, 100));
            GUILayout.Box("hello box2", GUILayout.Height(100));
            GUILayout.EndArea();

            //按钮列表
            var btnListRect = new Rect(400, 100, 200, 200);
            GUILayout.BeginArea(btnListRect);
            EditorGUI.DrawRect(new Rect(0, 0, btnListRect.width, btnListRect.height), new Color(.5f, 1.0f, 1.0f, 1));

            btnListScrollPos =
                GUILayout.BeginScrollView(btnListScrollPos, false, true, GUILayout.Height(btnListRect.height));

            GUILayout.BeginVertical("many button");

            for (int i = 0; i < 10; i++)
            {
                if (GUILayout.Button(i.ToString(), GUILayout.Height(20 + i)))
                {
                    Debug.Log("button " + i);
                }
            }

            GUILayout.EndVertical();

            GUILayout.EndScrollView();

            GUILayout.EndArea();
        }

        /// <summary>
        /// 自定义一个控件
        /// </summary>
        /// <param name="layout"></param>
        /// <param name="isCheck"></param>
        /// <param name="controlName"></param>
        /// <param name="a"></param>
        /// <param name="b"></param>
        /// <returns></returns>
        public bool ColoredRectToggle(Rect layout, bool isCheck, string controlName, Color a, Color b)
        {
            EditorGUI.DrawRect(layout, isCheck ? a : b);
            return GUILayout.Toggle(area2Red, controlName);
        }
    }
}
```
