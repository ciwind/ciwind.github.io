---
layout: post
title: Hello, GitHub Pages
category : others
tagline: "oo"
tags : [github, jekyll, jekyll-bootstrap]
---
{% include JB/setup %}
### GitHub Pages & Jekyll
其实，久闻GitHub Pages（以下简称GP）的大名，只是一直没去研究，觉得那不过就是一写blog的工具。直到前些日子，听hzyongsheng介绍，才知那确实就是一写blog的工具，但是，它可以很酷、很炫。

经过各种倒腾，今天终于用上这高大上的产品，总结一番，聊以慰藉之前受挫的心灵，和此时激动的心情。

GP最简单的用法如其[官网介绍](http://pages.github.com/)，用户可以自己写静态页面，自定义各种页面，托管在github上。

[Jekyll](http://jekyllrb.com/) 简单来说，是一个静态页面生成工具。支持Markdown，Liquid，HTML语法。

[Jekyll-bootstrap](http://jekyllbootstrap.com/) 可以理解为使用bootstrap样式来美化站点的Jekyll。

### 结合Jekyll使用GP
[Jekyll QuickStart](http://www.jekyllbootstrap.com/usage/jekyll-quick-start.html) 这篇教程通俗易懂，只要输入自己的仓库名称就可以定制安装Jekyll-bootstrap的代码。

在Install Jekyll-Bootstrap过程中，若碰到git push origin master错误，可以尝试使用

`https://github.com/USERNAME/USERNAME.github.com.git`

替换

`git@github.com:USERNAME/USERNAME.github.com.git`

#### Windows 7 环境下安装Jekyll
在本地安装jekyll可以很方便地进行调试，像写代码一样写文章，是不是很酷？

[Running Jekyll on Windows](http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html) 里写得很清楚，但是也有不少坑，需要注意：

* Ruby和Devkit的安装：二者的版本一定要对应，否则，你会在本关打无尽的小怪……
* Jekyll安装：需要进入Devkit的安装目录执行命令
* python版本：推荐2.7.5，不要3
* Pygments：这个需要[安装easy_install](https://pypi.python.org/pypi/setuptools#windows)先，完成后在C:\Python27\Scripts执行easy_install Pygments

完成以上几步，cd至本地repo目录，执行jekyll serve -w即可开启本地调试，在http://localhost:4000实时查看站点变化。

#### 可能碰到的问题
俗话说：行万里路，踩各种坑。搭环境过程问题着实不少，当geek耍酷真心不容易……[Running Jekyll on Windows](http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html)的Troubleshooting节已经列出大部分，我这边再啰嗦两句：

* jekyll serve执行出错，可能是1.4.3版本本身的问题，参考[Jekyll - Error Running 'Jekyll Serve'](http://stackoverflow.com/questions/21137096/jekyll-error-running-jekyll-serve)执行：
    
    `gem uninstall jekyll`

    `gem install jekyll --version "=1.4.2"`
* cannot load such file -- wdm (LoadError) 解决方案：
    
    `gem install wdm`
* 中文乱码问题 Liquid Exception: invalid byte sequence in GBK in 网上很多帖子的解决方案是改代码convertible.rb，个人觉得这么做很不雅，更合理的方法是：

    修改配置文件_config.yml，添加 `encoding: utf-8`
####代码高亮插件
  * 可以使用[google-code-prettify](https://code.google.com/p/google-code-prettify/)实现代码高亮。将代码下载到本地，挑选中意的[样式](http://google-code-prettify.googlecode.com/svn/trunk/styles/index.html)放在插件的skins目录下。
  * 在代码中引用js和css文件。

css

<pre class="code prettyprint linenums">
&lt;link href="{{ BASE_PATH }}/assets/themes/google-code-prettify/skins/sons-of-obsidian.css" rel="stylesheet" type="text/css" meida="all"/>;
&lt;link href='{{ BASE_PATH }}/assets/fonts/source-code-pro.css' rel='stylesheet' type='text/css'/>
&lt;link href='{{ BASE_PATH }}/assets/css/common/code.css' rel='stylesheet' type='text/css'/>
</pre>

js

<pre class="code prettyprint linenums">
&lt;script type="text/javascript" src="{{ BASE_PATH }}/assets/themes/google-code-prettify/prettify.js">&lt;/script>
&lt;script type="text/javascript">
  window.prettyPrint &amp;&amp; window.prettyPrint();
&lt;/script>
</pre>

  * 将代码包含在标签里：`<pre class="prettyprint linenums"></pre>`
  * 被包含的代码若包含`& <`需要用`&amp; &lt;`替换

#### 个人碰到的悬而未解的问题
* rake post title="中文" 导致出错









