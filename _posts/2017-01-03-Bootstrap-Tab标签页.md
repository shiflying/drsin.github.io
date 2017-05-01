---
layout: post
title:  "Bootstrap-Tab 标签页"
date:  2017-01-03 10:34:16
categories:  前端 
tags:  Bootstrap 前端 标签页
excerpt: Bootstrap-Tab的几个使用场景
---

* content
{:toc}

## 简要

*   具体的使用方法自己查看api文档就能搞定，很简单的，先在主要介绍一下关于tab的几个点击事件，在开发中常用到的
*   实际开发中可能会有多个tab页面，但是不可能一下子全部加载出来，否则速度太慢不说也不好更新内容，我们则可以通过触发tab时初始化tab中的数据


## 事件

1. ``show.bs.tab``

*   介绍：

*       该事件在标签页显示时触发，但是必须在新标签页被显示之前。分别使用 event.target 和 event.relatedTarget 来定位到激活的标签页和前一个激活的标签页。
   
*   实例：

        ```js
        
        $('a[data-toggle="tab"]').on('show.bs.tab', function (e) {
          e.target // 激活的标签页
          e.relatedTarget // 前一个激活的标签页
        })
        
        ```


2. ``shown.bs.tab``

*   介绍：

*       该事件在标签页显示时触发，但是必须在某个标签页已经显示之后。分别使用 event.target 和 event.relatedTarget 来定位到激活的标签页和前一个激活的标签页。
   
*   实例：

        ```js
   
        $('a[data-toggle="tab"]').on('shown.bs.tab', function (e) {
          e.target // 激活的标签页
          e.relatedTarget // 前一个激活的标签页
        })
   
        ```

## 本地实测

*   就是在激活标签页后 初始化tab中的表格及其数据


```js

$('a[data-toggle="tab"]').on('shown.bs.tab', function (e) {
    e.target // 激活的标签页
    e.relatedTarget // 前一个激活的标签页
    var currentTab = $(e.target).text(); //获取当前标签页的标题

    if (currentTab == '项目代码管理'){
        //1.初始化Table
        var oTable = new TableInitj();
        oTable.Init("/admin/sel/getxmdmb", "toolbar_xmdm");

        //2.初始化select的change事件
        $("#sel_exportoption").change(function () {
            $('#table_xmdm').bootstrapTable('refreshOptions', {
                exportDataType: $(this).val()
            });

        });
    }

})

```


##  资料文档

*   Bootstrap-tab标签 官方api文档：http://www.tutorialspoint.com/bootstrap/bootstrap_tab_plugin.htm