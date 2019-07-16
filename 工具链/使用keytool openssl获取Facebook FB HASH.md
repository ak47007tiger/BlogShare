# 使用keytool openssl获取Facebook FB HASH

## 官方命令
```
keytool -exportcert -alias <RELEASE_KEY_ALIAS> -keystore <RELEASE_KEY_PATH> | PATH_TO_OPENSSL_LIBRARY\bin\openssl sha1 -binary | PATH_TO_OPENSSL_LIBRARY\bin\openssl base64
```

## 实例命令
- 指明签名文件在哪里
- 指明openssl命令再哪里
```
keytool -exportcert -alias xxx -keystore xxx.jks | D:\ProgramFiles\openssl-1.0.2q-x64_86-win64\openssl sha1 -binary | D:\ProgramFiles\openssl-1.0.2q-x64_86-win64\openssl base64
```
- 执行后输入证书密码，命令行最后面会打印出HASH串，复制使用

## 获取openssl
- 各种openssl库列表
  - https://wiki.openssl.org/index.php/Binaries
- 本文使用的库的下载地址
  - https://indy.fulgan.com/SSL/
  - https://indy.fulgan.com/SSL/openssl-1.0.2q-x64_86-win64.zip
- 下载之后解压即用
