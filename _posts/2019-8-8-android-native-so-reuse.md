---
layout:     post
title:      android native so reuse
subtitle:   standard JNI so reuse
date:       2019-8-8
author:     lycium
header-img: 
catalog: true
tags:
    - linux
    - android
---

# 实验成功记录
首先我们要设置好Android Studio让它能够正常的访问它的仓库。
接着我们要在gradle.properties里添加以下属性
```
android.useDeprecateNdk = true
```

接着我们要在build.gradle(Module:app)里的android{.......}里添加以下属性
```
sourceSets{
        main{
            jniLibs.srcDir 'libs'
        }
    }
```
然后将标准的JNI so放入工程目录的libs下面，记得结构文件夹也应一起拷贝进来。
标准的JNI函数的命名规则: 包名_类名_方法名。需要注意的是，如果我们的包名里含有"."->"_"的转换。如果我们包名里含有"_"->"_1"的转换。
接下来就是添加代码，如果我们的so文件名为"libpp.so"则我们要在相应的类中添加以下代码。

```
    static {
        System.loadLibrary("pp");
    }
```

接着直接声明JNI函数的函数名和参数即可。
```
public native int p(int i, int i2);
```
然后你就可以用Toast或者TextView来展示你的结果。
