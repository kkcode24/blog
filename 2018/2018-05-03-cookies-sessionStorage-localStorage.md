[转：cookies,sessionStorage和localStorage的区别](http://www.cnblogs.com/GumpYan/p/5708692.html)

1.Web Storage　　
> 软件编程希望通过一些手段来持久化的存储一些有用的数据。对于网络化编程，一般将这项任务交给了服务器端的数据库或者浏览器端的cookie。随着HTML5的出现，web开发又有了两种选择：Web Storage和Web SQL Database.

- Web Storage有两种形式：
  + LocalStorage（本地存储）
  + sessionStorage（会话存储）。
> 这两种方式都允许开发者使用js设置的键值对进行操作，在在重新加载不同的页面的时候读出它们。这一点与cookie类似。


- 与cookie不同的是：
> Web Storage数据完全存储在客户端，不需要通过浏览器的请求将数据传给服务器，因此相比cookie来说能够存储更多的数据，大概5M左右。单个cookie在客户端的限制是3K，就是说一个站点在客户端存放的COOKIE不能3K。
　　　
- LocalStorage和sessionStorage功能上是一样的，但是存储持久时间不一样
> LocalStorage：浏览器关闭了数据仍然可以保存下来，并可用于所有同源（相同的域名、协议和端口）窗口（或标签页）永久存储，永不失效，除非手动删除sessionStorage：数据存储在窗口对象中，窗口关闭后对应的窗口对象消失，存储的数据也会丢失。就是浏览器窗口关闭就失效了。
  注意：sessionStorage 都可以用localStorage 来代替，但需要记住的是，在窗口或者标签页关闭时，使用sessionStorage 存储的数据会丢失。

- Storage类的两个实例　
> 使用 local storage和session storage主要通过在js中操作这两个对象来实现，分别为window.localStorage和window.sessionStorage. 这两个对象均是Storage类的两个实例，自然也具有Storage类的属性和方法。但是cookie需要前端开发者自己封装setCookie，getCookie。但是Cookie也是不可以或缺的：Cookie的作用是与服务器进行交互，作为HTTP规范的一部分而存在 ，而Web Storage仅仅是为了在本地“存储”数据而生。

![Storage类的相关成员](http://upload-images.jianshu.io/upload_images/5546717-6d8df50a169f0411.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其用法：
参数设置很简单，如下例：
`localStorage.setItem('age', 40); `
访问一个存储的数据一样很容易：
` = .getItem('age'); `
可以这样删除一个特定的键值对：
`localStorage.removeItem('age');` 
或者删除所有的键值对：
`localStorage.clear();` 

- sessionStorage与页面 js 数据对象的区别:
> sessionStorage只要是同源的同窗口（Tab）中，刷新页面或者进入不同的页面数据对象仍然被保存，也就是说只要浏览器窗口不关闭，加载页面（同源）或刷新页面，数据仍存在。
 
2.cookie与session的区别于联系
- cookie与session的区别：
  + cookie数据保存在客户端，session数据保存在服务器端
> 如果web服务器端使用的是session，那么所有的数据都保存在服务器上，客户端每次请求服务器的时候会发送当前会话的sessionid，服务器根据当前sessionid判断相应的用户数据标志，以确定用户是否登录或具有某种权限。由于数据是存储在服务器上面，所以你不能伪造，但是如果你能够获取某个登录用户的 sessionid，用特殊的浏览器伪造该用户的请求也是能够成功的。sessionid是服务器和客户端链接时候随机分配的，一般来说是不会有重复，但如果有大量的并发请求，也不是没有重复的可能性。

> 如果浏览器使用的是cookie，那么所有的数据都保存在浏览器端，比如你登录以后，服务器设置了cookie用户名，那么当你再次请求服务器的时候，浏览器会将用户名一块发送给服务器，这些变量有一定的特殊标记。服务器会解释为cookie变量，所以只要不关闭浏览器，那么cookie变量一直是有效的，所以能够保证长时间不掉线。如果你能够截获某个用户的 cookie变量，然后伪造一个数据包发送过去，那么服务器还是认为你是合法的。所以，使用 cookie被攻击的可能性比较大。如果设置了的有效时间，那么它会将 cookie保存在客户端的硬盘上，下次再访问该网站的时候，浏览器先检查有没有 cookie，如果有的话，就读取该 cookie，然后发送给服务器。如果你在机器上面保存了某个论坛 cookie，有效期是一年，如果有人入侵你的机器，将你的  cookie拷走，然后放在他的浏览器的目录下面，那么他登录该网站的时候就是用你的的身份登录的。所以 cookie是可以伪造的。当然，伪造的时候需要主意，直接copy    cookie文件到 cookie目录，浏览器是不认的，他有一个index.dat文件，存储了 cookie文件的建立时间，以及是否有修改，所以你必须先要有该网站的 cookie文件，并且要从保证时间上骗过浏览器。

- cookie和session的共同之处：
  + cookie和session都是用来跟踪浏览器用户身份的会话方式。

**栗子1：**

- 有关cookie和session的描述，下面错误的是？ D
  + A.cookie数据存放在客户的浏览器上，session数据放在服务器上
  + B.session是针对每一个用户的，变量的值保存在服务器上，用一个sessionID来区分是哪个用户session变量
  + C.保存这个session id的方式可以采用cookie
  + D.只要关闭浏览器，session就消失了
>原因：session在服务端，关闭浏览器不会被删除，会删除cookie
栗子2：

- 浏览器在一次 HTTP 请求中，需要传输一个 4097 字节的文本数据给服务端，可以采用那些方式?e
  + a.存入indexDB
  + b.写入cookie
  + c.放在url参数
  + d.写入session
  + e.使用post
  + f.放在local storage

> 解析：　IndexdDB 是 HTML5 的本地存储，把一些数据存储到浏览器（客户端）中，当与网络断开时，可以从浏览器中读取数据，用来做一些离线应用。<br>
Cookie 通过在客户端 ( 浏览器 ) 记录信息确定用户身份，最大为 4 kb =4096b。
url 参数用的是 get 方法，从服务器上获取数据，大小不能大于 2 kb 。
Session 是服务器端使用的一种记录客户端状态的机制 。
post 是向服务器传送数据，数据量较大。
local Storage 也是 HTML5 的本地存储，将数据保存在客户端中（一般是永久的）。


[转：HTML5本地存储——Web SQL Database](http://www.cnblogs.com/dolphinX/p/3405335.html) 
