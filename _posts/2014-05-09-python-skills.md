---
layout: post
title: "python, skills"
description: "python使用技巧"
category: skills
tags: [python]
---
{% include JB/setup %}

####记录python使用过程中碰到的问题及解决方案。

* python sql字段值转义

<pre class="code prettyprint linenums">
def addslashes(s):
    d = {'"':'\\"', "'":"\\'", "\0":"\\\0", "\\":"\\\\"}
    return ''.join(d.get(c, c) for c in s)
</pre>