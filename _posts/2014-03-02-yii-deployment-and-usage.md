---
layout: post
title: "Yii, Deployment and Usage"
description: "RedHat环境下，yii框架的部署和使用"
category: yii
tags: [php, yii]
---
{% include JB/setup %}
### 基础环境
首先当然是[LAMP](http://baike.baidu.com/subview/365086/10014223.htm)的安装了，参考[在Redhat安装部署Apache+MySQL+PH](http://my.oschina.net/alanlqc/blog/145449)，在RedHat下，利用[yum](http://baike.baidu.com/view/157053.htm)，总结为以下几步：

* apache服务，安装成功后，在浏览器输入ip即可看到默认页面。

`yum install httpd`

`/etc/init.d/httpd start`

* mysql数据库

`yum install mysql mysql-server`

`/etc/init.d/mysqld start`

* php5，需要重启httpd服务使之生效，此后，apache就可以执行php脚本了。

`yum install php`

`/etc/init.d/httpd restart`

可以在网站根目录/var/www/html/建立测试脚本info.php，内容为：

`<?php echo phpinfo(); ?>`

访问ip/info.php即可看到结果。

* php的mysql模块：php-mysql

`yum install php-mysql`

`/etc/init.d/httpd restart`

### Yii安装&配置
安装很简单，[下载源代码](http://www.yiiframework.com/download/)后解压到/var/www/html/下即可。
参考[建立第一个 Yii 应用](http://www.yiichina.com/guide/quickstart.first-app)，执行以下命令，创建一个web应用：

`cd WebRoot`

`php YiiRoot/framework/yiic.php webapp testdrive`

之后，保证testdrive目录下的assets和runtime的写权限，即可访问`http://hostname/testdrive/index.php`到自己的网站。

#### php配置
* 开启[魔法函数](http://baike.baidu.com/view/5234458.htm)： magic_quotes_gpc = On
* 如果不是https协议传输，则：session.cookie_secure = 0
* 激活了URL形式的fopen封装协议，访问URL对象例如文件：allow_url_fopen = On
* 关闭notice警告：error_reporting = E_ALL & ~E_NOTICE
* 启用exec函数（其他函数类似）：disable_functions选项中去掉exec
      
#### apache配置
* url改写，去掉index.php：
  
  取消http.conf中LoadModule rewrite_module modules/mod_rewrite.so注释，并确保<Directory></Directory>中有"AllowOverride All"

  在网站根目录添加.htaccess文件

#`未完待续……`


