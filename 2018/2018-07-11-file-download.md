# 文件下载
## 迅雷下载

> 1. 首先引入base64.js和webThunderDetect.js

```
<script src="http://pstatic.xunlei.com/js/webThunderDetect.js"></script>
<script src="http://pstatic.xunlei.com/js/base64.js"></script>
<script>
/**
 * 获取项目根路径
 * @return
 */
function getRootPath () {
    //获取当前网址
    var curWwwPath=window.document.location.href;
    //获取主机地址之后的目录
    var pathName=window.document.location.pathname;
    var pos=curWwwPath.indexOf(pathName);
    //获取主机地址
    var localhostPaht=curWwwPath.substring(0,pos);
    //获取带"/"的项目名
    var projectName=pathName.substring(0,pathName.substr(1).indexOf('/')+1);
    return(localhostPaht+projectName);
}
var thunder_url = getRootPath() + "/index.php"; //文件下载地址
var thunder_pid = "";
var restitle = "";
document.write('<a class="download-link" href="javascript:void(0);" 
thunderHref="' + ThunderEncode(thunder_url) + '" 
thunderPid="' + thunder_pid + '" 
thunderResTitle="' + restitle + '" 
onClick="return OnDownloadClick_Simple(this,2,4)" 
oncontextmenu="ThunderNetwork_SetHref(this)" title="">迅雷下载</a> '); 
</script>
```
> 2.小结：整体实现不复杂，主要就是依赖2个JS脚本，利用其内提供的方法，将我们的 url 进行编码，转为迅雷专用的下载链接，
> 就是这种格式：`thunder://QUFodHRwOi8vbG9jYWxob3N0L0RlbW8vaW5kZXguanNwWlo=`

## 启动本地浏览器下载

最简单的还是使用HTML5的a标签的新增a标签的 `download` 属性

使用HTML5的download属性，可以简便轻松的调用浏览器下载，但是就是浏览器的兼容优点问题。目前 chrome、firefox、opera和360浏览器都支持

```
<!-- 本地下载，使用 download 属性 -->
<a href="index.jsp" download="index.jsp">本地下载</a>
```

只需要简单的给出一个超链接就行, href 指向资源地址，download 是指明下载时浏览器下载弹出的文件名。

## JS实现本地下载1

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>JS实现文件上传下载</title>
</head>
<body>
<a href="javascript:void(0);" id="oDownLoad" onclick="oDownLoad('1.pdf','oDownLoad')">下载</a>
</body>
<script>
	//注：在html同一目录下准备一个1.pdf文件。
    function myBrowser(){
        var userAgent = navigator.userAgent; //取得浏览器的userAgent字符串

        if (userAgent.indexOf("Opera") > -1) {
            return "Opera"
        }
        if (userAgent.indexOf("Firefox") > -1) {
            return "FF";
        }
        if (userAgent.indexOf("Chrome") > -1){
            return "Chrome";
        }
        if (userAgent.indexOf("Safari") > -1) {
            return "Safari";
        }
        if (userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE") > -1 && !isOpera) {
            return "IE";
        };
        if (userAgent.indexOf("Trident") > -1) {
            return "Edge";
        }
    }
 
    function oDownLoad(url,id) {
        if (myBrowser()==="IE" || myBrowser()==="Edge"){
            var oPop = window.open(url,"","width=1, height=1, top=5000, left=5000");
            for(; oPop.document.readyState != "complete"; )
            {
                if (oPop.document.readyState == "complete")break;
            }
            oPop.document.execCommand("SaveAs");
            oPop.close();
        }else{
            //!IE
            var odownLoad=document.getElementById(id);
            odownLoad.href=url;
            odownLoad.download="";
        }
    }
</script>
</html>
```

## js下载文件到本地2

[https://github.com/PixelsCommander/Download-File-JS](https://github.com/PixelsCommander/Download-File-JS "项目地址2017-04-13")

```javascript
window.downloadFile = function (sUrl) {

    //iOS devices do not support downloading. We have to inform user about this.
    if (/(iP)/g.test(navigator.userAgent)) {
        alert('Your device does not support files downloading. Please try again in desktop browser.');
        return false;
    }

    //If in Chrome or Safari - download via virtual link click
    if (window.downloadFile.isChrome || window.downloadFile.isSafari) {
        //Creating new link node.
        var link = document.createElement('a');
        link.href = sUrl;

        if (link.download !== undefined) {
            //Set HTML5 download attribute. This will prevent file from opening if supported.
            var fileName = sUrl.substring(sUrl.lastIndexOf('/') + 1, sUrl.length);
            link.download = fileName;
        }

        //Dispatching click event.
        if (document.createEvent) {
            var e = document.createEvent('MouseEvents');
            e.initEvent('click', true, true);
            link.dispatchEvent(e);
            return true;
        }
    }

    // Force file download (whether supported by server).
    if (sUrl.indexOf('?') === -1) {
        sUrl += '?download';
    }

    window.open(sUrl, '_self');
    return true;
}

window.downloadFile.isChrome = navigator.userAgent.toLowerCase().indexOf('chrome') > -1;
window.downloadFile.isSafari = navigator.userAgent.toLowerCase().indexOf('safari') > -1;
```

## js下载文件到本地3

```
// 成熟的解决方案
http://danml.com/download.html
https://github.com/rndme/download
```

## js下载文件到本地4

https://lzw.me/a/fed-file-download.html