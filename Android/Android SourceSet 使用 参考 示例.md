# Android SourceSet 使用 参考 示例

# 样例1 包含多个目录
```
// 指定jniLibs目录、Lib目录
jniLibs.srcDirs = ['../libs']
// 指定源码目录，这里指定了多个
res.srcDirs = ['src/main/res','../full/src/main/res']
// 指定assets目录，这里指定了多个
assets.srcDirs = ['src/main/assets','../assets_share','../assets_full']
```

# 样例2
```
// 指定manifest文件路径
manifest.srcFile '../google_full/src/main/AndroidManifest.xml'
```

# debug和release区分
```
sourceSets {
    main {
        if (isDebug.toBoolean()) {
            manifest.srcFile 'src/main/debug/AndroidManifest.xml'
        } else {
            manifest.srcFile 'src/main/release/AndroidManifest.xml'
        }
    }
}
```

# 打包时剔除、屏蔽文件
```
sourceSets {
    main {
        java {
            exclude '/dev/**'
        }
        resources {
            exclude '/resource/dev/**'
        }
        .....
    }
}
```

# 更多参考
<https://developer.android.com/studio/build/build-variants?hl=zh-CN>