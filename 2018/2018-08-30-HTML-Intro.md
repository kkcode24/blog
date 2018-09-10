
# HTML简介  
  
什么是HTML5？  
> 狭义上，HTML的第五个版本。实际上是HTML+CSS+JS+API的组合  
  
HTML：Hyper Text MarkUp Language  
解释为超文本标记语言，作用是用来做网页。  
  
## HTML的发展史  
1. HTML1.0 Tim Berners-Lee(蒂姆-博纳斯-李)于1993年创建，世称其为万维网之父。  
2. HTML2.0 IETF（互联网工程小组）于1995年创建，该小组已经消失  
3. HTML3.2 W3C（万维网联盟，world wide web）于1997年1月创建  
4. HTML4.0 1997.12 W3C  
5. HTML4.1 2000 W3C  
6. HTML5.0 2004.10 W3C  
  
  
## HTML简介  
  
HTML文档是由多个元素组成。  
  
元素：组成HTML文档的基本单位，元素是由<>和元素名组成。  
  
元素分为两种：  
1. 双标签元素：开始标签+内容+结束标签  
2. 单标签元素：只有开始标签，H5中，单标签不需要闭合。  
  
标签内：元素的开始标签，即尖括号的元素名的后面  
标签里：只有双标签，才有标签里这个称呼。  
  
## HTML的基础标签  
  
### meta  
> 单标签  
> `<meta>`元素可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词  
> `<meta>`标签位于文档的头部，不包含任何内容。`<meta>`标签的属性定义了与文档相关联的名称/值对。  
  
meta标签都有哪些属性？  
  
必须的属性`content`,属性值是`some_text`,其意义是定义与http-equiv或name属性相关的元信息。  
  
  
| 属性 | 值 |描述|  
|--|--|--|  
| http-equiv |content-type，expires，refresh，set-cookie |把 content 属性关联到 HTTP 头部。|  
| name | - author, description, keywords, generator,revised,others | 把content属性关联到一个名称。 |  
| scheme | some_text | 定义用于翻译 content 属性值的格式。 |  
  
  
**http-equiv属性**  
该属性为名称/值对提供了名称。并指示服务器在发送实际的文档之前现在要传送给服务器的 MIME文档头部包含名称/值对  
  
当服务器向浏览器发送文档时，会先发送许多名称/值对. 虽然有些服务器会发送许多这种名称/值对，但是所有服务器都至少要发送一个：content-type:text/html。这将告诉浏览器准备介绍一个HTML文档  
  
使用带有http-equiv属性的<meta>标签时，服务器将把名称/值对添加到发送给服务器的内容头部。  
例如：  
```  
<meta http-equiv="charset" content="iso-8859-1">  
<meta http-equiv="expires" content="31 Dec 2008">  
```  
  
这样发送到浏览器的头部就应该包含：  
```  
content-type: text/html  
charset: iso-8859-1  
expires: 31 Dec 2018  
```  
当然只有浏览器可以接受这些附加的头部字段，并能以适当的方式使用它们时，这些字段才有意义。  
  
**name属性**  
> 该属性提供了名称/值对中的名称。HTML和XHTML标签都没有指定任何预先定义的<meta>名称。通常情况下，您可以自由使用对自己和源文档的读者来说富有意义的名称。  
  
"keywords" 是一个经常被用到的名称。它为文档定义了一组关键字。某些搜索引擎在遇到这些关键字时，会用这些关键字对文档进行分类。  
  
类似这样的meta标签可能对于进入搜索引擎的索引有帮助：  
```  
<meta name="keywords" content="HTML,ASP,PHP,SQL">  
```  
  
如果没有提供name属性，name名称/值对中的名称会采用http-equiv属性中的值。  
  
  
**content属性**  
> 该属性提供了名称/值对中的值。该值可以是任何有效的字符串。  
> content属性始终要和name属性或http-equiv属性一起使用。  
  
**scheme属性**  
> 该属性用于指定要用来翻译属性值的方案。此方案应该在由`<head>`标签的profile属性指定的概况文件中进行了定义。  
  
```  
// 告诉http头部，文档类型为text/html,字符集gb2312  
<meta http-equiv="Content-type" content="text/html" charset="gb2312">  
// 定义页面作者  
<meta name="author" content="qdgithub.com">  
// 修订信息  
<meta name="revised" content="David Yang,8/1/07">  
// 该文件是用什么生成的  
<meta name="generator" content="Dreamweaver 8.0en">  
// 定义web页面描述  
<meta name="description" content="HTML examples">  
// 定义文档关键词，用于搜索引擎  
<meta name="keywords" content="HTML,DHTML,CSS,XML,XHTML,JavaScript,VBScript">  
// 每30秒刷新页面  
<meta http-equiv="refresh" content="30">  
// 该文档已经变更，将用户重定向到新的地址  
<meta http-equiv="Refresh" content="5;url=http://www.qdgithub.com">  
  
<!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->  
<meta name="HandheldFriendly" content="true">  
<!-- 微软的老式浏览器 -->  
<meta name="MobileOptimized" content="320">  
<!-- uc强制竖屏 -->  
<meta name="screen-orientation" content="portrait">  
<!-- QQ强制竖屏 -->  
<meta name="x5-orientation" content="portrait">  
<!-- UC强制全屏 -->  
<meta name="full-screen" content="yes">  
<!-- QQ强制全屏 -->  
<meta name="x5-fullscreen" content="true">  
<!-- UC应用模式 -->  
<meta name="browsermode" content="application">  
<!-- QQ应用模式 -->  
<meta name="x5-page-mode" content="app">  
```  
  
`meta`属性在HTML4.01和HTML5之间的差异  
  
1. HTML5不支持scheme属性  
2. 在HTML5中有一个新的charset属性，它使字符集的定义更加容易：  
- HTML4.01：`<meta http-equiv="content-type" content="text/html;charset=UTF-8">`  
- HTML5: `<meta charset="UTF-8">`  
  
3. 还有一个viewport属性，用于移动端显示优化的。其有六个属性。  



| 属性 | 值描述 |  
|--|--|
| width | 设置layout viewport 的宽度，为一个正整数，或字符串"width-device" |  
| initial-scale | 设置页面的初始缩放值，为一个数字，可以带小数 |  
| minimum-scale | 允许用户的最小缩放值，为一个数字，可以带小数 |  
| maximum-scale | 允许用户的最大缩放值，为一个数字，可以带小数 |  
| height | 设置layout viewport 的高度，这个属性对我们并不重要，很少使用 |  
| user-scalable | 是否允许用户进行缩放，值为 "no" 或 "yes", no 代表不允许，yes 代表允许 |  
  
```  
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">  
```  
  
**总结：meta标签常用来干什么？**  
  
1. 字符集的编码格式，系统默认是拉丁文的编码格式。但是我们在使用的时候编码格式使用的是万国码UTF-8，如果只想显示中文，可以使用gb2312或者GBK  
2. 还可以设置当前文档的关键字，用于SEO优化。  
3. 还可以设置viewport，用于移动端的显示优化。  
  
  
【一起进步，微信公众号：qdgithub】
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMjI4ODc5MzIsODc3Nzc0NDkyXX0=
-->
