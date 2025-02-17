# alipay_kit

[![GitHub Tag](https://img.shields.io/github/tag/rxreader/alipay_kit.svg)](https://github.com/rxreader/alipay_kit/releases)
[![Pub Package](https://img.shields.io/pub/v/alipay_kit.svg)](https://pub.dartlang.org/packages/alipay_kit)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/rxreader/alipay_kit/blob/master/LICENSE)

flutter版支付宝SDK

## fake 系列 libraries

* [flutter版微信SDK](https://github.com/rxreader/wechat_kit)
* [flutter版腾讯(QQ)SDK](https://github.com/rxreader/tencent_kit)
* [flutter版新浪微博SDK](https://github.com/rxreader/weibo_kit)
* [flutter版支付宝SDK](https://github.com/rxreader/alipay_kit)
* [flutter版walle渠道打包工具](https://github.com/rxreader/walle_kit)

## dart/flutter 私服

* [simple_pub_server](https://github.com/rxreader/simple_pub_server)

## docs

* [蚂蚁金服开放平台](https://openhome.alipay.com/platform/appManage.htm)
* [支付宝支付](https://docs.open.alipay.com/204/105051/)
* [支付宝登录](https://docs.open.alipay.com/218/105329/)
* [应用签名工具](https://opendocs.alipay.com/open/common/104062)

## android

```groovy
buildscript {
    dependencies {
        // Android 11兼容，需升级Gradle到3.5.4/3.6.4/4.x.y
        classpath 'com.android.tools.build:gradle:3.5.4'
    }
}
```

```
# 不需要做任何额外接入工作
# 混淆已打入 Library，随 Library 引用，自动添加到 apk 打包混淆
```

#### UTDID冲突的问题解决方案

```shell
java.lang.RuntimeException: Duplicate class com.ta.utdid2.a.a.a found in modules alicloud-android-utdid-2.5.1-proguard.jar
```

```groovy
rootProject.subprojects {
    project.configurations.all {
        resolutionStrategy.eachDependency { details ->
            if (details.requested.group == 'com.aliyun.ams' && details.requested.name == 'alicloud-android-utdid') {
                // 用已存在的包替换掉冲突包
                details.useTarget group: 'androidx.annotation', name: 'annotation', version: '1.1.0'
            }
        }
    }
}
```

#### 获取 android 微信签名信息

非官方方法 -> 反编译 GenSignature_0630.apk 所得

命令：

```shell
keytool -list -v -keystore ${your_keystore_path} -storepass ${your_keystore_password} 2>/dev/null | grep -p 'MD5:.*' -o | sed 's/MD5://' | sed 's/ //g' | sed 's/://g' | awk '{print tolower($0)}'
```

示例：

```shell
keytool -list -v -keystore example/android/app/infos/dev.jks -storepass 123456 2>/dev/null | grep -p 'MD5:.*' -o | sed 's/MD5://' | sed 's/ //g' | sed 's/://g' | awk '{print tolower($0)}'
```

```shell
28424130a4416d519e00946651d53a46
```

## ios

```
在Xcode中，选择你的工程设置项，选中“TARGETS”一栏，在“info”标签栏的“URL type“添加“URL scheme”为你所注册的应用程序id

URL Types
alipay: identifier=alipay schemes=${your app scheme name} # schemes 不能为纯数字，推荐：alipay${appId}
```

```
iOS 9系统策略更新，限制了http协议的访问，此外应用需要在“Info.plist”中将要使用的URL Schemes列为白名单，才可正常检查其他应用是否安装。

<key>LSApplicationQueriesSchemes</key>
<array>
    <string>alipay</string>
</array>
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

## flutter

* break change
    * 3.1.0: android:minSdkVersion="19"
    * 3.0.0: 重构
    * 2.2.0: Alipay 单例
    * 2.1.0: nullsafety & 不再支持 Android embedding v1

* snapshot

```
dependencies:
  alipay_kit:
    git:
      url: https://github.com/rxreader/alipay_kit.git
```

* release

```
dependencies:
  alipay_kit: ^${latestTag}
```

```
dependencies:
  # 请不要加 ^
  # 请不要进行配置 iOS 相关配置，否则 Apple Store 审核时会拒绝
  alipay_kit: ${latestTag}-Android-Only
```

* example

[示例](./example/lib/main.dart)


## Star History

![stars](https://starchart.cc/rxreader/alipay_kit.svg)
