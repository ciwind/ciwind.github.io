---
layout: post
title: "svn, skills"
description: "svn使用技巧"
category: skills
tags: [svn]
---
{% include JB/setup %}

####记录svn使用过程中碰到的问题及解决方案。

* 在脚本里自动获取svn数据，不需要输入密码

<pre class="code prettyprint linenums">
svn cat http://x.xx.com/somefile > localfile --username username --password password --no-auth-cache
</pre>