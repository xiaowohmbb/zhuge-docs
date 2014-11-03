# Android SDK 开发指南

### 注册应用，获取Appkey
集成SDK之前，您首先需要到诸葛官网注册并且添加新应用，添加成功后，会自动获得AppKey。

### 导入SDK
从SDK下载页面下载 android版的SDK，并导入到项目中。

### 配置 AndroidManifest.xml 文件
需要添加下面两类配置：  

（1）AppKey和渠道  

|meta-data            |必选| 用途        | 
|---------------------|---|-------------|
|ZHUGE_APPKEY |是|用来定位该应用的唯一性|
|ZHUGE_CHANNEL     |是|用来标注应用推广渠道，区分用户来源|


app_key 和 channel 也可以在代码初始化时配置。

（2）需要的权限  

|权限                  |必选| 用途        | 
|---------------------|---|-------------|
|ACCESS_NETWORK_STATE |是|检测联网方式，统计2G/3G/4G/WIFI等|
|READ_PHONE_STATE     |是|获取手机的IMEI|
|ACCESS_WIFI_STATE    |是|获取WIFI的mac地址|
|INTERNET       |是|允许应用程序联网，上报统计数据|
|WRITE_EXTERNAL_STORAGE|是|获取SD卡信息|
|GET_ACCOUNTS     |是|获取用户账户信息，用于匹配互联网数据，完善用户标签|
|GET_TASKS        |是|获取用户应用列表，用于统计用户标签|

示例代码：

    <manifest……>
    	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    	<uses-permission android:name="android.permission.INTERNET" />
    	<uses-permission android:name="android.permission.READ_PHONE_STATE" />
    	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	    <uses-permission android:name="android.permission.GET_ACCOUNTS" />
    	<uses-permission android:name="android.permission.GET_TASKS" />
    	<application ……>
            ……
            <activity ……/>
    	</application>
    	<meta-data android:value="{{APP_KEY}}" android:name="ZHUGE_APPKEY" />
		<meta-data android:value="{{CHANNEL_ID}}" android:name="ZHUGE_CHANNEL"/>
    </manifest>
    
  	
### 初始化

在程序启动时的Activity中调用init接口。

    public class YourApp extends Application{
    	public void onCreate() {
       		super.onCreate();
        	ZhugeSDK.init(this);
    	}
    }
  

### 会话统计
只有正确集成以下代码，才能统计新增用户、活跃用户、留存用户、启动次数、使用时长等基础数据。

在每个 Activity 的 onResume 和 onPause 方法中调用相应的统计方法。

    public void onResume() {
    	super.onResume();
    	ZhugeSDK.onResume(this);
    }
    public void onPause() {
    	super.onPause();
    	ZhugeSDK.onPause(this);
    }

### 页面统计
1. 只有Activity构成的页面时，在上一步已经统计了页面，不需要再单独统计。
2. 由Activity、Fragment或View的构成的应用  
   首先在初始化后禁用页面自动统计，示例代码 
   
         ZhugeSDK.init(this); 
         ZhugeSDK.config.setPageStatistics(false);

	然后在每个页面中onResume和onPause时调用onPageStart和onPageEnd

		public void onResume() {
		    super.onResume();
		    ZhugeSDK.onPageStart(this,"MainPage");
		}
		public void onPause() {
		    super.onPause();
		    ZhugeSDK.onPageEnd(this,"MainPage"); 
		}


### 用户身份识别

诸葛数据平台允许开发者自定义用户身份信息，记录用户的昵称、性别、邮箱等，
这些信息帮助开发者追踪每一个指标背后的用户，对他们进行多维分析与画像，并针对性的进行营销。

示例代码

	HashMap<String, Object> properties = new HashMap<String, Object>();
	properties.put("name", "gump");
	properties.put("gender", "male");
	properties.put("email", "gump@37degree.com");
	properties.put("company", "37degree");
  ZhugeSDK.identify("1231", properties);

#### 预定义的属性：

为了便于分析和页面显示，我们抽取了一些共同的属性，要统计以下数据时，可按照下面格式填写。 

|属性Key     | 说明        | 
|--------|-------------|
|name    | 名称|
|gender  | 性别(值:男,女)|
|birthday| 生日(格式: yyyy/MM/dd)|
|avatar   | 头像地址|
|email   | 邮箱|
|mobile   | 手机号|
|qq      | QQ账号|
|weixin  | 微信账号|
|weibo   | 微博账号|
|location   | 地域，如北京|

 **长度限制**:Key最长支持25个字符，Value最长支持255个字符，一个汉字按3个字符计算。

### 系统配置

开发者可以通过SDK内置的方法进行自定义设置，如果应用已经上线，可以在官网上进行云端设置。

#### 打开调试模式
  
调试模式下可记录日志信息，便于开发调试。

示例代码

	ZhugeSDK.config.setDebug(true);


#### 修改上传策略	

系统支持以下4种方式上传。  

|Key | 说明        | 
|--------|-------------|
|APP_LAUNCH    | 应用打开时上传（默认推荐）|
|INSTANT  | 实时上传|
|ONLY_WIFI| 只在WiFi下上传|
|PERIOD   | 按一定间隔时间上传|

示例代码

 	ZhugeSDK.config.setUpload_method(ZhugeSDK.config.APP_LAUNCH);

	
#### 设置会话间隔 

用户从打开应用到离开应用为一次完整的回话(session)，
如果两次打开应用间隔小于30秒，系统认为是用户误操作，仍认为是同一次回话，这个间隔可以配置。

示例代码

 	ZhugeSDK.config.setSession_exceed(45);
 	
 	
#### 禁止上传数据 

开发者可禁止部分或全部数据上传，应用发布后也可云端设置。

示例代码

 	// 禁止收集用户账户
 	ZhugeSDK.config.setDisable_accounts(true);
 	// 禁止收集用户应用列表
 	ZhugeSDK.config.setDisable_applist(true);
 	// 禁止收集手机号
 	ZhugeSDK.config.setDisable_phonenum(true);
 	
 	// 禁止上传所有数据
 	ZhugeSDK.config.setDisable_upload(true);

