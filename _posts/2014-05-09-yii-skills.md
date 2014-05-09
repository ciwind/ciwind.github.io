---
layout: post
title: "Yii, skills"
description: "Yii框架使用技巧"
category: skills
tags: [yii, skill]
---
{% include JB/setup %}

####记录Yii使用过程中碰到的问题及解决方案。

* 下拉列表支持输入、过滤

使用控件bootstrap-combobox.js，在代码中使用：

<pre class="code prettyprint linenums">
    Yii::app()->clientScript->registerScriptFile(Yii::app()->baseUrl . "/js/bootstrap-combobox.js");                                       
    Yii::app()->clientScript->registerCssFile(Yii::app()->baseUrl . "/css/bootstrap-combobox.css");
    echo $form->dropDownList(
        $model, 
        $key, 
        $values,
        array(                                                                                                                
            'class'=>'combobox',             
        )
    );
    &lt;script>
       $('.combobox').combobox();
    &lt;/script>
</pre>

* radioButtonList横排

<pre class="code prettyprint linenums">
    CHtml::radioButtonList(
        'name', 
        'value1', 
        array(
            'radio1'=>'value1', 
            'radio1'=>'value1'
        ), 
        array(
            'template'=>'   {input}&lt;li style="display:inline-block;">{label}&lt;/li>', 
            'separator'=>' ', 
        )
    );
</pre>

* 设置和使用cache

在config/main.php的components中加入以下代码即可开启：

<pre class="code prettyprint linenums">
    'cache'=>array(
        // 还有别的cache类型，如CMemCache
        'class'=>'system.caching.CFileCache',
    ),
</pre>

通过`Yii::app()->cache`使用，具体参考[Data caching](http://www.yiiframework.com/doc/guide/1.1/en/caching.data)