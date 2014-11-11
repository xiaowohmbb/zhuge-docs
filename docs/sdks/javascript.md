# Javascript SDK 开发指南

## 注册应用，获取Appkey
集成SDK之前，您首先需要到诸葛官网注册并且添加新应用，添加成功后，会自动获得AppKey。

## 安装Javascript库
在每个页面的<head> 和 </head>标签纸巾，嵌入下面代码。
**注意把AppKey替换为你自己项目的AppKey。**

    <script type="text/javascript">
    window.zhuge=window.zhuge||[];window.zhuge.methods="_init debug identify track trackLink trackForm page".split(" ");window.zhuge.factory=function(b){return function(){var a=Array.prototype.slice.call(arguments);a.unshift(b);window.zhuge.push(a);return window.zhuge}};for(var i=0;i<window.zhuge.methods.length;i++){var key=window.zhuge.methods[i];window.zhuge[key]=window.zhuge.factory(key)};window.zhuge.load=function(b){if(!document.getElementById("zhuge-js")){var a=document.createElement("script");a.type="text/javascript";a.id="zhuge-js";a.async=!0;a.src="http://103.17.40.173/zhuge.min.js";var c=document.getElementsByTagName("script")[0];c.parentNode.insertBefore(a,c);window.zhuge._init(b)}};
    window.zhuge.load('{{AppKey}}');
    window.zhuge.page();
    </script>

上面代码会异步下载zhuge.min.js库，并创建一个全局变量zhuge。

## 识别用户
identify方法用来识别用户信息。

参数：  
    ***userId*** 用户的唯一ID  
    ***properties*** 用户的属性

示例代码：

    zhuge.identify('122122', {
      email: 'info@zhuge.io',
      '行业': '互联网'
    });

## 访问页面
page方法用来记录页面访问。

参数：  
    ***name*** 页面名称  

示例代码： 

    // 在head的嵌入js时已经调用，注意不要重复
    // 如果没有页面名称，会用URL地址作为名称
    window.zhuge.page('商品详细页');

## 跟踪事件
track方法用来跟踪自定义事件。

参数：  
    ***event*** 事件名称  
    ***properties*** 事件的属性

示例代码：

    zhuge.track('购买手机', {
      '手机': '小米4',
      '价格': 1799,
      '运营商': '移动'
    });


