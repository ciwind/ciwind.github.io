---
layout: post
title: "Yii, export csv file"
description: "csv文件导出"
category: 
tags: [yii, csv]
---
{% include JB/setup %}

在yii中使用以下代码，简单地实现导出cvs文件功能

<pre class="code prettyprint linenums lang-php">
class CsvExport {
    // $export_data: array(array(col1, col2, ...), array(col1, col2, ...), ...)
    public static function export($export_data)
    {
        $fp = fopen('php://temp', 'w');
        fputs($fp, $bom =( chr(0xEF) . chr(0xBB) . chr(0xBF) ));
        foreach($export_data as $data)
            fputcsv($fp, $data);
        rewind($fp);

        Yii::app()->user->setState('export',stream_get_contents($fp));
        fclose($fp);
        Yii::app()->request->sendFile('export.csv',Yii::app()->user->getState('export'));
        Yii::app()->user->clearState('export');
    }  
}
</pre>

其中`fputs($fp, $bom =( chr(0xEF) . chr(0xBB) . chr(0xBF) ));`写入文件BOM头，用来解决windows下查看中文乱码问题的。
