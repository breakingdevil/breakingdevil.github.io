---
layout:     post
title:      android debug tricks
subtitle:   android debug tricks
date:       2019-8-2
author:     枸杞蒲蒻
header-img: 
catalog: true
tags:
    - linux
    - android
    
---

# 安卓软件调试技巧
我一般使用网易的沐沐模拟器，但是arm架构的apk还是使用真机来进行调试，妥善安装Android Studio即可。
一般使用一下代码生成证书
```
keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
```
之后再使用以下代码给apk签名
```
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore new.apk publishingdoc
```
沐沐模拟器需要对adb进行一些设置才能正常访问
```
adb connect 127.0.0.1:7555
adb shell
```
一些安卓手机当然要root才能愉快地进行调试，当然我们也要使用以下命令关闭SElinux
```
setenforce 0
```
这是一些常用的tricks
