## 问题
- 一个列表里面有30个数据，现在想第n填打开界面的时候滚动到第n个

## 计算pos
- horizontalNormalizedPosition
- 每个item所在的pos = (itemIndex * itemWidth) / (itemCount * ItemWidth - scrollRectWidth)
- 关键在于减去scrollRect可视区域的大小