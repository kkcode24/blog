## requirejs的使用方法

1. 在页面中引入requirejs
2. 定义其他的模块
3. 在main.js中引入其他的js模块

具体代码如下：

```javascript

// 第一步: 在html中引入require.js
<script src="js/require.js" data-main="js/main"></script>

// 第二步，定义其他的模块
// moduleA.js
define(function(){
	return {
		GUIDEIDGET:	false,
		NAVWIDGET:	false,
		SCREENWIDGET:	false,
		READMODE:	false
	};
});

// 第三步：在main.js中引入moduleA.js

require(["moduleA"],function(moduleA){
	console.log(moduleA.GUIDEIDGET);
});

```

## 猜测requirejs的实现步骤

1. 在页面中引入`requirejs`,并指定`main.js`的路径。
2. requirejs分析main.js的配置及模块引入

分析如下：
> requirejs中，定义了模块定义的方法`define`,
取得`script`标签上的`data-main`属性的值。
并使用require函数加载到页面中。
而在`main.js`中,require函数可能加载了多个模块。
当所有的模块都加载完毕后，调用callback函数。

## 实现require函数


```javascript
// 这是简易的require.js

/**
 * @descrition 实现js的加载
 * @param arr  js的路径数组
 * @param callback 当所有的js都加载完后的回调函数
 * @date 2018-09-13
 * @Author Yike
*/
// 导出方法的简陋实现
window.exports = {};

function require(arr,callback){
	if(!(arr instanceof Array)){
		console.error("arr is not a Array");
		return false;
	}
	
	// REQ_TOTAL  标记已加载成功个数
	// EXP_ARR    记录各个模块的顺序
	// REQLEN     定义共需要加载多少个js
	var REQ_TOTAL = 0,
		EXP_ARR = [],
		REQLEN = arr.length;
		
		console.log(arr);
	
	arr.forEach(function(req_item,index,arr){
		// 创建script的标签并放到页面中
		var $script = createScript(req_item,index);
		document.body.appendChild($script);
		
		(function($script){
			//检测script的onload事件
			$script.onload = function(){
				REQ_TOTAL++;
				var script_index = $script.getAttribute('index');
				// 把导出对象放到数组中
				EXP_ARR[script_index] = exports;
				// 重置对象
				window.exports = {};

				//所有js加载成功后，执行callback函数。
				if (REQ_TOTAL == REQLEN) {
					callback && callback.apply(exports, EXP_ARR);
				}
			}
		})($script);
	})
}

//创建一个script标签
function createScript(src, index){
	var $s = document.createElement('script');
	$s.setAttribute('src',src);
	$s.setAttribute('index',index);
	return $s;
}

```

## 测试

**定义A模块**

```javascript
// moduleA.js

exports.define = {
  name: 'ModuleA',
  desc: 'this is other way to define ',
  sayHello: function() {
    console.log(this.name  + " : " +  this.desc);
  }
}

```

**定义B模块**

```javascript
// moduleB.js

exports.define = {
  name: 'ModuleB',
  desc: 'this is other way to define ',
  sayWorld: function() {
    console.log(this.name + " : " + this.desc);
  }
}

```

**在页面中引入刚才写的require.js**
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	
	<script src="require.js"></script>
	<script src="main.js"></script>
</body>
</html>
```

> 至此，就完成了一个简单的requirejs。
还有许多需要优化，比如:

1. 自动识别main.js
2. js模块的异步加载，这里的实现是按照顺序加载的js。
3. 模块之间的依赖关系。比如moduleA依赖ModuleB
等等...

**【一起讨论,欢迎关注微信公众号:qdgithub】**
