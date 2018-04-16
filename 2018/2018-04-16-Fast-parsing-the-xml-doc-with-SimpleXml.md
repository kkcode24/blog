# 用simpleXml快速解析文档

**知识点**
- simpleXml
- xpath

**1个实战**
- 用xml充当数据库，做词典查询

simpleXml解析xml文件灰常简单
因为它一次性把xml文档解析成一个对象。

```xml
<!-- book.xml -->
<?xml version="1.0" encoding="utf-8"?>
<bookstore>
	<book category="COOKING" id="id2">
		<title lang="en">Everyday Italian</title>
		<author>Giada De Laurentiis</author>
		<year>2005</year>
		<price>30.00</price>
	</book>
	<book category="武侠">
		<title lang="中文">侠客行</title>
		<author>金庸</author>
		<year>2005</year>
		<price>29.99</price>
	</book>
	<book category="网页">
		<title lang="中文">Jquery 7日通</title>
		<author>小二虎</author>
		<year>2003</year>
		<price>49.99</price>
	</book>
	<book category="网页">
		<title lang="en">Learning XML</title>
		<author>Erik T. Ray</author>
		<year>2003</year>
		<price>39.95</price>
		<edition>第三版</edition>
	</book>
</bookstore>
```


```php
// 从文件载入XML文档
$simxml = simplexml_load_file('./book.xml');

//print_r($simxml);

// echo $simxml->book[1]->title;

// 看看bookstore下面有几本书
echo '有',$simxml->count(),'个子元素<br />';

$sons = $simxml->children();

foreach($sons as $s) {
    echo '分别有',$s->count(),'个子元素,当前元素名是:',$s->getName(),'<br />';
}
```
