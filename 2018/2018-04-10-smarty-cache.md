# smarty-cache
> 缓存？<br/>
> 就是把页面内容保存到磁盘上，下次访问相同页面，直接返回保存的内容，减轻了数据库的压力。

```php

require('../smarty3/smaty.class.php');
require('../mySmaty.class.php');

$smarty = new MySmarty();


//思考：
// 如下数据，短期内不会变化，比如1小时，很少变化，而一小时内却有1000位用户访问
// 那么，这1000次，每次都是同样的内容，但是访问了数据库1000次
// 这时，就产生了性能上的浪费。这时就可以开启smarty缓存了


{
	$conn = mysql_connect('localhost','root','111111');
	mysql_query("set names utf8");
	mysql_query("set shop");


	$sql = "select goods_id,goods_name,goods_price from  goods limit 5";
	$rs = mysql_query($sql,$conn);

	$goods = array();
	while($row = mysql_fetch_assoc($rs)){
		$goods[] = $row;
	}

	// 最终，$goods 数组如下所示
	$goods = array('0'=>array('goods_id'=>'1','goods_name'=>'iPad air2','goods_price'=>'4673'),
    '2'=>array('goods_id'=>'2','goods_name'=>'iPhone5s','goods_price'=>'3300'));

   echo "我走了数据库";
}



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
        缓存
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

## smarty页面缓存的用法

1. 开启
2. 配置缓存的生命周期
3. 设置缓存目录，用于存储缓存文件
4. 判断是否缓存并是否从数据库中取数据。
5. 输出

```php

require('../smarty3/smaty.class.php');
require('../mySmaty.class.php');

$smarty = new MySmarty();

// 1. 开启缓存
$smarty->caching = true;

// 2. 设置一个缓存周期
$smarty->cache_lifetime = 3600;

// 3. 设置缓存目录，用于存储缓存文件
$smarty->cache_dir = './cache';


if(!$smarty->isCached('demo.html')){ // 4. 判断demo.html是否已经被缓存了
	$conn = mysql_connect('localhost','root','111111');
	mysql_query("set names utf8");
	mysql_query("set shop");


	$sql = "select goods_id,goods_name,goods_price from  goods limit 5";
	$rs = mysql_query($sql,$conn);

	$goods = array();
	while($row = mysql_fetch_assoc($rs)){
		$goods[] = $row;
	}

	// 最终，$goods 数组如下所示
	$goods = array('0'=>array('goods_id'=>'1','goods_name'=>'iPad air2','goods_price'=>'4673'),
    '2'=>array('goods_id'=>'2','goods_name'=>'iPhone5s','goods_price'=>'3300'));

   echo "我走了数据库";

   $smarty->assign('goods',$goods);
}


// 思考：
// 如果我在页面中需要显示当前时间，该如何，它会把时间也缓存
// 这就需要用到smarty的局部缓存
// smarty在页面缓存的情况下，可以设置部分内容不缓存
// 因为页面的某部分，如随机广告，股票信息，时间等

$smarty->display('demo.html');

```


## smarty页面局部缓存的用法

1. 在模板中控制（demo.html），某标签不缓存 
	+ ```{$标签 nocache}```
2. 控制一片标签不缓存
	+ ```{nocache} 这里是不缓存的内容标签 {/nocache}```
3. 在php中就控制不缓存,第三个参数是nocache,true表示不缓存
	
	+ ```php $smarty->assign('time2',$time2,true); ```
4. 在php中和模板中分别控制
	+ php中
	```php
	function insert_welcome($parm){
		return "你好" . $parm['name'] . rand(1000,9999);
	}
	```
	+ 模板中 ```{insert name = 'welcome' name='王五'}```

> 注意： 不缓存的标签，要保证总能从php处得到值。


## 单模板多缓存
> 根据地址的goods_id来取商品的信息

```php

$smarty = new MySmarty();

// 1. 开启缓存
$smarty->caching = true;
$smarty->cache_lifetime = 3600;
$smarty->cache_dir = './cache';

$goods_id = $_GET['goods_id'] + 0;

if(!$smarty->isCache('goods.html',$goods_id)){
    $conn = mysql_connect('localhost','root','111111');
    mysql_query("set names utf8");
    mysql_query("set shop");

    $sql = "select goods_id,goods_name,goods_price from  goods where goods_id=" . $goods_id;
    $rs = mysql_query($sql,$conn);

    $goods = mysql_fetch_assoc($rs);

    // 最终，$goods 数组如下所示
    $goods = array('0'=>array('goods_id'=>'1'));
    $smarty->assign('goods',$goods);
    echo "走了数据库";
}

$smarty->display('goods.html',$goods_id);
```

> goods.html

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
    {literal}
        单页面多缓存: 
            典型场景：
                为商品模板设置缓存，但是goods.php?id=N
                当缓存后，所有的商品页面都一样了，因为被缓存了，这显示不合适
            思考：
                能否为同一个模板，生成不同的缓存文件呢？
                比如，根据id的不同，来生成各个商品的缓存页面。
            答：
                可以，用到“单模板多缓存”
            原理：
                是生成缓存的时候，可以再传一个“缓存id”，如果id不同，生成的缓存文件则不同
            在何处传：
                在php页面中，$smarty->assign('goods.html',$goods_id);
            总结：
                你的哪些参数要影响页面的内容，就需要把哪些参数当成“缓存id”
                比如：page=3&cat=4 , 第四栏目的第三页，
                page和cat都要影响结果，因此这2个参数都要传入
            经典案例：
                ecshop商城
    {/literal}

	<table>
		<tr><td>序号</td><td>商品名</td><td>价格(元)</td></tr>
		<tr><td>{$goods.goods_id}</td><td>{$goods.goods_name}</td><td>{$goods.goods_price}</td></tr>
	</table>
</body>
</html>
```







* 参考燕十八老师的smarty视频教程，其官网地址是[布尔教育](http://www.itbool.com)