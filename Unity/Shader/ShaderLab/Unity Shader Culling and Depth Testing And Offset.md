## Culling
- 渲染背面还是正面还是都渲染
- `Cull Back | Front | Off`
  - 不渲染背面
  - 不渲染正面
  - 都渲染

## ZWrite
- 是否写入深度值

## ZTest
- 当前像素深度和目标位置的深度满足什么条件可以被渲染

## Offset
- 多个像素在同一个位置，有相同深度，决定哪个像素被渲染
- 个人解释：ZTest不够用，因为float类型不精确
- 格式`Offset Factor, Units`
- offset = m * Factor + r * Units
- 关于m
  - m is the maximum depth slope of the polygon 
  - 平行于near and far clip panel的m为0
  - The depth slope is the change in z (depth) values divided by the change in either x or y coordinates, as you traverse a polygon
  - slope > 0, 体现z随x或者y变化的速度
- r是平台定义的值，是在坐标系统中可分辨的差异最小的值
- reference
  - http://www.glprogramming.com/red/chapter06.html#name4