# Array
## Array对象
> 提供对创建任何数据类型的数组的支持。

```javascript
//格式：
var arrayObj = new Array();
var arrayObj = new Array([size]);
var arrayObj = new Array([element0[, element1[, ...[, elementN]]]]);
```

**参数**
- arrayObj
	+ 必选项。要赋值为 `Array` 对象的变量名。
- size
	+ 可选项。可选项数组的大小。由于数组的下标是从零开始，创建的元素的下标将从零到 size。
- element0,...,elementN
	+ 可选项。要放到数组中的元素。这将创建具有 n + 1 个元素的长度为 n + 1 的数组。使用该语法时必须有一个以上元素。

**说明**
> 创建数组后，能够用 [ ] 符号访问数组单个元素，例如： 

```javascript
var my_array = new Array();
for(i = 0; i < 10; i++) {
	my_array[i] = i;
}
x = my_array[4];
```

**注意**

- 由于 Microsoft JScript 中的数组的下标是从零开始的，前面例子中最后一条语句访问数组的第五个元素。该元素中保存的值是 4。
>如果只向 Array 的构造函数传递了一个参数，而该参数是数字，则它必须是无符号32位整数（大约40亿）。
>>该值成为数组的大小。如果该值为数值，但小于0或不为整数，发生运行时错误。

- 如果传递给 Array 构造函数的是单个值并且不是数值，设置 length 属性为1，而且唯一的元素值成为单个的传入的参数。
- 请注意 JScript 数组为解析数组，也就是尽管可以分配多个元素给一个数组，但实际上只有包含数据的元素才存在。这减少了数组使用的内存数量。

**属性**
1. **constructor属性**
> 表示创建对象的函数。

object.constructor
object是对象或函数的名称。 

constructor 属性是所有具有 prototype 的对象的成员。<br>
它们包括除 Global 和 Math 对象以外的所有 JScript 固有对象(Array，Boolean，Date， Function，Global，Math，Number，Object，RegExp，Regular Expression，和 String)。<br>
constructor 属性保存了对构造特定对象实例的函数的引用。 <br>

例如： 

```javascript
x = new String("Hi");
if (x.constructor == String)
      // 进行处理（条件为真）。


或 

function MyFunc {
   // 函数体。
}

y = new MyFunc;
if (y.constructor == MyFunc)
      // 进行处理（条件为真）。
```

2. **prototype属性**
> 返回对象类型原型的引用。

objectName.prototype
objectName 参数是对象的名称。 

用 `prototype` 属性提供对象的类的一组基本功能。 对象的新实例“继承”赋予该对象原型的操作。 
例如，要为 Array 对象添加返回数组中最大元素值的方法。 要完成这一点，声明该函数，将它加入 Array.prototype， 并使用它。 

```javascript
function array_max() {
	var i, max = this[0];
	for(i = 1; i < this.length; i++) {
		if(max < this[i]) max = this[i];
	}
	return max;
}
Array.prototype.max = array_max;
var x = new Array(1, 2, 3, 4, 5, 6);
var y = x.max();
```

所有 JScript 固有对象都有只读的 `prototype` 属性。可以像上例中那样为原型添加功能，但该对象不能被赋予不同的原型。
然而，用户定义的对象可以被赋给新的原型。

3. **length属性(Array)**
> 返回一个整数值，这个整数比数组中所定义的最高位元素的下标大 1。<br>
> 格式：numVar = arrayObj.length

**参数**
- numVar
	+ 必选项。任意数值。
- arrayObj
	+ 必选项。 任意 Array 对象。

**注意**
因为一个数组中的元素并不一定是连续的，所以 length 属性也并不一定就等于数组中的元素个数。<br>
例如，在下面的数组定义中，my_array.length 中就包含 7，而不是 2： 

```javascript
var my_array = new Array( );
my_array[0] = "Test";
my_array[6] = "Another Test";
```
如果 length 属性被赋予了一个比原先值小的数值，那么数组就被截断，所有数组下标等于或者大于 length 属性的新值的元素都会被丢失。 
如果 length 属性被赋予了一个比原先值大的数值，那么数组就被扩展，且所有新建元素都被赋值为 undefined。

## concat
> 返回一个新数组，这个新数组是由两个或更多数组组合而成的。<br>
> 格式：array1.concat([item1[, item2[, . . . [, itemN]]]])

**参数**
- array1 
    + 必选项。其他所有数组要进行连接的 Array 对象。 
- item1,. . ., itemN
    + 可选项。要连接到 array1 末尾的其他项目。

**说明**
> 要加的项目（item1 … itemN）会按照从左到右的顺序添加到数组。<br>
如果某一项为数组，那么添加其内容到 array1 的末尾。<br>
如果该项目不是数组，就将其作为单个的数组元素添加到数组的末尾。<br>
例如：

```javascript
function ConcatArrayDemo() {
	var a, b, c, d;
	a = new Array(1, 2, 3);
	b = "JScript";
	c = new Array(42, "VBScript");
	d = a.concat(b, c);
	// 返回数组 [1, 2, 3, "JScript", 42, "VBScript"]   
	return(d);
}
```
**注意**
> 以下为从源数组复制元素到结果数组： 
> 源数组是指array1和item1...itemN。<br>
> 结果数组是指通过concat联接后返回的新数组。<br>

```javascript
var arrayOne = new Array('hello','world');
var arrayTwo = new Array('你好','世界');
var resArr = arrayOne.concat(arrayTwo); 

console.log(resArr);  // ["hello", "world", "你好", "世界"]
arrayOne[2] = '!';
console.log(resArr); // ["hello", "world", "你好", "世界"]
arrayTwo[2] = '!';
console.log(resArr); // ["hello", "world", "你好", "世界"]

// 对arrayTwo重新赋值，开辟栈，开辟新的引用地址。
arrayTwo = [['你好','世界']];
// resArr中对arrayTwo的引用还是原来的引用地址，并不会随着arrayTwo的重新赋值而变换resArr对原来的地址的引用。
console.log(resArr); // ["hello", "world", "你好", "世界"]

// 重新concat两个数组。则使用的arrayTwo的是最新的引用地址
resArr = arrayOne.concat(arrayTwo);
console.log(resArr); // ["hello", "world", "!", ["你好", "世界"]]

// 向arrayTwo中添加一个数组元素
arrayTwo[0].push('!');
console.log(arrayTwo); // ["你好", "世界", "!"]
console.log(resArr);  // ["hello", "world", "!", ["你好", "世界","!"]]
```

**总结**
> 通过上面的示例可以得出如下结论：

- 对于联接到新数组的数值或字符串，只复制其值。一个数组中值有改变并不影响另一个数组中的值。 
- 对于从正被联接到结果数组的源数组中复制的**对象参数**，复制后仍然指向相同的对象。不论结果数组和源数组中哪一个有改变，都将引起另一个的改变。 


## join()
> 返回字符串值，其中包含了连接到一起的数组的所有元素，元素由指定的分隔符分隔开来。<br>
> 格式：arrayObj.join(separator)

**参数**

- arrayObj
	+ 必选项。Array 对象。
- separator
	+ 必选项。是一个 String 对象，作为最终的 String 对象中对数组元素之间的分隔符。如果省略了这个参数，那么数组元素之间就用一个逗号来分隔。

**示例**

```javascript
new Array(0,1,2,3,4).join(); // "0,1,2,3,4"
new Array(0,1,2,3,4).join('|'); // "0|1|2|3|4"
```

**注意**
> 如果数组中有元素没有定义或者为 null，将其作为空字符串处理。

```javascript
[1,2,,4].join(); // "1,2,,4"
```

## split()
> 方法用于把一个字符串分割成字符串数组。<br>
> 格式：string.split(separator,limit)

**参数**

- separator
	+ 可选。字符串或**正则表达式**，从该参数指定的地方分割 string Object。
- limit
	+ 可选。该参数可指定返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。
	+ 如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。

**示例**

```javascript
"0,1,2,3,4".split(','); // ["0", "1", "2", "3", "4"]
"0|1|2|3|4".split('|',3); // ["0", "1", "2"]
```

## pop()
> 移除数组中的最后一个元素并返回该元素。
> 格式：arrayObj.pop();

**参数**
- arrayObj 
	+ 必选的,是一个 Array 对象。

**注意**
> 如果该数组为空，那么将返回 `undefined`

## push()
> 将新元素添加到一个数组中，并返回数组的新长度值。<br>
> 格式：arrayObj.push([item1 [item2 [. . . [itemN ]]]])

**参数**

- arrayObj
	+ 必选项。一个 Array 对象。
- item, item2,. . . itemN
	+ 可选项。该 Array 的新元素。

**注意**
> push方法将以新元素出现的顺序添加这些元素。<br>
> 如果参数之一为数组，那么该数组将作为单个元素添加到数组中。<br>
> 如果要合并两个或多个数组中的元素，请使用 concat 方法。<br>


## shift()
> 移除数组中的第一个元素并返回该元素。<br>
> 格式：arrayObj.shift()

**参数**
- arrayObj 
	+ 必选的，是一个 Array 对象。

**注意**
> shift方法可移除数组中的第一个元素并返回该元素。<br>
> 返回值是移除的元素。

## unshift()
> 将指定的元素插入数组开始位置并返回该数组。<br>
> 格式：arrayObj.unshift([item1[, item2 [, . . . [, itemN]]]])

**参数**

- arrayObj
	+ 必选项。一个 Array 对象。
- item1, item2,. . .,itemN
	+ 可选项。将插入到该 Array 开始部分的元素。

**注意**
> `unshift` 方法将这些元素插入到一个数组的开始部分，所以这些元素将以参数序列中的次序出现在数组中。
返回值是 `arrayObj` 的新长度。

```javascript
new Array(1,2,3,4,5,6).unshift('0'); // 7
```

## reverse()
> 返回一个元素顺序被反转的 Array 对象。 <br>
> 格式：arrayObj.reverse()

**注意**

> reverse 方法将一个 Array 对象中的元素位置进行反转。<br>
> 在执行过程中，这个方法并不会创建一个新的 Array 对象。 <br>
> 如果数组是不连续的，reverse 方法将在数组中创建元素以便填充数组中的间隔。<br>
> 这样所创建的全部元素的值都是 undefined。

```javascript
new Array(0,1,2,3,4).reverse(); // [4, 3, 2, 1, 0]
```

## slice()(Array)
> 返回一个数组的一段。
> 格式：arrayObj.slice(start, [end]) 

**参数**
- arrayObj
	+ 必选项。一个 Array 对象。
- start 
	+ 必选项。arrayObj 中所指定的部分的开始元素是从零开始计算的下标。 
- end 
	+ 可选项。arrayObj 中所指定的部分的结束元素是从零开始计算的下标。

**注意**

> slice 方法返回一个 Array 对象，其中包含了 arrayObj 的指定部分。 

- slice 方法一直复制到 end 所指定的元素，但是不包括该元素。
	+ 如果 start 为负，将它作为 length + start处理，此处 length 为数组的长度。
	+ 如果 end 为负，就将它作为 length + end 处理，此处 length 为数组的长度。
	+ 如果省略 end ，那么 slice 方法将一直复制到 arrayObj 的结尾。
	+ 如果 end 出现在 start 之前，不复制任何元素到新数组中。

**示例**
在下面这个例子中，除了最后一个元素之外，myArray 中所有的元素都被复制到 newArray 中： 
```javascript
newArray = myArray.slice(0, -1);
```

## splice()
> 从一个数组中移除一个或多个元素，如果必要，在所移除元素的位置上插入新元素，返回所移除的元素。<br>
> 格式：arrayObj.splice(start, deleteCount, [item1[, item2[, . . . [,itemN]]]])

参数

- arrayObj
	+ 必选项。一个 Array 对象。
- start
	+ 必选项。指定从数组中移除元素的开始位置，这个位置是从 0 开始计算的。
- deleteCount
	+ 必选项。要移除的元素的个数。
- item1, item2,. . .,itemN
	+ 必选项。要在所移除元素的位置上插入的新元素。

**注意**
> splice 方法可以移除从 start 位置开始的指定个数的元素并插入新元素，从而修改 arrayObj。<br>
> 返回值是一个由所移除的元素组成的新 Array 对象。

## sort()

> 返回一个元素已经进行了排序的 Array 对象。<br> 
> 格式：arrayobj.sort(sortfunction) 

**参数**

- arrayObj
	+ 必选项。任意 Array 对象。
- sortFunction
	+ 可选项。是用来确定元素顺序的函数的名称。如果这个参数被省略，那么元素将按照 `ASCII` 字符顺序进行升序排列。 

**注意**
> sort 方法将 Array 对象进行适当的排序；在执行过程中并不会创建新的 Array 对象。 

**如果为 sortfunction 参数提供了一个函数，那么该函数必须返回下列值之一:** 

- 负值，如果所传递的第一个参数比第二个参数小。 
- 零，如果两个参数相等。 
- 正值，如果第一个参数比第二个参数大。 

**示例**
```javascript
function SortDemo() {
	var a, l;
	a = new Array("X", "y", "d", "Z", "v", "m", "r");
	l = a.sort(); // 排序数组。
	return(l); // 返回排序的数组。
}
```

## toLocaleString()
> 返回一个日期，该日期使用当前区域设置并已被转换为字符串。 <br>
> 格式：dateObj.toLocaleString()

**参数**
- dateObj 
	+ 必选项，为任意的 Date 对象。

**说明**
`toLocaleString` 方法返回一个 String 对象，这个对象中包含了用当前区域设置的默认格式表示的日期。 

- 对于公元 1601 和 1999 之间的时间，日期格式要按照用户的“控制面板”中“区域设置”来确定。 
- F对于此区间外的其他时间，使用 toString 方法的默认格式。 

例如，同样是 1 月 5 日，在美国，toLocaleString 可能会返回 "01/05/96 00:00:00"，
而在欧洲，返回值则可能是 "05/01/96 00:00:00"，因为欧洲的惯例是将日期放在月份前面。

注意 toLocaleString 只用来显示结果给用户；不要在脚本中用来做基本计算，因为返回的结果是随机器不同而不同的。

示例
下面这个例子说明了 toLocaleString 方法的用法。 
```javascript
function demo(){
   var d, s;
   d = new Date();                // 创建 Date 对象。
   s = "Current setting is ";
   s += d.toLocaleString();       // 转换为当前区域。
   return(s);                     // 返回转换的日期。
}
```

## toString()
> 返回对象的字符串表示。<br>
> 格式：objectname.toString([radix])

**参数**

- objectname
	+ 必选项。要得到字符串表示的对象。
- radix
	+ 可选项。指定将数字值转换为字符串时的进制。

**注意**
toString 方法是所有内建的`JS`对象的成员。它的操作依赖于对象的类型：

| 对象      |    操作  | 
| :-------- | --------:|
| Array  |  将 Array 的元素转换为字符串。结果字符串由逗号分隔，且连接起来。  |
| Boolean     |   如果 Boolean 值是 true，则返回 “true”。否则，返回 “false”。 | 
| Date      |     返回日期的文字表示法。 | 
| Error      |    返回一个包含相关错误消息的字符串。 | 
| Function      |     返回如下格式的字符串，其中 functionname 是被调用 toString 方法函数的名称： 
function functionname( ) { [native code] } | 
| Number      |    返回数字的文字表示。 | 
| String      |    返回 String 对象的值。 | 
| 默认      |   返回 “[object objectname]”，其中 objectname 是对象类型的名称。 | 

**示例**
```javascript
function CreateRadixTable() {
	var s, s1, s2, s3, x; // 声明变量。
	s = "Hex    Dec   Bin \n"; // 创建表头。
	for(x = 0; x < 16; x++) // 根据所示值的
	{ // 数字建立
		switch(x) // 表尺寸。
		{ // 设置栏目间空间。         
			case 0 : 
				s1 = "      ";
				s2 = "    ";
				s3 = "   ";
				break;
			case 1:
				s1 = "      ";
				s2 = "    ";
				s3 = "   ";
				break;
			case 2:
				s3 = "  ";
				break;
			case 3:
				s3 = "  ";
				break;
			case 4:
				s3 = " ";
				break;
			case 5:
				s3 = " ";
				break;
			case 6:
				s3 = " ";
				break;
			case 7:
				s3 = " ";
				break;
			case 8:
				s3 = "";
				break;
			case 9:
				s3 = "";
				break;
			default:
				s1 = "     ";
				s2 = "";
				s3 = "    ";
		} // 转换为十六进制、十进制、二进制。
		s += " " + x.toString(16) + s1 + x.toString(10)
		s += s2 + s3 + x.toString(2) + "\n";
	}
	return(s); // 返回整个 radix 表。
}
```


## valueOf()
> 返回指定对象的原始值。<br>
> 格式：object.valueOf( )

**参数**
-object
	+ 必选项，参数是任意固有`Js`对象。 

**注意**
> 每个 `Js` 固有对象的 valueOf 方法定义不同。<br>
> Math 和 Error 对象没有 valueOf 方法。 

| 对象      |    返回值  | 
| :-------- | --------:|
| Array  |  数组的元素被转换为字符串，这些字符串由逗号分隔，连接在一起。其操作与 Array.toString 和 Array.join 方法相同。 |
| Boolean  |  Boolean 值。 |
| Date  |  存储的时间是从 1970 年 1 月 1 日午夜开始计的毫秒数 UTC。 |
| Function  |  函数本身。|
| Number  |  数字值。 |
| Object  |  对象本身。这是默认情况。 |
| String  |  字符串值。 |
