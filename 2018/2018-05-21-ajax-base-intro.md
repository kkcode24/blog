## AJAX基础
```html
<form>
    用户名:
    <input type="text" id="username" value="" /> 
    密码:
    <input type="password" id="password" value="" />
    <br />
    <br />
    <input type="submit" id="update" name="提交" />
</form>
```

```javascript
var update = document.getElementById("update");
update.onclick = function () {
    var username = document.getElementById("username").value;
    var pass = document.getElementById("password").value;
    //ajax操作的创建
    //步骤1:创建Ajax对象
    if (window.XMLHttpRequest) {
        var ajax = new XMLHttpRequest();//在主流浏览器下创建Ajax对象
    } else {
        var ajax = new ActiveXObject("Microsoft.xmlhttp");//在IE浏览器下创建Ajax对象
    }
    //开启ajax
    /**************get方式***********/
    var url = "http://localhost/ajax/ajax01/dealDate.php?username=" + username + "&password=" + pass;
    ajax.open("GET", url, true);
    // 发送数据
    ajax.send();
    /******post方式*******/
    // ajax.open("POST","dealDate.php",true);
    // //请求过程中数据的编码格式(POST专用操作)
    // ajax.setRequestHeader("Content-type","application/x-www-form-urlencoded");
    // var parameter = "username="+username+"&password="+pass;
    // ajax.send(parameter);
    
    //等待接收数据
    /*Ajax对象在执行过程中伴随着状态的切换,共存有5中状态的切换
    0.代表Ajax对象的创建,但是未调用open方法
    1.代表Ajax对象调用open方法,但是未调用send方法
    2.代表Ajax对象调用send方法,但是还没有接收到数据
    3.代表Ajax对象正在接收数据
    4.代表Ajax对象接收数据完成*/
    ajax.onreadystatechange = function () {
        //当状态码为4的时候,代表数据接收完毕
        if (ajax.readyState == 4) {
            if (ajax.status >= 200 && ajax.status < 300 || ajax.status == 304) {
                //输出服务器返回的数据,但是该数据必须是通过echo输出的文本数据
                var p = document.createElement("p");
                p.innerHTML = ajax.responseText;
                document.body.appendChild(p);
            }
        }
    }

}
```

```php
<?php
    header("Content-type:text/html;charset=utf-8");
    $user = $_GET["username"];
    $password = $_GET["password"];
    echo "{$user}".":"."{$password}";
?>
```

## 利用Ajax上传包含图片的数据到sql数据库

```html
<style type="text/css">
    #photo {
        width: 300px;
        height: auto;
        display: none;
    }
</style>
<form action="" method="post" enctype="multipart/form-data">
    真实姓名:
    <input type="text" id="username" value="" />
    <hr /> 身份证号:
    <input type="text" id="id" value="" />
    <hr /> 证件照片:
    <input type="file" id="file" value="" />
    <br />
    <img src="" alt="" id="photo" name="img" />
    <hr />
    <input type="submit" value="验证" id="check" />
</form>
```

```javascript
<script type="text/javascript" src="jquery-2.2.4.min.js"></script>
<script type="text/javascript">
    var path;//定义变量接收图片路径
    var fileM = document.getElementById("file");
    $("#file").on("change", function () {
        //创建formData对象,内置的对象
        var formData = new FormData();
        //files:存储的是文件对象,数据类型是一个数组
        formData.append("file", fileM.files[0]);
        //创建ajax对象
        if (window.XMLHttpRequest) {
            var ajax = new XMLHttpRequest();
        }else {
            var ajax = new ActiveXObject("Microsoft.xmlhttp");
        }
        //开始设置
        ajax.open("POST", "file_up_load.php", true);
        //传输数据
        ajax.send(formData);
        ajax.onreadystatechange = function () {
            if (ajax.readyState == 4) {
                if (ajax.status >= 200 && ajax.status < 300 || ajax.status == 304) {
                    path = ajax.responseText;
                    $("#photo").css("display","block");
                    $("#photo").attr("src", ajax.responseText);
                }
            }
        }
    });
    $("#check").on("click", function () {
        var name = document.getElementById("username").value;
        var id = document.getElementById("id").value;
        $.ajax({
            type: "post",
            url: "http://localhost/ajax/ajax01/fromAndAjax.php",
            data: "username=" + name + "&id=" + id + "&imgpath=" + path
        });
    })
</script>
```

```php
// file_up_load.php  处理图片文件的php代码:
<?php
$files = $_FILES["file"];
//判断当前上传的文件类型
$types = array("jpg","png","jpeg");
$type = $files["type"];
$imgTypes = explode("/",$type);

if(in_array($imgTypes[1],$types)){
    $des = "img/".time().".".$imgTypes[1];
    $res = move_uploaded_file($files["tmp_name"],$des);//将图片移动到指定的路径下
    if ($res) {
    // echo pathinfo($_SERVER["REQUEST_URI"])["dirname"]."/".$des;
    echo "http://10.90.93.195:8020/ajax/ajax01/".$des;
    }
}
?>
```

```php
// fromAndAjax.php 创建数据库的php代码:
header("Content-type:text/html;charset=utf-8");//显示中文
$name = $_POST["username"];
$idcard = $_POST["id"];
$imgpath = $_POST["imgpath"];

//创建mysqli对象
$mysqli = new mysqli("localhost","root","","H7-44");
//创建表格
$sql = "create table if not exists message(id int unsigned auto_increment primary key,name varchar(50),idcard bigint(18),imgpath varchar(2000))";
$res = $mysqli->query($sql);//执行
$sql = "insert into message(name,idcard,imgpath)value('$name','$idcard','$imgpath')";
$res = $mysqli->query($sql);
```

## ajax-json

```html
<input type="button" name="" id="data" value="获取数据" />
<script type="text/javascript" src="jquery-2.2.4.min.js"></script>
<script type="text/javascript">
    $("#data").on("click", function () {
        //使用ajax进行数据请求
        $.ajax({
            type: "get",
            url: "http://localhost/ajax/ajax02/getData.php",
            success: function (txt) {
                console.log("txt:" + txt);
                var data = JSON.parse(txt);
                console.log("data:" + data);
                for (var i = 0; i < data.length; i++) {
                    console.log(data[i].name);
                }
            }
        });
    });
</script>
```

```php
//JSON数据的生成
$stu = array(array("name"=>"小菜","sex"=>"男","age"=>30,"sno"=>"10110","classroom"=>"H7班"),array("name"=>"老司机","sex"=>"男","age"=>21,"sno"=>"10111","classroom"=>"H7班"));
//JSONP的JSON数据格式
// $stu = '[{"name":"小菜","sex":"男","age":"30","sno":"10110","classroom":"H7班"},{"name":"老司机","sex":"男","age":"21","sno":"10111","classroom":"H7班"}]';
// echo "{$fun}({$stu})";
$json = json_encode($stu);//将数据转化成json串(json数据本质上是一个字符串)
echo  $json;
```


## JSONP

```html
<input type="button" name="" id="data" value="获取数据" />

<script type="text/javascript" src="jquery-2.2.4.min.js"></script>
<script type="text/javascript">
    /*jsonp:解决网页跨域访问其他服务器数据的问题 解决跨域问题的时候需要借助具有src属性的标签进行数据请求,json只能通过get请求进行
    json:是一种数据传输的格式,但是jsonp则是解决json数据跨域传输的一种手段
    jsonp实现跨域请求数据前提是对应的php文件支8持跨域操作
    */
    //回调方法用来获取跨域的json数据
    function getData(txt) {
        // var data = JSON.parse(txt);//使用JSONP的时候针对传入的数据 默认进行了json解析
        for (var i = 0; i < txt.length; i++) {
            console.log(txt[i].name);
        }
    }
    $("#data").on("click", function () {
        //创建script标签
        var script = $("<script src='http://localhost/ajax/ajax02/JSONP.php?fun=getData'><\/script>");
        //设置script标签的src属性
        $("body").append(script);
    });
</script>
```

```php
// JSONP.php

//获取对应的参数值
$fun = $_GET["fun"];
//JSON数据的生成
$stu = array(array("name"=>"小菜","sex"=>"男","age"=>30,"sno"=>"10110","classroom"=>"H7班"),array("name"=>"老司机","sex"=>"男","age"=>21,"sno"=>"10111","classroom"=>"H7班"));
//JSONP的JSON数据格式
// $stu = '[{"name":"小菜","sex":"男","age":"30","sno":"10110","classroom":"H7班"},{"name":"老司机","sex":"男","age":"21","sno":"10111","classroom":"H7班"}]';
// echo "{$fun}({$stu})";
$json = json_encode($stu);//将数据转化成json串(json数据本质上是一个字符串)
echo "{$fun}({$json})"; 
```

## 封装AJAX

```html
用户名:
<input type="text" id="username" value="" />
<br />
<br /> 密码:
<input type="text" id="password" value="" />
<br />
<br />
<input type="submit" id="click" value="提交" />
<script type="text/javascript" src="ajax.js"></script>
<script type="text/javascript" src="jquery-2.2.4.min.js"></script>
<script type="text/javascript">
    var btn = document.getElementById("click");
    btn.onclick = function () {
        var user = document.getElementById("username").value;
        var psd = document.getElementById("password").value;
        //进行Ajax请求
        ajax({
            method: "POST",
            url: "dealDate.php",
            data: {
                username: user,
                password: psd
            },
            success: function (test) {
                alert(test);
            }
        });
        /*******JQ的方法****/
        // $.ajax({
        // type:"post",
        // url:"http://localhost/ajax/ajax01/dealDate.php",
        // data:"username="+user+"&pass="+psd,
        // success:function(txt){
        // alert(txt);
        // }
        // });
    }
</script>
```

```javascript
// ajax.js
/*obj:对应的是一个对象,存储当前进行ajax请求时对应的信息*/
var ajax = function (obj) {
    var method = obj["method"];
    if (!method) {
        method = "GET";//当外界没有指定请求方式,按照默认的方式
    } var asy = obj.asy;//判断当前的请求是不是异步请求 
    if (asy == undefined) {
        asy = true;//默认异步执行 
    }
    //创建ajax对象 
    if (window.XMLHttpRequest) {
        var ajaxR = new XMLHttpRequest();
    } else {
        var ajxaR = new ActiveXObject("Microsoft.xmlhttp");
    } //开启ajax 
    var url = obj.url;//获取网址
    var data = obj.data;//获取参数数据 
    var array = []; //将用户的数据转换成key=value的形式 
    for (var key in data) {
        var value = data[key];
        var str = key + "=" + value;
        array.push(str);
    } //将每一个键值对转换成用&连接的字符串 
    data = array.join("&");
    if (method == "GET") {
        url = url + "?" + data;
        ajaxR.open(method, url, asy);
        ajxaR.send();
    } else {
        ajaxR.open(method, url, asy); //设置参数的编码格式 
        ajaxR.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
        ajaxR.send(data);
    } ajaxR.onreadystatechange = function () {
        if (ajaxR.readyState == 4) {
            if (ajaxR.status >= 200 && ajaxR.status
                < 300 || ajaxR.status == 304) {
                if (obj.success) {
                    obj.success(ajaxR.responseText);
                }
            }
        }
    }
}
```

```php
$user = $_POST["username"];
$password = $_POST["password"];
echo "{$user}".":"."{$password}";
```