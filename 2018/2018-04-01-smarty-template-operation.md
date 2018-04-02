# smarty之模板操作

## 标签的数学运算

```php 

require('../smarty3/smaty.class.php');
require('../mySmaty.class.php');

$smarty = new MySmarty();

$smarty->assign('age',88);
$smarty->display('jisuan.html');

```

> jisuan.html

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
	
	<p>标签是可以参与运算的，但是不推荐</p>

	{$age + 10}
	{$age - 10}
	{$age * 10}
	{$age / 10}
	等...
</body>
</html>
```

## 标签的逻辑运算

> logical-operation.html

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
	{if $status === 1}
		<h2>处方有效</h2>
	{else}
		<h2>处方过期</h2>
	{/if}

	<p>从地址栏里传参</p>

	{if $smarty.get.sex === 0}
		you are a man
	{elseif $smarty.get.sex === 1}
		you are a women
	{else}
		人妖
	{/if}
</body>
</html>
```

## 标签的循环

* `for`,`while`
* `foreach`,`section`

> `section`功能多，配置选项多，原理和foreach一样，我们在开发中，
用foreach可以满足绝大部分循环情况

### for循环

```php

require('../smarty3/smaty.class.php');
require('../mySmaty.class.php');

$smarty = new MySmarty();

// for循环从1到10
$smarty->assign('start',1);
$smarty->assign('end',10);

$smarty->display('loop.html');

```

> loop.html

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>

	<pre>
		{literal}
			{for $i = $start to $end}
			{/for}
		{/literal}
		原理是$i的初始值为$start,$i不断增加，直到等于$end
	</pre>
	

	{for $i = $start to $end step 2}
			
			{$i}

	{/for}

	$i@total,总循环次数
	$i@iteration,当前循环是第几次
	$i@first,首次循环
	$i@last,最后一次循环

</body>
</html>
```

### foreach循环

> 二位数组的循环，如：新闻列表，会员列表，商品列表

```php

require('../smarty3/smaty.class.php');
require('../mySmaty.class.php');

$smarty = new MySmarty();

$goods = array('0'=>array('goods_id'=>'1','goods_name'=>'iPad air2','goods_price'=>'4673'),
    '2'=>array('goods_id'=>'2','goods_name'=>'iPhone5s','goods_price'=>'3300'));

$smarty->assign('goods',$goods);
$smarty->display('demo.html');

```

> demo.html

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
		{literal}
			
			foreach循环数组的典型写法:
			{foreach $source as $itemvar}
			{foreach $arrayvar as $keyvar=>$itemvar}

		{/literal}

	<table>
		<tr><td>序号</td><td>商品名</td><td>价格(元)</td></tr>

		{foreach $goods as $key=>$item}
		<tr><td>{$item.goods_id}</td><td>{$item.goods_name}</td><td>{$item.goods_price}</td></tr>
		</foreach>

	</table>
</body>
</html>
```

* 参考燕十八老师的smarty视频教程，其官网地址是[布尔教育](http://www.itbool.com)
