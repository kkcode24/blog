# 在window上安装Apache

## 下载Apache

进入[Apache官网](http://apache.org/)
1. 在菜单中选择download
2. 进入Apache的镜像站
3. 点击 "archive site" 进入Apache老版本存档站
4. 选择httpd目录
5. 选择binaries目录
6. 选择win32目录
7. 选择一个Apache版本进行下载，文件后缀为 .msi

![下载Apache操作指导](https://i.imgur.com/3yWgZ1p.png)

进入http://httpd.apache.org/去下载最新版本，具体操作不演示。 

## 安装Apache

- 安装步骤如下图所示：

![apache安装步骤](https://i.imgur.com/He376Bp.png)

- 如何知道是否安装成功？
> 在浏览器中输入 localhost
> 如果出现下图所示的情况表示成功。

![apache安装成功](https://i.imgur.com/1VKDiBq.png)

## Apache虚拟主机配置

我把apache安装在了F盘，所以打开 F:\Apache2.2\conf

![](https://i.imgur.com/t9JujNX.png)


1. 打开httpd.conf文件
2. 引入Include conf/extra/httpd-vhosts.conf配置文件，也就是把#号去掉

![](https://i.imgur.com/aJABNnh.png)



3.再打开F:\Apache2.2\conf/extra/httpd-vhosts.conf文件

配置如下：复制一份虚拟机的配置，进行修改
![](https://i.imgur.com/jmTRvND.png)

4.找到hosts文件
`C:\Windows\System32\drivers\etc`

配置
```
# 引导b.com到127.0.0.1上
127.0.0.1   b.com
```

5.在`F:\Apache2.2\htdocs`下添加b.com文件夹

在b.com文件夹下新建index.html

![](https://i.imgur.com/lQDFXis.png)

6.重启apache，在浏览器中访问b.com

![](https://i.imgur.com/Ztnqvov.png)
