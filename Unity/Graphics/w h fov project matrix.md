```
// h = 2 * tan(0.5 * fovVert), assuming screen is 1 unit in front of camera (in world space)
 // fovVert = 2 * atan(1 / unity_CameraProjection._m11)
 // => h = 2 / unity_CameraProjection._m11
 float h = 2.0f / unity_CameraProjection._m11;
 float w = h * scrW / scrH;
```