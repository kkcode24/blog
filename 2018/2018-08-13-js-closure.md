## 概念
**先看一段代码**

```
function t1(){
     var age = 22;
     function t2(){
        alert(age);
     }
     return t2;
}
var tmp = t1();
var age = 99;
tmp();//22
```
在大部分的语言中，t1被调用执行，则申请内存，并把局部变量push入栈。t1函数执行完毕，内部的局部变量随着函数的退出而销毁。因此，age = 22这个局部变量就消失了。

> 但是在js中，age = 20这个变量却被t2捕捉，即使t1执行完毕，通过t2依然能够访问该变量。这种情况》返回的函数，并非孤立的函数，甚至把其周围的变量环境，形成了封闭的“环境包”，共同返回，所以叫闭包。

**概括：函数的作用域取决于声明时，而不取决于调用时。**

## 闭包计数器

> **问题：**多个人开发js程序，需要一个全局计数器，多个人的函数共同用一个计数器，计数器一直增长。
<br/>
>**答：**设立一个全局变量， window.cnt = 0;调用：++window.cnt;
分析：虽然这个方法可行，但是应该避免过多的使用全局变量，防止全局变量被污染。当你引用其他人的js程序时，如果他的程序内也有一个全局变量window.cnt = 'hello';那么你的全局变量就会被无情的改变了

**那么如何使用闭包做计数器呢？**
### 第一版
```
function counter(){
   var cnt = 0;
   var cnter = function (){
      return ++cnt;
   }
   return cnter;
}
var inc = counter();
alert(inc());
alert(inc());
alert(inc());
```
> 当counter执行完毕后，除了返回的cnter函数谁也访问不到cnt变量了
> 但是，这样有2个问题，全局多了一个counter函数和inc变量
> 下面，我们使用立即执行函数。

### 第二版简化

```
var cnt = (function (){
	var cnt = 0;
	return function(){
		return ++cnt;
    }
})();

alert(cnt());
alert(cnt());
```
> 疑问：cnt不依然是全局变量吗？

**在工作中，如何避免全局污染和冲突。**

 1. 统一放在一个全局对象上，如:jquery ->$
 2. 每人用自己的命名空间。如：省份+姓名（hn-yk）河南+yk

### 第三版
> jquery的计数器插件形式

```
var $ = {};//jquery对象
$.cnt = (function (){
	var cnt = 0;
	return function(){
		return ++cnt;
	}
})();

alert($.cnt());
alert($.cnt());
```

> 个人命名空间，在团队开发中也很常见。

```
var HN_YK = {};
HN_YK.cnt = (function (){
	var cnt = 0;
	return function(){
		return ++cnt;
	}
})();

alert(HN_YK.cnt());
alert(HN_YK.cnt());
```

**【一起进步，微信公众号：qdgithub】**
