# http

**重要性**
> 无论是以后用webserverice，还是用rest做大型架构，都离不开对HTTP协议的认识。

甚至可以简化说：

- webserverice = http协议+XML
- Rest = HTTP协议 + json
- 各种API一般也是用http+xml/json来实现的

做采集，小偷站也需要对HTTP协议有所了解。

**什么是协议**
> 计算机中的协议和现实中的协议是一样的，一式双份/多份。
双方/多方都遵循的一个共同规范，这个规范就可以称为协议。
http协议即按一定规则向服务器要数据或发送数据，而服务器按一定规则回应数据。

- ftp
- http
- stmp
- pop
- tcp/ip

## 当你打开一个页面时，发生了什么？
![](./images/http.png)

## 浏览器发送一次http请求
![](./images/http-request-reponse.png)

## http协议的工作流程
1. 发送请求
2. 返回信息
3. 断开请求

**http请求信息和响应信息的格式**

- 请求
	+ 请求行
	+ 请求头信息
	+ 请求主体信息

- 请求行又分3部分
	+ 请求方法
	+ 请求路径 
	+ 请求所用协议及版本

- 请求方法又有多种
	+ GET
	+ POST
	+ PUT
	+ DELETE
	+ TRACE
	+ OPTIONS

## 使用telnet发送一次http请求
![](./images/telnet-request.png)


**浏览器可以发送http协议，那协议一定要浏览器发送吗？**
> 不是，http仅仅只是一种协议，什么工具都能发。



















