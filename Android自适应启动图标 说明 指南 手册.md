# Android自适应启动图标 说明 指南 手册

## 自适应图标出现的目的
- google为了增加应用启动图标的动画效果

## 自适应图标资源的构成
- foreground 应用的图标，由开发者提供
- background 放在应用图标下面的图片，由开发者提供
- mask 用来创建动画效果的遮罩图，由厂商提供

## 让应用具有自适应图标的方法
- 在res/mipmap-anydpi-v26提供
  - ic_launcher.xml
  - ic_launcher_round.xml
- 配置manifest/application
  ```
  <application
      …
      android:icon="@mipmap/ic_launcher"
      android:roundIcon="@mipmap/ic_launcher_round"
      …>
  </application>
  ```
- 备注
  - ic_launcher.xml例子
  ```
  <?xml version="1.0" encoding="utf-8"?>
  <adaptive-icon xmlns:android="http://schemas.android.com/apk/res/android">
      <background android:drawable="@drawable/ic_launcher_background" />
      <foreground android:drawable="@drawable/ic_launcher_foreground" />
  </adaptive-icon>
  ```
  - ic_launcher_round.xml同样定义了background, foreground
  - 资源名字可以自定义，和application里面配置的一致即可

## 让人困惑的地方 
- 这要从android aip 25的适配说起
  - api 25之前，开发者只需要提供icon
  - 从api 25开始，google要求开发者提供
    - icon
    - roundIcon
- 自适配图标是不要求一定要提供roundIcon的
- 自适配图标只要求开发者提供
  - ic_launcher.xml，这个xml文件定义了
    - foreground
    - background

## 创建自适应图标资源的便捷工具
- 使用 Image Asset Studio 创建应用图标
- 这个工具可以根据一张icon创建视频多种分辨率的
  - ic_launcher.png
  - ic_launcher_round.png
  - res/mipmap-anydpi-v26下的自适配的
    - ic_launcher.xml
    - ic_launcher_round.xml
- 使用手册 https://developer.android.com/studio/write/image-asset-studio.html#create-adaptive

## 小结
- adaptive-icon 给应用启动图标增加动画效果，其资源放在res/mipmap-anydpi-v26文件夹下
- 常规的 xxx.png 图标提供静态启动图标，无法提供动画效果
- application的iconRound配置用来适配 android api 25以上的机型
- 如果想适配 >= android api 25的机型需要
  - 提供
    - icon
    - roundIcon
- 如果想给启动图标加动画效果，在res/mipmap-anydpi-v26下提供
  - icon的adaptive-icon资源
  - roundIcon的adaptive-icon资源
- 如果想让提供的资源起效果，配置manifest文件里面的application标签

## 参考
- https://developer.android.com/guide/practices/ui_guidelines/icon_design_adaptive
- https://dev.to/_s_farias/how-to-create-adaptive-icons-for-android-using-android-studio-459h

## 图例
