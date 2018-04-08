# JS 浏览器检测
## 检测浏览器信息

```javascript
var x = navigator;

document.write("平台："+ x.platform + "<br />");
document.write("OnLine=" + x.onLine + "<br />");
document.write("代码："+ x.appCodeName + "<br />");
document.write("浏览器名称："+ x.appName + "<br />");
document.write("CPUClass=" + x.cpuClass + "<br />");
document.write("UserLanguage=" + x.userLanguage + "<br />");
document.write("Cookies 启用："+ x.cookieEnabled + "<br />");
document.write("浏览器的用户代理报头:"+ x.userAgent + "<br />");
document.write("SystemLanguage=" + x.systemLanguage + "<br />");
document.write("BrowserLanguage=" + x.browserLanguage + "<br />");
document.write("浏览器版本："+ parseFloat(x.appVersion) + "<br />");

// 侦探/探测浏览器
function detectBrowser(){
    var browser=navigator.appName;
    var b_version=navigator.appVersion;
    var version=parseFloat(b_version);
    if ((browser=="Netscape"||browser=="Microsoft Internet Explorer") && (version>=4)){
        alert("您的浏览器够先进了！");
    }else{
        alert("是时候升级您的浏览器了！");
    }
}
```

## JS判断是否是微信浏览器
> 微信内置浏览器中UA有关键字micromessenger

```javascript
//判断是否微信登陆
function isWeiXin() {
    var ua = window.navigator.userAgent.toLowerCase();
    return ua.match(/MicroMessenger/i) ? true : false;
}

if(isWeiXin()){
    console.log(" 是来自微信内置浏览器")
}else{
    console.log("不是来自微信内置浏览器")
}
```

**在iphone下**
```javascript
Mozilla/5.0 (iPhone; CPU iPhone OS 5_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Mobile/9B176 MicroMessenger/4.3.2
```
**在Android下**
```javascript
Mozilla/5.0 (Linux; U; Android 2.3.6; zh-cn; GT-S5660 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1 MicroMessenger/4.5.255
```

新建模拟器，更换UA如图：
> 打开Chrome，F12打开开发人员工具，点击菜单按钮-----More Tools -----Network condition打开Network condition窗口

![新建模拟器](https://github.com/kkcode24/blog/blob/master/2018/images/20180328001.gif)

> 用Chrome的iPhone5模拟测试

![测试](https://github.com/kkcode24/blog/blob/master/2018/images/20180328002.gif)

> 通过测试完全通过，无论是android 还是iphone，ipad 都可以，当然我们除了用js来判断之外，用其它语言来判断就更简单了，比如PHP

```php
function is_weixin(){
    if (strpos($_SERVER['HTTP_USER_AGENT'],'MicroMessenger') !== false) { 
        return true; 
    }else{
        return false;
    }
}
```
