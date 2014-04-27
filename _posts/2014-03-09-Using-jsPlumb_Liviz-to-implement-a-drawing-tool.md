---
layout: post
title: "Using jsPlumb_Liviz to implement a drawing tool"
description: "使用jsPlumb和Liviz打造一个js绘图工具"
category: geek
tags: [js, jsPlumb, Liviz, Flowchart]
---
{% include JB/setup %}

这里说的绘图，并不是指趋势图、饼图、柱状图一类的数据统计图形（图表），而是类似流程图、状态图，工具的作用是图的绘制（节点添加、拖动、连线、删除等功能）。

#### jsPlumb&Liviz简介
[jsPlumb](http://jsplumbtoolkit.com/demo/home/jquery.html)提供了元素连接的功能，可以与jQuery、 MooTools或者YUI3协同工作。

[Liviz.js](http://ushiroad.com/jsviz/about/)是基于DHTML的自动布局工具，通过设置点和点间的关系（用配置表示一个有向图），利用该工具初始化图形。

有了jsPlumb，就可以实现节点和连线的创建、删除，通过与数据库交互，保存图的结构；有了Liviz.js，就可以通过配置初始化图形。能存能取，隐约看到了做出流程图工具的希望了。

[jsPlumb_Liviz.js](https://github.com/lndb/jsPlumb_Liviz.js)是将二者结合后的工具，可以在此基础上二次开发。

#### 如何使用&体验

* 上手还算容易，该做的都做好了，关键是要学会怎么用。
* 两个插件合并，有些臃肿，根据实际情况，可以将无用的删掉（包括自带的jquery）

`jsPlumb`需要理解几个概念和方法：

* EndPoint是用于连线的触点（注意与节点本身区分）
* jsPlumb.makeSource/jsPlumb.makeTarget将节点初始化为源点/终点，可以通过css类批量设置（`filter:".ep"`）。另外，该类方法可以设置节点、连线的样式和属性等。

<pre class="code prettyprint linenums">
var windows = jsPlumb.getSelector(".window");
jsPlumb.makeSource(windows, {
    filter:".ep",               // only supported by jquery
    anchor:"Continuous",
    connector:[ "StateMachine", { curviness:20 } ],
    connectorStyle:{ strokeStyle:"#5c96bc", lineWidth:2, outlineColor:"transparent", outlineWidth:4 },
});                     

// initialise all '.window' elements as connection targets.
jsPlumb.makeTarget(windows, {
    dropOptions:{ hoverClass:"dragHover", },
    // 此处限定每两个节点间只能有一条线
    beforeDrop:function(params) { 
        var conn1 = jsPlumb.select({source:params.sourceId, target:params.targetId});
        var conn2 = jsPlumb.select({source:params.targetId, target:params.sourceId});
        return 0 == (conn1.length + conn2.length);
    },
    anchor:"Continuous"             
});
</pre>

* jsPlumb.draggable(windows)设置一些列节点的“可拖动”属性
* jsPlumb.importDefaults设置默认属性，包括终端节点、连线的样式等。

<pre class="code prettyprint linenums">
jsPlumb.importDefaults({
    Endpoint : ["Dot", {radius:2}],
    HoverPaintStyle : {strokeStyle:"#1e8151", lineWidth:2 },
    ConnectionOverlays:[
        [ "Arrow", { 
            location:1
        } ],
        ["Label", {
            label: "X",
            location: 0.5
        }]
    ],
    DragOptions : { cursor: "pointer", zIndex:2000, containment:"#pipeline-tasks" }
});
</pre>

* jsPlumb.doWhileSuspended初始化需要，进行图形绘制、时间绑定等。
* jsPlumb.connect({source:tag,target:next_tag}); 连线
* var $window = $("#id"); jsPlumb.detachAllConnections($window); 删除某个节点的所有连线
* jsPlumb内置了一些列[事件](http://jsplumbtoolkit.com/doc/events)处理，常用事件有：
    * jsPlumb.bind("connection", function()); 连线事件，可在此与数据库进行交互，记录一个连线
    * jsPlumb.bind("connectionDetached", function()); 连线删除事件
    * jsPlumb.bind("beforeDetach", function(conn) {return confirm("删除连接？");}); 删除连线前触发，可以做些准备工作
    * jsPlumb.bind("click", function(c){jsPlumb.detach(c);}); 连线单击事件（detach用于删除连线）

`Liviz.js`的功能比较简单，可以参照[例子](http://lndb.info/light_novel/diagram/Akikan!)。需要注意的是，在初始化之前，需要在网页中写入节点以及需要被初始化的图的关系，如：
    
<pre class="code prettyprint linenums">
digraph chargraph {
    node[shape=box, margin=0, width=2, height=1]; 
    1 -> 2 -> 3;
    3 -> 4;
}
</pre>

节点间的而联系用;或者换行符分隔

#### 其他功能实现
jsPlumb_Liviz.js已经帮我们实现了节点拖动、连线、自动初始化布局等功能，如果这还不够的话，那么请看...
* 节点添加

设置一个按钮，绑定其click事件，符合条件后，在网页中插入一个节点（div），然后设置样式、绑定时间

<pre class="code prettyprint linenums">
$("#" + DIV_ID_CONTAINER).append("&lt;div id='" + TAG_PREFIX + task.id + "' taskId=" + task.id + " class='window'&gt;" + formProcName + "&lt;div class='ep'&gt;&lt;/div&gt;&lt;/div&gt;");
</pre>

* 节点删除

可以为节点创建右键菜单，加入删除选项

html代码：

<pre class="code prettyprint linenums">
&lt;div id="contextMenu" class="dropdown clearfix" style="z-index:10000"&gt;
  &lt;ul class="dropdown-menu" role="menu" aria-labelledby="dropdownMenu" style="display:block;position:static;margin-bottom:5px"&gt;
    &lt;li&gt;&lt;a op="delete-task" tabindex="-1" href="#"&gt;删除&lt;/a&gt;&lt;/li&gt;
  &lt;/ul&gt;
&lt;/div&gt;
</pre>

js代码：

<pre class="code prettyprint linenums">
    var $contextMenu = $("#contextMenu");
    var $taskSelected;

    $("body").on("contextmenu", ".window", function (e) {
        $taskSelected = $(this);
        $contextMenu.css({
            display: "block",
            left: e.pageX,
            top: e.pageY
        });
        return false;
    });
    $contextMenu.on("click", "a", function (e) {
        e.preventDefault();
        $contextMenu.hide();
        if('delete-task' == $(this).attr("op") &amp;&amp; confirm("删除该task?")) {
            var taskId = $taskSelected.attr('taskId');
            PipelineTasks.deleteTask(taskId);
        }
    });

    $(document).click(function () {
        $contextMenu.hide();
    });
</pre>

* 节点编辑

绑定节点的双击事件，然后爱干嘛干嘛...下面的例子是弹出一个编辑框：

<pre class="code prettyprint linenums">
$(".window").on('dblclick', function(e) {
    formType = 'update';    
    $.ajaxSetup({async : false});  
    $("#modal-task-form").modal({
        remote : url + "/update/" + $(e.target).attr('taskId'),
        keyboard: false,
        backdrop: 'static'
    });
});
</pre>
    



