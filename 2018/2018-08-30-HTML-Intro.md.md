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
| http-equiv |content-type，expires，refresh，set-cookie  |把 content 属性关联到 HTTP 头部。|
| name | -   author, description, keywords, generator,revised,others | 把content属性关联到一个名称。 |
| scheme | some_text | 定义用于翻译 content 属性值的格式。 |

O

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUzOTI1NTg5OV19
-->