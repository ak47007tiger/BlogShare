# 借助Unity实现一个超简单的软光栅

## 概述
- 借助Unity的Camera,Matrix4x4,Vector4,Mesh,Texture2D，完成顶点从模型空间到屏幕空间的坐标变换，在屏幕上绘制线框三角形

## 效果图

## 实现
```
using System;
using System.Collections;
using System.Collections.Generic;
using DC;
using UnityEngine;
using UnityEngine.UI;

public class SoftRenderer : MonoBehaviour
{
  public Camera _camera;

  public RawImage _ragImage;

  public Vector2Int _screenSize;

  public Transform _CubeTf;

  public Mesh _Mesh;

  private SoftEngine _softEngine;

  private Device _device;

  void Start()
  {
    _device = new Device();
    _device.Create(_screenSize.x, _screenSize.y);

    _softEngine = new SoftEngine();
    _softEngine.DeviceProp = _device;

    _ragImage.texture = _device.GetResult();
  }

  void Update()
  {
    _device.Clear();

    _softEngine.OnRender(_camera, _Mesh, _CubeTf);
  }

  /*
  local
  world
  view
  projection
  screen
  clip
   */
}

namespace DC
{
  public class SoftEngine
  {
    public Device DeviceProp { get; set; }

    private Color _color = Color.red;

    public Color ColorProp
    {
      get { return _color; }
      set { _color = value; }
    }

    public void OnRender(Camera camera, Mesh mesh, Transform transform)
    {
      var worldMatrix = transform.localToWorldMatrix;
      var viewMatrix = camera.worldToCameraMatrix;
      var projectionMatrix = GL.GetGPUProjectionMatrix(camera.projectionMatrix, false);

      for (var i = 0; i < mesh.triangles.Length - 3; i += 3)
      {
        var pi1 = mesh.triangles[i];
        var pi2 = mesh.triangles[i + 1];
        var pi3 = mesh.triangles[i + 2];

        var lp1 = mesh.vertices[pi1];
        var lp2 = mesh.vertices[pi2];
        var lp3 = mesh.vertices[pi3];

        var mvpMatrix = projectionMatrix * viewMatrix * worldMatrix;

        var lp14 = new Vector4(lp1.x, lp1.y, lp1.z, 1);
        var lp24 = new Vector4(lp2.x, lp2.y, lp2.z, 1);
        var lp34 = new Vector4(lp3.x, lp3.y, lp3.z, 1);

        var mvpp1 = mvpMatrix * lp14;
        var mvpp2 = mvpMatrix * lp24;
        var mvpp3 = mvpMatrix * lp34;

        mvpp1 /= mvpp1.w;
        mvpp2 /= mvpp2.w;
        mvpp3 /= mvpp3.w;

        //todo clip triangle
        var sp1 = ComputeScreenCoordinates(mvpp1);
        var sp2 = ComputeScreenCoordinates(mvpp2);
        var sp3 = ComputeScreenCoordinates(mvpp3);
//                RenderPoint(sp1, ColorProp);
//                RenderPoint(sp2, ColorProp);
//                RenderPoint(sp3, ColorProp);
        RenderTriangle(sp1, sp2, sp3, ColorProp);
      }

//            for (var i = 0; i < mesh.vertexCount; i++)
//            {
//                var mvpMatrix = projectionMatrix * viewMatrix * worldMatrix;
//                var localPoint = mesh.vertices[i];
//                var localPoint4 = new Vector4(localPoint.x, localPoint.y, localPoint.z, 1);
////                var worldPoint = worldMatrix.MultiplyPoint(localPoint);
////                var viewPoint = viewMatrix.MultiplyPoint(worldPoint);
////                var leftViewPoint = rightToLeft.MultiplyPoint(viewPoint);
////                var projectionPoint = projectionMatrix.MultiplyPoint(leftViewPoint);
////                var mvpPoint = mvpMatrix.MultiplyPoint(localPoint);
//                var mvpPoint4 = mvpMatrix * localPoint4;
//                if (NotClip(mvpPoint4))
//                {
//                    var mvpPoint = mvpPoint4 / mvpPoint4.w;
//                    var screenPos = ComputeScreenCoordinates(mvpPoint);
//                    //                var screenPos2 = ComputeScreenCoordinates(projectionPoint);
//                    //                RenderPoint(screenPos, ColorProp);
//                    RenderPoint(screenPos, ColorProp);
//                }
//            }

      DeviceProp.Commit();
    }

    public static bool NotClip(Vector4 mvpPoint4)
    {
      return Mathf.Abs(mvpPoint4.x) < mvpPoint4.w && Mathf.Abs(mvpPoint4.y) < mvpPoint4.w &&
             Mathf.Abs(mvpPoint4.z) < mvpPoint4.w;
    }

    public Vector2Int ComputeScreenCoordinates(Vector3 projectionPos)
    {
      return new Vector2Int((int) (projectionPos.x * DeviceProp.Width * 0.5f + DeviceProp.Width * 0.5f + 0.5f),
        (int) (projectionPos.y * DeviceProp.Height * 0.5f + DeviceProp.Height * 0.5f + 0.5f));
      //            return new Vector2Int(Convert.ToInt32(projectionPos.x * DeviceProp.Width + 0.5f), Convert.ToInt32(projectionPos.y * DeviceProp.Height));
//            return new Vector2Int((int)(projectionPos.x * DeviceProp.Width + 0.5f + DeviceProp.Width * 0.5f), (int)(projectionPos.y * DeviceProp.Height + 0.5f + DeviceProp.Height * 0.5f));
    }

    public void RenderPoint(Vector2Int point, Color color)
    {
      DeviceProp.SetPixel(point.x, point.y, color);
    }

    public void RenderPoint(int x, int y, Color color)
    {
      DeviceProp.SetPixel(x, y, color);
    }

    public void RenderLine(Vector2Int start, Vector2Int end, Color color)
    {
      var x0 = start.x;
      var y0 = start.y;

      var x1 = end.x;
      var y1 = end.y;
      if (x0 == x1 && y0 == y1)
      {
        RenderPoint(x0,y0, color);
        return;
      }

      var x = x0;
      var y = y0;

      var d = f(x0 + 1, y + 0.5f, x0, y0, x1, y1);

      var delta = Convert.ToInt32(Mathf.Sign(x1 - x0));
      if (x0 == x1)
      {
        delta = Convert.ToInt32(Mathf.Sign(y1 - y0));
        while (y != y1)
        {
          y += delta;
          RenderPoint((int)(x + 0.5f), (int)(y + 0.5f), color);
        }
      }
      else if (y0 == y1)
      {
        while (x != x1)
        {
          x += delta;
          RenderPoint((int)(x + 0.5f), (int)(y + 0.5f), color);
        }
      }
      else
      {
        while (x != x1)
        {
          RenderPoint((int)(x + 0.5f), (int)(y + 0.5f), color);
          //                if (d < 0)
          //                {
          //                    y++;
          //                    d = d + (x1 - x0) + (y0 - y1);
          //                }
          //                else
          //                {
          //                    d = d + (y0 - y1);
          //                }
          x = x + delta;
          y = (int)(calculateY(x, x0, y0, x1, y1) + 0.5f);
        }
      }

      RenderPoint(end.x, end.y, color);

//            var d = 2 * (y0 - y1) * (x0 + 1) + (x1 - x0) * (2 * y0 + 1) + 2 * x0 * y1 - 2 * x1 * y0;
//            for (var x = x0; x <= x1; x++)
//            {
//                RenderPoint(x, y, color);
//                if (d < 0)
//                {
//                    y++;
//                    d = d + 2 * (x1 - x0) + 2 * (y0 - y1);
//                }
//                else
//                {
//                    d = d + 2 * (y0 - y1);
//                }
//            }
    }

    public static float calculateY(float x, float x0, float y0, float x1, float y1)
    {
      return (y1 - y0) * (x - x0) / (x1 - x0) + y0;
    }

    public float f(float x, float y, float x0, float y0, float x1, float y1)
    {
      return (y0 - y1) * x + (x1 - x0) * y + x0 * y1 - x1 * y0;
    }

    public void RenderTriangle(Vector2Int p1, Vector2Int p2, Vector2Int p3, Color color)
    {
      RenderLine(p1, p2, color);
      RenderLine(p2, p3, color);
      RenderLine(p3, p1, color);
    }
  }

  public class Device
  {
    private Texture2D _texture2D;

    public int Width { get; private set; }
    public int Height { get; private set; }

    public void Create(int width, int height)
    {
      this.Width = width;
      this.Height = height;
      _texture2D = new Texture2D(width, height, TextureFormat.ARGB32, false, true);
    }

    public void Clear()
    {
      var black = Color.black;
      for (var x = 0; x < Width; x++)
      {
        for (var y = 0; y < Height; y++)
        {
          SetPixel(x, y, black);
        }
      }
    }

    public void Commit()
    {
      _texture2D.Apply();
    }

    public void SetPixel(int x, int y, Color color)
    {
      _texture2D.SetPixel(x, y, color);
    }

    public Color GetPixel(int x, int y)
    {
      return _texture2D.GetPixel(x, y);
    }

    public Texture2D GetResult()
    {
      return _texture2D;
    }
  }
}
```