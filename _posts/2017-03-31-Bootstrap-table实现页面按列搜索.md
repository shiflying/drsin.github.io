---
layout: post
title:  "Bootstrap-table实现按列搜索"
date:   2017-03-31 19:34:55
categories: 前端
tags: Bootstrap-Table
excerpt: Bootstrap-table表格数据按列搜索
---

* content
{:toc}

## 简要

*   当客户端分页显示数据时就经常用的一个全局搜索框，他会在所有列内里面筛选我们需要的信息，而有些时候数据量太大或者相似度高就用起来不太方便，这时就诞生了按列搜索的高级工具栏



## 下载使用

*   需要引入 [bootstrap-table-toolbar.js](https://github.com/JiaoHongwei/persion_jar) ，下载完成引入到原有的页面
```html
    <script src="${ctxStatic}/plugins/bootstrap-table/bootstrap-table-toolbar.js"></script>
```

*   在 `table` 中添加 属性

         data-advanced-search="true"
        
        data-id-table="advancedTable"

    ```html
        <table class="table table-primary mb30" id="table-osver-tab2" 
            data-advanced-search="true"
            data-id-table="advancedTable">
            <tbody></tbody>
        </table>
    ```


##  ok

*   实现以下效果

![Object](http://jiaohongwei.github.io/images/2017-03/20170331194730.png)

>   官方demo  http://issues.wenzhixin.net.cn/bootstrap-table/#extensions/toolbar.html 
