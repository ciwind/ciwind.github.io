---
layout: post
title: "Matplotlib, Installation and Usage"
description: "matplotlib的安装和使用"
category: geek
tags: [python, matplotlib, installation]
---
{% include JB/setup %}
如果想使用python绘制图形，可以参考[13个Python图形库](http://django-china.cn/topic/75/)，稍微一比较，相信总有一款能满足需求。

[Matplotlib](http://matplotlib.org/)应该算其中最重量级2D图形库，支持[各种各样的图形](http://matplotlib.org/gallery.html)，只有你想不到，没有它画不了。并且，api、文档一应俱全，有心研究的话，完全可以利用它搞出个Matlab来（实际上matplotlib确实克隆了许多Matlab的函数。

### redhat下matplotlib安装
当然，想要用上这恐龙级的应用可没那么简单，准备工作得做足。其英文版[安装指南](http://matplotlib.org/users/installing.html)写得还是比较详尽的，按部就班的话，应该没多大问题，主要是除了python本身外，还要先安装一大堆依赖库。
* [numpy](http://www.numpy.org/)为python提供科学计算支持的基础库 
* [six](https://pypi.python.org/pypi/six/)用于兼容python2和3
* [libpng](http://www.libpng.org/pub/png/libpng.html)用于读取和保存png格式文件
* [freetype](http://www.freetype.org/)字体处理相关的库
* [setuptools](https://pypi.python.org/pypi/setuptools#id1)用于自动安装python模块
* [dateutil](http://labix.org/python-dateutil)python处理时间的一个扩展库
* [pyparsing](https://pypi.python.org/pypi)是一个用python编写的语法解析器模块

这些库的安装方法大同小异，以下两步即可（一台机器上有多个python情况下，需注意调用正确的python版本）：
1. 获取源代码，解压
2. 找到setup脚本，执行`python setup.py build && python setup.py install`

对于开源的东西，最难的最痛苦的莫过于安装环境，每一步都可能意想不到地出现问题。在不断的google并解决问题之后，你会发现这一切都是值得的，因为实在是太好用了。看[绘图实例源码](http://matplotlib.org/examples/index.html)，只怕你找不到，不怕它不提供，这文档太全了，来不及感动，我已经学会。

### 可能碰到的问题
* ImportError: libpng16.so.16

      解决方案：export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
