---
layout: post
title: "XML in python"
description: "使用python解析XML"
category: python
tags: [python, xml, parse, parseString]
---
{% include JB/setup %}

研究python的XML相关模块，也是任务驱动的。背景是有个合作，需要将对方提供的xml格式的新闻，导入到数据库。
据说python有三种解析xml的方式，都有官方文档：
    [SAX](https://docs.python.org/2/library/xml.sax.html)
    [DOM](https://docs.python.org/2/library/xml.dom.minidom.html)
    [ElementTree](https://docs.python.org/2/library/xml.etree.elementtree.html)
三者的差异和优劣，暂时没有时间去研究，后续抽空补上。由于dom比较耳熟，所以不行被我选中，哈哈……

我的需求很简单，先来个xml的demo：

<pre class="code prettyprint linenums lang-html">
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;DOCUMENT&gt;  
    &lt;item&gt;
        &lt;key&gt;&lt;![CDATA[口水火锅]]&gt;&lt;/key&gt;
        &lt;!-- key字段是关键词字段， 当query匹配上关键词时，此专题将被展示，每个关键词只允许展示一个专题，所以相同关键词提交重复的item将会相互覆盖 --&gt; 
        &lt;display&gt;
            &lt;title&gt;&lt;![CDATA[口水火锅剩菜剩油回收]]&gt;&lt;/title&gt;
            &lt;!--标题文案，不能为空, title不要加站点名后缀,最长不超过16个汉字--&gt;
            &lt;url&gt;&lt;![CDATA[]]&gt;&lt;/url&gt;
            &lt;!--专题落地页地址，不能为空--&gt;
            &lt;poster_image&gt;&lt;![CDATA[]]&gt;&lt;/poster_image&gt;
            &lt;!--专题海报地址，尺寸不小于400*180，图片大小不得大于200kb--&gt;
            &lt;host&gt;&lt;![CDATA[手机人民网]]&gt;&lt;/host&gt;
            &lt;!--必填非空，显示站点名称，最多5个字 --&gt;
            &lt;img_news&gt;
                &lt;!--img_news是必须节点, 节点下必须包含1-3个img_new子节点，提供用户特别关注的焦点图片新闻--&gt;
                &lt;img_new&gt;
                    &lt;title&gt;&lt;![CDATA[口水火锅剩菜剩油回收]]&gt;&lt;/title&gt;
                    &lt;!--图片新闻标题文案，不能为空, title不要加站点名后缀,最长不超过16个汉字--&gt;
                    &lt;url&gt;&lt;![CDATA[http://m2.people.cn/r/MV80XzE3NzgwOTNfOTc2XzE0MjA2NzM2ODY=]]&gt;&lt;/url&gt;
                    &lt;!--图片新闻详情页地址，不能为空--&gt;
                    &lt;poster_image&gt;&lt;![CDATA[http://a1.peoplecdn.cn/dd3e69e054d960b146e45837bd0b4c79@1l]]&gt;&lt;/poster_image&gt;
                    &lt;!--图片新闻图片地址，尺寸不小于400*180，图片大小不得大于200kb，不能为空--&gt;
                &lt;/img_new&gt;
            &lt;/img_news&gt;
            &lt;news&gt;
                &lt;!--news是必须节点, 节点下必须包含2-3个new子节点，提供用户特别关注的焦点新闻或视频--&gt;
                &lt;new&gt;
                    &lt;type&gt;&lt;![CDATA[新闻]]&gt;&lt;/type&gt;
                    &lt;!--新闻类型，只能是：新闻或视频。--&gt;
                    &lt;title&gt;&lt;![CDATA[喷香火锅实为口水火锅]]&gt;&lt;/title&gt;
                    &lt;!--必填非空，title是新闻的标题 --&gt;
                    &lt;url&gt;&lt;![CDATA[http://m2.people.cn/r/MV80XzE3NzgwOTNfOTc2XzE0MjA2NzM2ODY=]]&gt;&lt;/url&gt;
                    &lt;!--必填非空，url是新闻的详情页 --&gt;
                    &lt;posttime&gt;&lt;![CDATA[2015-01-08 07:29]]&gt;&lt;/posttime&gt;
                    &lt;!--必填非空，posttime是新闻发布时间, 格式必须是YYYY-MM-DD HH:mm:ss --&gt;
                &lt;/new&gt;    
                &lt;new&gt;
                    &lt;type&gt;&lt;![CDATA[新闻]]&gt;&lt;/type&gt;
                    &lt;!--新闻类型，只能是：新闻或视频。--&gt;
                    &lt;title&gt;&lt;![CDATA[后厨员工都连说两个太脏了]]&gt;&lt;/title&gt;
                    &lt;!--必填非空，title是新闻的标题 --&gt;
                    &lt;url&gt;&lt;![CDATA[http://m2.people.cn/r/MV80XzE3NzgxNzlfOTc2XzE0MjA2NzM2ODY=]]&gt;&lt;/url&gt;
                    &lt;!--必填非空，url是新闻的详情页 --&gt;
                    &lt;posttime&gt;&lt;![CDATA[2015-01-08 07:31]]&gt;&lt;/posttime&gt;
                    &lt;!--必填非空，posttime是新闻发布时间, 格式必须是YYYY-MM-DD HH:mm:ss --&gt;
                &lt;/new&gt;    
                &lt;new&gt;
                    &lt;type&gt;&lt;![CDATA[新闻]]&gt;&lt;/type&gt;
                    &lt;!--新闻类型，只能是：新闻或视频。--&gt;
                    &lt;title&gt;&lt;![CDATA[记者卧底老码头 白汤淡奶调]]&gt;&lt;/title&gt;
                    &lt;!--必填非空，title是新闻的标题 --&gt;
                    &lt;url&gt;&lt;![CDATA[http://m2.people.cn/r/MV80XzE3NzgwOTNfOTc2XzE0MjA2NzM2ODY=]]&gt;&lt;/url&gt;
                    &lt;!--必填非空，url是新闻的详情页 --&gt;
                    &lt;posttime&gt;&lt;![CDATA[2015-01-08 07:32]]&gt;&lt;/posttime&gt;
                    &lt;!--必填非空，posttime是新闻发布时间, 格式必须是YYYY-MM-DD HH:mm:ss --&gt;
                &lt;/new&gt;
            &lt;/news&gt;
        &lt;/display&gt;  
    &lt;/item&gt;  
&lt;/DOCUMENT&gt;
</pre>


只需逐层解析，将CDATA部分内容提取出来即可，只需用到几个方法即可。

<pre class="code prettyprint linenums">
import xml.dom.minidom
dom = xml.dom.minidom.parseString(data)
dom = xml.dom.minidom.parse('~/path/demo.xml') #从文件获取

# 从root开始获取名字为name的对象list
items = dom.getElementsByTagName('name')
for item in items:
    sub_items = item.getElementsByTagName('sub_name')
</pre>

按上面的方式，依次递归解析即可。
最关键的是获取CDATA里的文本，如下边的代码所示：

<pre class="code prettyprint linenums">
    # 若CDATA的[]里包含换行、空格等空白字符，getElementsByTagName会被分解成多个节点
    # 取第一个节点
    text_item = item.getElementsByTagName('title')[0].firstChild.wholeText
</pre>
