### 绘制线条
- 使用Handles类绘制各类线条

### 效果图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417102053481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrNDcwMDd0aWdlcg==,size_16,color_FFFFFF,t_70#pic_center)


### 实例代码
```
using UnityEditor;
using UnityEngine;

namespace DC.DCIMGUIBox
{
    public class DrawLineWindow : EditorWindow
    {
        [MenuItem("DC/IMGUI/DrawLineWindow")]
        public static void Open()
        {
            var window = GetWindow<DrawLineWindow>();
            window.minSize = new Vector2(800, 600);
        }

        public Vector3 startPos = new Vector3();
        public Vector3 endPos = new Vector3(500, 100);
        public Vector3 startTangent = new Vector3(20, 0);
        public Vector3 endTangent;

        public Vector3 startPos2 = new Vector3();
        public Vector3 endPos2 = new Vector3(500, 100);
        public Vector3 startTangent2 = new Vector3(20, 0);
        public Vector3 endTangent2;

        public bool lineA = true;

        public bool lineB = true;

        public void OnGUI()
        {
            GUILayout.Label("red line");

            startPos = CustomControls.Float3Field("start",startPos);
            endPos = CustomControls.Float3Field("end", endPos);
            startTangent = CustomControls.Float3Field("startTangent", startTangent);
            endTangent = CustomControls.Float3Field("endTangent", endTangent);

            GUILayout.Space(20);

            GUILayout.Label("green line");

            startPos2 = CustomControls.Float3Field(startPos2);
            endPos2 = CustomControls.Float3Field(endPos2);
            startTangent2 = CustomControls.Float3Field(startTangent2);
            endTangent2 = CustomControls.Float3Field(endTangent2);

            GUILayout.Space(20);
            GUILayout.BeginHorizontal();
            lineA = GUILayout.Toggle(lineA, "draw red", GUILayout.Width(100));
            lineB = GUILayout.Toggle(lineB, "draw green", GUILayout.Width(100));
            GUILayout.EndHorizontal();

            if(lineA)
                Handles.DrawBezier(startPos, endPos, startTangent, endTangent, Color.red, CustomControls.texture2d, 1);

            if(lineB)
                Handles.DrawBezier(startPos2, endPos2, startTangent2, endTangent2, Color.green, CustomControls.texture2d, 1);
        }
    }
}
```

### 库文件
```
using UnityEditor;
using UnityEngine;

namespace DC.DCIMGUIBox
{
    public class CustomControls
    {
        public static Texture2D texture2d = new Texture2D(1,1);

        public static Vector3 Float3Field(string ctrlName, Vector3 input, params GUILayoutOption[] options)
        {
            EditorGUILayout.BeginHorizontal();
            GUILayout.Label(ctrlName);
            input.x = EditorGUILayout.FloatField(input.x, options);
            input.y = EditorGUILayout.FloatField(input.y, options);
            input.z = EditorGUILayout.FloatField(input.z, options);
            EditorGUILayout.EndHorizontal();
            return input;
        }

        public static Vector3 Float3Field(Vector3 input, params GUILayoutOption[] options)
        {
            EditorGUILayout.BeginHorizontal();
            input.x = EditorGUILayout.FloatField(input.x, options);
            input.y = EditorGUILayout.FloatField(input.y, options);
            input.z = EditorGUILayout.FloatField(input.z, options);
            EditorGUILayout.EndHorizontal();
            return input;
        }

        public static Vector4 Float4Field(Vector4 input, string ctrlName, params GUILayoutOption[] options)
        {
            EditorGUILayout.BeginHorizontal();
            GUILayout.Label(ctrlName);
            input.x = EditorGUILayout.FloatField(input.x, options);
            input.y = EditorGUILayout.FloatField(input.y, options);
            input.z = EditorGUILayout.FloatField(input.z, options);
            input.w = EditorGUILayout.FloatField(input.w, options);
            EditorGUILayout.EndHorizontal();
            return input;
        }

        public static Vector4 Float4Field(string ctrlName, Vector4 input, params GUILayoutOption[] options)
        {
            EditorGUILayout.BeginHorizontal();
            GUILayout.Label(ctrlName);
            input.x = EditorGUILayout.FloatField(input.x, options);
            input.y = EditorGUILayout.FloatField(input.y, options);
            input.z = EditorGUILayout.FloatField(input.z, options);
            input.w = EditorGUILayout.FloatField(input.w, options);
            EditorGUILayout.EndHorizontal();
            return input;
        }
    }
}
```