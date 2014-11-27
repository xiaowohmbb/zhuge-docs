# 诸葛IOS SDK开发指南

## 安装SDK
最简单的安装方式是使用[CocoaPods](http://cocoapods.org/)
 1. 安装CocoaPod `gem install cocoapods`
 2. 项目目录下创建`Podfile`文件，并加入一行代码: `pod 'Zhuge'`
 3. 项目目录下执行`pod install`，CocoaPods会自动安装Zhuge SDK，并生成工作区文件*.xcworkspace，打开该工作区即可。

你也可以把`Zhuge`目录下的文件直接copy到项目下来安装。

## 兼容性和ARC
 1. 诸葛SDK仅支持iOS 6.0以上系统，您需要使用Xcode 5和IOS 7.0以上的SDK进行编译，如果您的版本较低，强烈建议升级。
 2. 诸葛SDK默认采用ARC，如果您的项目没有采用ARC，您需要在编译(`Build Phases -> Compile Sources`)时，为每个Zhuge文件标识为`-fobj-arc`。

## 初始化
在集成诸葛SDk时，您首先需要用AppKey启动。Appkey是在官网上创建项目时生成。
```objc
#import <Zhuge/Zhuge.h>

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [[Zhuge sharedInstance] startWithAppKey:@"Your App Key"];
}
```
如果您需要修改SDK的默认设置，如打开日志打印、设置上报策略等时，一定要在startWithAppKey前执行。参考代码：
```objc
    Zhuge *zhuge = [Zhuge sharedInstance];

    // 设置上报策略
    [zhuge.config setPolicy:SEND_REALTIME]; // 实时发送

    // 打开SDK日志打印
    [zhuge.config setIsLogEnabled:YES]; // 默认关闭

    // 开启行为追踪
    [zhuge startWithAppKey:@"0a824f87315749a49c16fcbaea277707"];
```

## 识别用户身份
## 跟踪事件
## 跟踪页面
## SDK设置
