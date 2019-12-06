## visual effect
- 模拟云雾
  - 顏色
  - 半透明
  - 不均匀
- 移动

## theory
- 体积雾 volumetric fog & mist
- 布朗分形运动

## implemention
- screen space
  - cloud texture
- 半透明
  - blend
- 不均匀
  - 相机距离越近雾效越弱
  - 已知一个点，求覆盖该点的雾的世界坐标
- 高低起伏
  - nois world.y < (fog.y + noseVal)
- movement
  - noise texture offset y