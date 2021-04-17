### 滚动列表
- 不搞移动端那种下拉加载更多，上拉刷新
- 使用分页和跳页的方式展示列表数据
- 本文提供一个带有翻页功能的自定义滚动列表

### 效果图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416232916318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FrNDcwMDd0aWdlcg==,size_16,color_FFFFFF,t_70#pic_center)


### 构造滚动列表需要用到的API
```
//以下需要成对出现
GUILayout.BeginScrollView
GUILayout.EndScrollView

GUILayout.BeginVertical
GUILayout.EndVertical

GUILayout.BeginHorizontal
GUILayout.EndHorizontal

//Unity编辑器的滚动列表是在滚动视图中间放一个包含内容的垂直或者水平布局组件
```

### 代码
```
using System;
using System.Collections.Generic;
using UnityEditor;
using UnityEngine;

namespace DC.DCIMGUIBox
{
    public struct ListViewUserData
    {
        public Vector2 pos;
        public int pageIndex;
        public int onePageCnt;
        public int newPageIndex;

        public ListViewUserData(Vector2 pos, int pageIndex, int onePageCnt, int newPageIndex)
        {
            this.pos = pos;
            this.pageIndex = pageIndex;
            this.onePageCnt = onePageCnt;
            this.newPageIndex = newPageIndex;
        }
    }

    public class ScrollWindow_03 : EditorWindow
    {
        [MenuItem("DC/IMGUI/ScrollWindow_03")]
        public static void Open()
        {
            var window = GetWindow<ScrollWindow_03>();
            window.minSize = new Vector2(800, 600);
        }

        List<int> dataListA = new List<int>();

        private ListViewUserData listInfo = new ListViewUserData(Vector2.zero, 0, 20, 0);

        void OnGUI()
        {
            if (dataListA.Count == 0)
            {
                for (int i = 0; i < 100; i++)
                {
                    dataListA.Add(i);
                }
            }

            listInfo = ScrollList(listInfo, true, dataListA, Render,
                new GUILayoutOption[] {GUILayout.Width(200), GUILayout.Height(200)},
                new GUILayoutOption[] { });
        }

        public void Render<T>(int index, T data)
        {
            GUILayout.Label(string.Format("id：{0} name：{1}", index, index));
        }

        public ListViewUserData ScrollList<T>(ListViewUserData listInfo, bool vertical, List<T> dataList,
            Action<int, T> renderFunc, GUILayoutOption[] svLp, GUILayoutOption[] containerLp)
        {
            var startIndex = listInfo.pageIndex * listInfo.onePageCnt;
            if (startIndex < 0)
            {
                return listInfo;
            }

            if (dataList.Count == 0)
            {
                GUILayout.Label("dataList is empty! 空列表");
                return listInfo;
            }

            if (startIndex > dataList.Count)
            {
                GUILayout.Label("page index is too big! 超最大页数了");
                return listInfo;
            }

            //页头
            GUILayout.BeginHorizontal();

            EditorGUILayout.LabelField("页号", GUILayout.Width(30));

            listInfo.newPageIndex = Math.Max(0, EditorGUILayout.IntField(listInfo.newPageIndex, GUILayout.Width(30)));

            EditorGUILayout.LabelField("每页显示", GUILayout.Width(50));

            listInfo.onePageCnt = Math.Max(1, EditorGUILayout.IntField(listInfo.onePageCnt, GUILayout.Width(30)));

            if (GUILayout.Button("跳转", GUILayout.Width(40)))
            {
                listInfo.pageIndex = listInfo.newPageIndex;
                if (vertical)
                {
                    listInfo.pos.y = 0;
                }
                else
                {
                    listInfo.pos.x = 0;
                }
            }

            GUILayout.EndHorizontal();

            //列表
            listInfo.pos = GUILayout.BeginScrollView(listInfo.pos, svLp);

            var endIndex = Math.Min(startIndex + listInfo.onePageCnt, dataList.Count);

            if (vertical)
            {
                GUILayout.BeginVertical(containerLp);
            }
            else
            {
                GUILayout.BeginHorizontal(containerLp);
            }

            for (int i = startIndex; i < endIndex; i++)
            {
                renderFunc(i, dataList[i]);
            }

            if (vertical)
            {
                GUILayout.EndVertical();
            }
            else
            {
                GUILayout.EndHorizontal();
            }

            GUILayout.EndScrollView();

            return listInfo;
        }
    }
}
```