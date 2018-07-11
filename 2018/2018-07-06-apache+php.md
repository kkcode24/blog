
# 整合apache+php（把php作为apache模块整合）

## 步骤

1. 解压PHP，并配置php.ini
	- 把PHP解压到某路径，设为x:/path/php
	- 修改或添加配置项： extension_dir="X:/path/php/ext"
	- 修改或添加配置项： date.timezone = PRC
2. 让apache引入php解释引擎
	- 修改apache的主配置文件`httpd.conf`
	- PHPIniDir "X:/path/php"
	- LoadModule php5_module "X:/path/php/php5apache2_2.dll"
	- LoadFile "X:/path/php/libeay32.dll"
	- LoadFile "X:/path/php/ssleay32.dll"
3. 告诉apache碰到`.php`结尾的就去找php模块先解释`AddType application/x-httpd-php.php`
4. 通过声明，让apache能够识别`.php`程序
	- 在AddTyp系列行附件，添加一行
	- `AddType application/x-httpd-php.php`
5. 重启apache测试效果
