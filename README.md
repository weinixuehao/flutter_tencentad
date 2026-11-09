# 腾讯优量汇(广点通)广告 Flutter版本（支持bidding）

## 本 fork 维护说明

基于上游 `flutter_tencentad` 1.3.3。iOS 原生信息流容器启用了 `clipsToBounds`，
防止原生广告视图绘制到分配区域之外；同时移除了广告回调和事件流中的调试
`print` 输出。Android 仓库配置使用 `mavenCentral()` 替代 Gradle 9 已移除的
`jcenter()`。后续合并上游更新时，请保留这些改动并验证 iOS 与 Android 构建。

<p>
<a href="https://pub.flutter-io.cn/packages/flutter_tencentad"><img src=https://img.shields.io/pub/v/flutter_tencentad?color=orange></a>
<a href="https://pub.flutter-io.cn/packages/flutter_tencentad"><img src=https://img.shields.io/pub/likes/flutter_tencentad></a>
<a href="https://pub.flutter-io.cn/packages/flutter_tencentad"><img src=https://img.shields.io/pub/points/flutter_tencentad></a>
<a href="https://github.com/gstory0404/flutter_tencentad/commits"><img src=https://img.shields.io/github/last-commit/gstory0404/flutter_tencentad></a>
<a href="https://github.com/gstory0404/flutter_tencentad"><img src=https://img.shields.io/github/stars/gstory0404/flutter_tencentad></a>
</p>
<p>
<a href="http://qm.qq.com/cgi-bin/qm/qr?_wv=1027&k=VhD0AZSmzvsD3fu7CeQFkzpBQHMHANb1&authKey=W7JGJ0HKklyhP1jyBvbTF2Dkw0cq4UmhVSx2zXVdIm6n48Xrto%2B7%2B1n9jbkAadyF&noverify=0&group_code=649574038"><img src=https://img.shields.io/badge/flutter%E4%BA%A4%E6%B5%81%E7%BE%A4-649574038-blue></a>
<a href="http://qm.qq.com/cgi-bin/qm/qr?_wv=1027&k=9I9lyXewEsEnx0f00EOF_9hEcFmG5Bmg&authKey=AJfQ8%2FhOLcoJ0p5B16EITjFav1IIs3UAerZSUsWZfa0evuklgxibHti51AYlZgI3&noverify=0&group_code=769626410"><img src=https://img.shields.io/badge/flutter%E4%BA%A4%E6%B5%81%E7%BE%A42-769626410-blue></a>
<a href="https://qm.qq.com/q/4MSgZuKimc"><img src=https://img.shields.io/badge/flutter%E5%B9%BF%E5%91%8A%E4%BA%A4%E6%B5%81-662186116-blue></a>
</p>


## 简介
flutter_tencentad是一款集成了腾讯优量汇广告(广点通)Android和iOS SDK的Flutter插件,方便直接调用优量汇(广点通)广告SDK方法快速开发,[体验demo](https://www.pgyer.com/j7YB)，可通过[GTAds](https://github.com/gstory0404/GTAds)实现多个广告平台接入、统一管理。

<img src="https://github.com/gstory0404/flutter_tencentad/blob/master/images/tencentad.gif" width="30%">


## 官方文档
* [Android](https://developers.adnet.qq.com/doc/android/access_doc)
* [IOS](https://developers.adnet.qq.com/doc/ios/guide)

## 版本更新

[更新日志](https://github.com/gstory0404/flutter_tencentad/blob/master/CHANGELOG.md)

## 本地开发环境
```
[✓] Flutter (Channel stable, 3.16.6, on macOS 14.2.1 23C71 darwin-x64, locale zh-Hans-CN)
[✓] Android toolchain - develop for Android devices (Android SDK version 33.0.1)
[✓] Xcode - develop for iOS and macOS (Xcode 15.2)
[✓] Chrome - develop for the web
[✓] Android Studio (version 2022.1)
[✓] Android Studio (version 2023.1)
[✓] IntelliJ IDEA Ultimate Edition (version 2023.3.2)
[✓] VS Code (version 1.85.2)
[✓] Connected device (4 available)
[✓] Network resources
```

## 集成步骤
#### 1、pubspec.yaml
```Dart
flutter_tencentad: ^latest
```
引入
```Dart
import 'package:flutter_tencentad/flutter_tencentad.dart';
```

> bidding模式下 必需要调用对应Controller 回传竞价结果

#### 2、Android
需要在android目录中AndroidManifest.xml配置

⚠️插件1.1.4以后不再默认集成权限，需手动配置
```Java
<manifest ···
    xmlns:tools="http://schemas.android.com/tools"
    ···>
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.REQUEST_INSTALL_PACKAGES" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
  <application
        tools:replace="android:label">
```

#### 3、IOS

根据SDK文档配置。因为使用PlatformView，在Info.plist加入
```
 <key>io.flutter.embedded_views_preview</key>
    <true/>
```

## 使用

#### 1、SDK初始化
```Dart
await FlutterTencentad.register(
    //androidId
    androidId: "1200310001",
    //iosId
    iosId: "1202937154",
    //是否显示日志log
    debug: true,
    //是否显示个性化推荐广告
    personalized: FlutterTencentadPersonalized.show,
    //渠道id
    channelId: FlutterTencentadChannel.other,
    //安卓隐私设置 根绝官方文档填写
    androidPrivacy: {
        //false为关闭粗略地理位置获取，不设置或者设置为true为获取
        "cell_id": false,
        //优量汇SDK将不采集mac地址
        "mac_address": false,
        //false为关闭device_id获取，不设置或者设置为true为获取
        "device_id": false,
        //false为关闭android_id获取，不设置或者设置为true为获取
        "android_id": false,
        //false为关闭移动网络状态下获取IP地址，不设置或者设置为true为获取
        "mipaddr":false,
        //false为关闭WIFI状态下获取IP地址，不设置或者设置为true为获取
        "wipaddr":false
        //禁用taid获取
        "taid": false,
        //禁用oaid获取
        "oaid": false
    },
    convOptimizelnfo: {
        //关闭应用安装监听状态
        "hieib": false
    },
    enableCollectAppInstallStatus: false,//安卓隐私合规 是否开启收集应用安装状态
);
```
#### 2、获取SDK版本
```Dart
await FlutterTencentad.getSDKVersion();
```

#### 3、开屏广告
```Dart
FlutterTencentAdBiddingController _bidding =
new FlutterTencentAdBiddingController();
FlutterTencentad.splashAdView(
    //android广告id
    androidId: widget.androidId,
    //ios广告id
    iosId: widget.iosId,
    ////设置开屏广告从请求到展示所花的最大时长（并不是指广告曝光时长），取值范围为[1500, 5000]ms
    fetchDelay: 3000,
    //下载二次确认弹窗 默认false
    downloadConfirm: true,
    //是否开启竞价 默认不开启
    isBidding: widget.isBidding,
    //竞价结果回传
    bidding: _bidding,
    //广告回调
    callBack: FlutterTencentadSplashCallBack(onShow: () {
          print("开屏广告显示");
        }, onADTick: (time) {
          print("开屏广告倒计时剩余时间 $time");
        }, onClick: () {
          print("开屏广告点击");
        }, onClose: () {
          print("开屏广告关闭");
          Navigator.pop(context);
        }, onExpose: () {
          print("开屏广告曝光");
        }, onFail: (code, message) {
          print("开屏广告失败  $code $message");
          Navigator.pop(context);
        }, onECPM: (ecpmLevel, ecpm) {
          print("开屏广告竞价  ecpmLevel=$ecpmLevel  ecpm=$ecpm");
            //规则 自己根据业务处理
            if (ecpm > 0) {
            //竞胜出价，类型为Integer
            //最大竞败方出价，类型为Integer
            _bidding.biddingResult(
            FlutterTencentBiddingResult().success(ecpm, 0));
            } else {
            //竞胜方出价（单位：分），类型为Integer
            //优量汇广告竞败原因 FlutterTencentAdBiddingLossReason
            //竞胜方渠道ID FlutterTencentAdADNID
            _bidding.biddingResult(FlutterTencentBiddingResult().fail(
            1000,
            FlutterTencentAdBiddingLossReason.LOW_PRICE,
            FlutterTencentAdADNID.othoerADN));
            }
        }),
),
```
#### 4、banner广告
```Dart
FlutterTencentAdBiddingController _bidding =
new FlutterTencentAdBiddingController();
FlutterTencentad.bannerAdView(
    //android广告id
    androidId: "8042711873318113",
    //ios广告id
    iosId: "6062430096832369",
    //广告宽 单位dp
    viewWidth: 500,
    //广告高  单位dp   宽高比应该为6.4:1
    viewHeight: 100,
    //下载二次确认弹窗 默认false
    downloadConfirm: true,
    //是否开启竞价 默认不开启
    isBidding: widget.isBidding,
    //竞价结果回传
    bidding: _bidding,
    // 广告回调
    callBack: FlutterTencentadBannerCallBack(
        onShow: () {
          print("Banner广告显示");
        },
        onFail: (code, message) {
          print("Banner广告错误 $code $message");
        },
        onClose: () {
          print("Banner广告关闭");
        },
        onExpose: () {
          print("Banner广告曝光");
        },
        onClick: () {
          print("Banner广告点击");
        }, onECPM: (ecpmLevel, ecpm) {
            print("Banner广告竞价  ecpmLevel=$ecpmLevel  ecpm=$ecpm");
            //规则 自己根据业务处理
            if (ecpm > 0) {
                //竞胜出价，类型为Integer
                //最大竞败方出价，类型为Integer
                _bidding.biddingResult(
                FlutterTencentBiddingResult().success(ecpm, 0));
            } else {
                //竞胜方出价（单位：分），类型为Integer
                //优量汇广告竞败原因 FlutterTencentAdBiddingLossReason
                //竞胜方渠道ID FlutterTencentAdADNID
                _bidding.biddingResult(FlutterTencentBiddingResult().fail(
                1000,
                FlutterTencentAdBiddingLossReason.LOW_PRICE,
                FlutterTencentAdADNID.othoerADN));
            }
        },
    ),
),
```

#### 5、动态信息流/横幅/视频贴片广告
⚠️ android端信息流广告曝光异常
```dart
FlutterTencentAdBiddingController _bidding =
new FlutterTencentAdBiddingController();
FlutterTencentad.expressAdView(
              //android广告id
              androidId: "4033498034524284",
              //ios广告id
              iosId: "4033278955532222",
              viewWidth: 400,
              viewHeight: 300,
              //下载二次确认弹窗 默认false
              downloadConfirm: true,
              //是否开启竞价模式
              isBidding: true,
              bidding: _bidding,
              //回调事件
              callBack: FlutterTencentadExpressCallBack(
                  onShow: () {
                    print("动态信息流广告显示");
                  },
                  onFail: (code, message) {
                    print("动态信息流广告错误 $code $message");
                  },
                  onClose: () {
                    print("动态信息流广告关闭");
                  },
                  onExpose: () {
                    print("动态信息流广告曝光");
                  },
                  onClick: () {
                    print("动态信息流广告点击");
                  },
                  onECPM: (ecpmLevel, ecpm) {
                    print("动态信息流广告竞价  ecpmLevel=$ecpmLevel  ecpm=$ecpm");
                    //规则 自己根据业务处理
                    if (ecpm > 0) {
                      //竞胜出价，类型为Integer
                      //最大竞败方出价，类型为Integer
                      _bidding.biddingResult(
                          FlutterTencentBiddingResult().success(ecpm, 0));
                    } else {
                      //竞胜方出价（单位：分），类型为Integer
                      //优量汇广告竞败原因 FlutterTencentAdBiddingLossReason
                      //竞胜方渠道ID FlutterTencentAdADNID
                      _bidding.biddingResult(FlutterTencentBiddingResult().fail(
                          1000,
                          FlutterTencentAdBiddingLossReason.LOW_PRICE,
                          FlutterTencentAdADNID.othoerADN));
                    }
                  }
              ),
            ),
```

#### 6、激励视频广告
预加载激励视频广告
```Dart
await FlutterTencentad.loadRewardVideoAd(
    //android广告id
    androidId: "5042816813706194",
    //ios广告id
    iosId: "8062535056034159",
    //用户id
    userID: "123",
    //奖励
    rewardName: "100金币",
    //奖励数
    rewardAmount: 100,
    //扩展参数 服务器回调使用
    customData: ""
    //下载二次确认弹窗 默认false
    downloadConfirm: true,
    //是否开启竞价
    isBidding: true,
);
```
显示激励视频广告
```dart
  await FlutterTencentad.showRewardVideoAd();
```
监听激励视频结果
> 当为竞价时触发onECPM，否则触发onReady
```Dart
 FlutterTencentAdStream.initAdStream(
      //激励广告
    flutterTencentadRewardCallBack: FlutterTencentadRewardCallBack(
        onShow: () {
          print("激励广告显示");
        },
        onClick: () {
          print("激励广告点击");
        },
        onFail: (code, message) {
          print("激励广告失败 $code $message");
        },
        onClose: () {
          print("激励广告关闭");
        },
        onReady: () async {
          print("激励广告预加载准备就绪");
          await FlutterTencentad.showRewardVideoAd();
        },
        onUnReady: () {
          print("激励广告预加载未准备就绪");
        },
         onVerify: (transId,rewardName,rewardAmount) {
          print("激励广告奖励  $transId   $rewardName   $rewardAmount");
        },
        onFinish: (){
          print("激励广告完成");
        },
        onECPM: (ecpmLevel, ecpm) async {
            print("激励广告竞价  ecpmLevel=$ecpmLevel  ecpm=$ecpm");
            //规则 自己根据业务处理
            if (ecpm > 0) {
                //竞胜出价，类型为Integer
                //最大竞败方出价，类型为Integer
                await FlutterTencentad.showRewardVideoAd(
                result: FlutterTencentBiddingResult().success(ecpm, 0));
            } else {
                //竞胜方出价（单位：分），类型为Integer
                //优量汇广告竞败原因 FlutterTencentAdBiddingLossReason
                //竞胜方渠道ID FlutterTencentAdADNID
                await FlutterTencentad.showRewardVideoAd(
                result: FlutterTencentBiddingResult().fail(
                1000,
                FlutterTencentAdBiddingLossReason.LOW_PRICE,
                FlutterTencentAdADNID.othoerADN));
          }
        },
      ),
    );
```
#### 7、插屏广告
预加载插屏广告
```dart
await FlutterTencentad.loadUnifiedInterstitialAD(
    //android广告id
    androidId: "9062813863614416",
    //ios广告id
    iosId: "1052938046031440",
    //是否全屏
    isFullScreen: false,
    //下载二次确认弹窗 默认false
    downloadConfirm: true,
    //是否开启竞价
    isBidding: true,
);
```

显示插屏广告
```dart
  await FlutterTencentad.showUnifiedInterstitialAD();
```

插屏广告结果监听
> 当为竞价时触发onECPM，否则触发onReady
```dart
FlutterTencentAdStream.initAdStream(
    flutterTencentadInteractionCallBack: FlutterTencentadInteractionCallBack(
        onShow: () {
          print("插屏广告显示");
        },
        onClick: () {
          print("插屏广告点击");
        },
        onFail: (code, message) {
          print("插屏广告失败 $code $message");
        },
        onClose: () {
          print("插屏广告关闭");
        },
        onReady: () async {
          print("插屏广告预加载准备就绪");
          await FlutterTencentad.showUnifiedInterstitialAD();
        },
        onUnReady: () {
          print("插屏广告预加载未准备就绪");
        },
        onVerify: (transId,rewardName,rewardAmount){
          print("广告奖励凭证id  $transId");
        },
        onECPM: (ecpmLevel, ecpm) async {
            print("插屏广告竞价  ecpmLevel=$ecpmLevel  ecpm=$ecpm");
            //规则 自己根据业务处理
            if (ecpm > 0) {
                //竞胜出价，类型为Integer
                //最大竞败方出价，类型为Integer
                await FlutterTencentad.showUnifiedInterstitialAD(
                result: FlutterTencentBiddingResult().success(ecpm, 0));
            } else {
                //竞胜方出价（单位：分），类型为Integer
                //优量汇广告竞败原因 FlutterTencentAdBiddingLossReason
                //竞胜方渠道ID FlutterTencentAdADNID
                await FlutterTencentad.showUnifiedInterstitialAD(
                result: FlutterTencentBiddingResult().fail(
                1000,
                FlutterTencentAdBiddingLossReason.LOW_PRICE,
                FlutterTencentAdADNID.othoerADN));
            }
      },
  ),
);
```

#### 8、进入APP下载列表（仅android）
```dart
await FlutterTencentad.enterAPPDownloadListPage();
```

## 错误码

- [错误码1](https://developers.adnet.qq.com/doc/android/union/union_debug#sdk%20%E9%94%99%E8%AF%AF%E7%A0%81)
- [错误码2](https://developers.adnet.qq.com/backend/error_code.html)

## 插件链接

|插件|地址|
|:----|:----|
|字节-穿山甲广告插件|[flutter_unionad](https://github.com/gstory0404/flutter_unionad)|
|腾讯-优量汇广告插件|[flutter_tencentad](https://github.com/gstory0404/flutter_tencentad)|
|百度-百青藤广告插件|[baiduad](https://github.com/gstory0404/baiduad)|
|字节-Gromore聚合广告|[gromore](https://github.com/gstory0404/gromore)|
|Sigmob广告|[sigmobad](https://github.com/gstory0404/sigmobad)|
|聚合广告插件(迁移至GTAds)|[flutter_universalad](https://github.com/gstory0404/flutter_universalad)|
|GTAds聚合广告|[GTAds](https://github.com/gstory0404/GTAds)|
|字节穿山甲内容合作插件|[flutter_pangrowth](https://github.com/gstory0404/flutter_pangrowth)|
|文档预览插件|[file_preview](https://github.com/gstory0404/file_preview)|
|滤镜|[gpu_image](https://github.com/gstory0404/gpu_image)|

### 开源不易，觉得有用的话可以请作者喝杯奶茶🧋
<img src="https://github.com/gstory0404/flutter_tencentad/blob/master/images/weixin.jpg" width = "200" height = "160" alt="打赏"/>

## 联系方式
* Email: gstory0404@gmail.com
* Blog：https://www.gstory.cn/
