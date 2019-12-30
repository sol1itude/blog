---
title: Flutter环境配置
date: 2019-12-30 14:13:17
tags:
---


# 1.从如下地址下载对应的flutter SDK，点击如图下载最新版本sdk。

flutterSDK下载地址：https://flutter.dev/docs/get-started/install/windows

![Image text](1.png)

# 2.将sdk复制到系统盘并解压，位置随意。如下图所示

![Image text](2.png)

# 3.打开解压的flutter文件夹，找到bin，复制如下图地址

![Image text](3.png)

# 4.桌面找到 此电脑-->右击选择属性-->高级系统设置-->环境变量-->找到系统变量-->Path-->编辑-->新建-->粘贴第三步复制的地址-->点击确定保存

![Image text](4.png)

# 5.配置国内镜像。桌面找到 此电脑-->右击选择属性-->高级系统设置-->环境变量-->找到用户变量-->点击新建

```javascript
PUB_HOSTED_URL=https://pub.flutter-io.cn

FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

```

![Image text](5.png)

# 6.配置Android sdk，找到用户变量，点击新建，如下图所示。
Android sdk在Android studio中的位置
![Image text](6.1.png)
```javascript
ANDROID_HOME=D:\AndroidSDK //该sdk地址是你安装Android studio的时候配置的sdk的地址
```
![Image text](6.png)

# 7.安装Android studio中的Flutter和Dart插件。

![Image text](7.png)

# 8.重启电脑。

# 9.打开命令行工具输入flutter doctor，如下图所示表明环境配置成功，接下来就能进行flutter的开发了。

![Image text](9.png)

出现感叹号的原因是我没有连接设备，所以提示No devices available



