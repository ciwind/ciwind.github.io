---
layout: post
title: "Python, make time range"
description: "构造一个时间序列"
category: 
tags: [python, time, datetime, range]
---
{% include JB/setup %}

下面的方法，实现了传入起始和终止时间，返回一个时间序列。

<pre class="code prettyprint linenums lang-php">
#!/usr/bin/env python
#encoding:utf-8

import sys 
import os
import datetime
import time

TIME_FORMAT_8 = '%Y%m%d'
TIME_FORMAT_10 = '%Y%m%d%H'
# 最大序列长度
MAX_RANGE_SIZE = 24

# 获取一个时间序列[start, end]
# start=20140410,end=20140412 return=[20140410,20140411,20140412]
# start=2014041000,end=2014041002 return=[2014041000,2014041001,2014041002]
def makeRangeTime(start, end):
    
    range_time = []
    if len(start) == len(end) and len(start) in (8, 10) and start &lt; end:
    
        time_format = TIME_FORMAT_8 if len(start) == 8 else TIME_FORMAT_10
        base_time = datetime.datetime.strptime(start, time_format)
    
        offset = 1 
        range_time = [start]
        while True:
            delta = datetime.timedelta(days = offset) if len(start) == 8 else datetime.timedelta(hours = offset)
            new_time = datetime.date.strftime(base_time + delta, time_format)
            offset += 1
            if new_time &lt; end:
                range_time.append(new_time)
                if len(range_time) > MAX_RANGE_SIZE:
                    raise Exception('%s-%s时间区间太大（要求小于%d）' % (start, end, MAX_RANGE_SIZE))
            else:
                break
        range_time.append(end)
    else:
        raise Exception('时间格式不对，要求等长的8位或12位时间格式，如20140428')
    
    return range_time

if __name__ == '__main__':
    start_end = ( 
        ('20140425', '20140428'),       #['20140425', '20140426', '20140427', '20140428']
        ('2014042812', '2014042815'),   #['2014042812', '2014042813', '2014042814', '2014042815']
        ('2014042812', '2014042915'),   #2014042812-2014042915时间区间太大（要求小于24）
        ('2014042812', '20140429')      #时间格式不对，要求等长的8位或12位时间格式，如20140428
    )   
    for start, end in start_end:
        try:
            print makeRangeTime(start, end)
        except Exception, ex: 
            print ex.message
            continue
</pre>
