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
## 识别用户身份
## 跟踪事件
## 跟踪页面
## 系统设置
