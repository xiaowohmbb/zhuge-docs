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
如果您需要修改SDK的默认设置，如打开日志打印、设置上报策略等时，一定要在`startWithAppKey`前执行。参考代码：
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
您可以通过调用`identify:properties:`来记录用户身份信息。
```objc
    NSMutableDictionary *user = [NSMutableDictionary dictionary];
    user[@"name"] = @"zhuge";
    user[@"gender"] = @"男";
    user[@"birthday"] = @"2014/11/11";
    user[@"avatar"] = @"http://tp2.sinaimg.cn/2885710157/180/5637236139/1";
    user[@"email"] = @"hello@zhuge.io";
    user[@"mobile"] = @"18901010101";
    user[@"qq"] = @"91919";
    user[@"weixin"] = @"121212";
    user[@"weibo"] = @"122222";
    user[@"location"] = @"北京朝阳区";
    user[@"公司"] = @"37degree";
    [[Zhuge sharedInstance] identify:@"1234" properties:user];
```

## 跟踪事件
您可以通过调用`track:properties:`来跟踪自定义事件。
```objc
    [[Zhuge sharedInstance] track:@"购物" properties: @{@"商家":@"京东"}];
```

## 页面访问
您可以通过在每个页面的`viewWillAppear`方法调用`pageStart:`、`viewWillDisappear`方法调用`pageEnd:`来记录页面访问。
```objc
- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    
    //页面开始
    [[Zhuge sharedInstance] pageStart:@"发现"];
}

- (void)viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    
    //页面结束
    [[Zhuge sharedInstance] pageEnd:@"发现"];
}
```
## SDK设置
默认每天从官网下载一次系统配置，如果想在代码中修改，先关闭从线上更新配置。
在调用`startWithAppKey`方法前，可以修改SDK的默认设置。
 1. 关闭从官网自动下载系统设置，
 ```objc
    // 关闭从线上更新配置
    [zhuge.config setIsOnlineConfigEnabled:NO]; // 默认开启
```
 2. 修改发送策略，默认是启动时发送，方法如下：
```objc
    Zhuge *zhuge = [Zhuge sharedInstance];

    // 设置发送策略
    [zhuge.config setPolicy:SEND_REALTIME]; // 实时发送

    // 开启行为追踪
    [zhuge startWithAppKey:@"0a824f87315749a49c16fcbaea277707"];
```
 2. 打开SDK日志
 ```objc
    // 打开SDK日志打印
    [zhuge.config setIsLogEnabled:YES]; // 默认关闭
```

