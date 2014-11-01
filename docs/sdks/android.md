# Android SDK 开发指南


### 注册应用，获取Appkey
集成SDK之前，您首先需要到诸葛官网注册并且添加新应用，获得AppKey。

### 导入SDK
从SDK下载页面下载 android版的SDK，并导入到项目中。


### 配置 AndroidManifest.xml 文件
manifest的配置主要包括添加权限、添加Appkey和添加渠道三部分。  
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
    
 **权限说明**
 
|权限                  |必须| 说明        | 
|---------------------|---|-------------|
|ACCESS_NETWORK_STATE |必须|检测联网方式，统计2G/3G/4G/WIFI等|
|READ_PHONE_STATE 	  |必须|获取手机的IMEI|
|ACCESS_WIFI_STATE	  |必须|获取WIFI的mac地址|
|INTERNET			  |必须|允许应用程序联网，上报统计数据|
|WRITE_EXTERNAL_STORAGE|必须|获取SD卡信息|
|GET_ACCOUNTS		  |必须|获取用户账户信息，用于匹配互联网数据，完善用户标签|
|GET_TASKS   		  |必须|获取用户应用列表，用于统计用户标签|

  ***app_key*** 
  	从诸葛官网上获取
  	 
  ***channel*** 
  	应用推广的渠道，例如wandoujia
  	
  app_key 和 channel 也可以在代码初始化时配置。
  	
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

诸葛数据平台允许开发者自定义用户身份信息，记录用户的昵称、性别、邮箱等，这些信息帮助开发者追踪每一个指标背后的用户，对他们进行多维分析与画像，并针对性的进行营销。

##### 调用代码:

	HashMap<String, Object> properties = new HashMap<String, Object>();
	properties.put("name", "gump");
	properties.put("gender", "male");
	properties.put("email", "gump@37degree.com");
	properties.put("company", "37degree");
    ZhugeSDK.identify("1231", properties);
 
 ***uid*** 用户唯一标识  
 ***properties*** 自定义属性
 
###### 预定义的属性：

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

开发时可以通过SDK内置的方法进行配置，App发布后可通过官网进行设置。

###### SDK配置的参数


***调试模式***  
调试模式下可记录日志信息，便于开发调试，通过下面方法开启

	ZhugeSDK.config.setDebug(true);


***上传策略***	
系统支持以下4种方式上传  
 1. APP_LAUNCH  应用打开时（推荐）  
 2. INSTANT 实时上传  
 3. ONLY_WIFI 只在WiFi下上传  
 4. PERIOD 间隔时间
 
 	ZhugeSDK.config.setUpload_method(ZhugeSDK.config.APP_LAUNCH);

	
***session间隔***  
用户从打开应用到离开应用为一次完整的回话(session)，如果两次打开应用间隔小于30秒，系统认为是用户误操作，仍认为是同一次回话，这个间隔可以配置。

 	ZhugeSDK.config.setSession_exceed(45);
 	
 	
***禁止上传数据***  
	可通过以下方式禁止部分或全部数据上传

 	// 禁止收集用户账户
 	ZhugeSDK.config.setDisable_accounts(true);
 	// 禁止收集用户应用列表
 	ZhugeSDK.config.setDisable_applist(true);
 	// 禁止收集手机号
 	ZhugeSDK.config.setDisable_phonenum(true);
 	
 	// 禁止上传所有数据
 	ZhugeSDK.config.setDisable_upload(true);

***自动页面统计***  
	不希望自动统计页面时，可调用下面方法
	
	ZhugeSDK.config.setPageStatistics(false);











