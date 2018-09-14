作为jquery的作者。John Resig在博客里记录了一种class的实现，原文[在此](https://johnresig.com/blog/simple-javascript-inheritance/)

下面是源码：

```javascript
(function(){

  var initializing = false, fnTest = /xyz/.test(function(){xyz;}) ? /\b_super\b/ : /.*/;
 
  this.Class = function(){};

  Class.extend = function(prop) {
    var _super = this.prototype;

    initializing = true;
    var prototype = new this();
    initializing = false;
   
    for (var name in prop) {

      prototype[name] = typeof prop[name] == "function" &&
        typeof _super[name] == "function" && fnTest.test(prop[name]) ?
        (function(name, fn){
          return function() {
            var tmp = this._super;
            this._super = _super[name];
            var ret = fn.apply(this, arguments);
            this._super = tmp;
            return ret;
          };
        })(name, prop[name]) : prop[name];
    }
   
    function Class() {
      if ( !initializing && this.init ){
		this.init.apply(this, arguments); 
	  }
    }
	
    Class.prototype = prototype;
    Class.prototype.constructor = Class;
    Class.extend = arguments.callee;
   
    return Class;
  };
  
})();
```

我们把调用方法带入进去看看其到底是怎么实现的。

```javascript

var Person = Class.extend({
  init: function(isDancing){
    this.dancing = isDancing;
  },
  dance: function(){
    return this.dancing;
  }
});

因此
prop = {
  init: function(isDancing){
    this.dancing = isDancing;
  },
  dance: function(){
    return this.dancing;
  }
}

var _super = 超级父类的原型对象
var prototype = 由超级父类生成的实例
initializing = false;

**第一次for循环**

 typeof prop["init"] == "function" // true
 typeof _super["init"] == "function" // false
 typeof fnTest.test(function(isDancing){this.dancing = isDancing;}) // false


**循环结束后，所以**

 prototype["init"] = prop["init"];
 prototype["dance"] = prop["dance"]

子类：

 function Class(){
	if(!initializing && this.init){
	    this.init.apply(this, arguments);
	}
 }

 // 赋值原型链，完成继承
 Class.prototype = prototype;

 // 改变constructor的指向
 Class.prototype.constructor = Class;

 // 为子类Class添加extend方法
 Class.extend = arguments.callee;

 // 返回子类
 return Class;


 因此
	
	Person = 返回的子类Class;

 而子类Class
 此时的this指向超级父类Class
	function Class(){
	    if(!initializing && this.init){
		this.init.apply(this, arguments);
	    }
	} 

	Class.prototype = {
	    constructor: 指向自身的构造函数Class
	    init: function(isDancing){this.dancing = isDancing},
	    dance: function(){ return this.dancing;}
	}

	Class.extend方法与超级父类相同


调用，使用Person构造函数，创建一个实例。

 var p = new Person(true);
 当new Person()时，实际上调用的是 返回的子类Class
 此时this指向刚刚创建生成的实例p,
 因此if条件为真，执行 this.init.apply(this,arguments)

 init: function(true){this.dancing = true;}

 此时调用p.dance();则其输出为true

```

**此时，Person类指向子类Class**

```
function Class(){
	if(!initializing && this.init){
		this.init.apply(this, arguments);
	}
} 

Class.prototype = {
	constructor: 指向自身的构造函数Class
	init: function(isDancing){this.dancing = isDancing},
	dance: function(){ return this.dancing;}
}

Class.extend方法与超级父类相同
```

**当基于Person进行继承时**

```javascript
var qdgithub = Person.extend({
  init: function(){
    this._super( false );
  },
  dance: function(){
    // Call the inherited version of dance()
    return this._super();
  },
  swingSword: function(){
    return true;
  }
});
 

 因此

 var _super = Person的原型对象
 var prototype = Person类生成的实例
 initializing = false;

** 第一次for循环：**

 typeof prop["init"] == "function"   // true
 typeof _super["init"] == "function"  // true
 fnTest.test(function(){this._super(false)}) // true

 因此进入闭包，把Person类的对象属性方法混入子类上。

 参数： 
 name: "init"
 fn: function(){this._super(false)}

 返回匿名函数
 prototype["init"] = function(){
	var tmp = this._super;
        this._super = function(isDancing){this.dancing = isDancing;};
	fn = function(){this._super(false)};
        var ret = fn.apply(this, arguments);
        this._super = tmp;
        return ret;
 }

 第二次循环同第一次循环
 prototype["dance"] = function(){
	var tmp = this._super;
        this._super = function(){ return this.dancing;}
	fn = function(){return this._super();}
        var ret = fn.apply(this, arguments);
        this._super = tmp;
        return ret;
 }

 第三次循环
 prototype["swingSword"] = function(){return true;}


返回子类
function Class(){
	if(!initializing && this.init){
		this.init.apply(this, arguments);
	}
} 

Class.prototype = {
	constructor: 指向自身的构造函数Class
	init: function(){
		var tmp = this._super;
        	this._super = function(isDancing){this.dancing = isDancing;};
		fn = function(){this._super(false)};
        	var ret = fn.apply(this, arguments);
        	this._super = tmp;
        	return ret;
	},
	dance: function(){
		var tmp = this._super;
        	this._super = function(){ return this.dancing;}
		fn = function(){return this._super();}
        	var ret = fn.apply(this, arguments);
        	this._super = tmp;
        	return ret;
	},
	swingSword: function(){return true;}
}

Class.extend方法与超级父类相同


因此:QdGithub = 返回的子类Class（就是上面刚刚分析，返回的子类Class）

 调用，使用QdGithub构造函数，创建一个实例。

 var n = new QdGithub(true);
 当new QdGithub()时，实际上调用的是 返回的子类Class
 此时this指向刚刚创建生成的实例n,
 因此if条件为真，执行 this.init.apply(this,arguments),把实例n的dancing属性设置为false

 init: function(){
	var tmp = this._super;
        this._super = function(isDancing){this.dancing = isDancing;};
	fn = function(){this._super(false)};
        var ret = fn.apply(this, arguments);
        this._super = tmp;
        return ret;
 } 

 此时调用n.dance();则其输出为false
 此时调用n.swingSword();则其输出为true
 
// Should all be true
p instanceof Person && p instanceof Class &&
n instanceof Ninja && n instanceof Person && n instanceof Class
 
```
**【一起探讨，微信公众号：qdgithub】**
