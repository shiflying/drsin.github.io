---
layout: post
title:  "Bootstrap-Table 导出Excel"
date:  2017-01-03 10:03:37
categories:  前端 
tags:  Bootstrap 前端 Bootstrap-Table
excerpt: Bootstrap-Table导出Excel
---

* content
{:toc}

## 简要

*   简要介绍一下bootatrap-table的导出功能（纯前端操作）

## 概述

*   bootstrap-table中可以设置导出表格内容，需要引入相关的export.js文件
    
```html

<script src="${basePath}/static/js/bootstrap-table/bootstrap-table-export.js"></script>
<script src="${basePath}/static/js/bootstrap-table/tableExport/tableExport.js"></script>    

```

## 初始化


```js
$(function () {

    //1.初始化Table
    var oTable = new TableInitj();
    oTable.Init("/admin/sel/getxmdmb", "toolbar_xmdm");

    //2.初始化Button的点击事件
    var oButtonInit = new ButtonInit();
    oButtonInit.Init();

    //2.初始化select的change事件
    $("#sel_exportoption").change(function () {
        $('#table_xmdm').bootstrapTable('refreshOptions', {
            exportDataType: $(this).val()
        });

    });


});

var TableInitj = function () {
    var oTableInit = new Object();
    //初始化Table
    oTableInit.Init = function (uploadUrl, toolbar) {
        var toolbar = $('#' + toolbar);

        $("#table_xmdm").bootstrapTable({
                                            responsive: true,
                                            url: uploadUrl,         //请求后台的URL（*）
                                            method: 'get',                      //请求方式（*）
                                            toolbar: toolbar,                //工具按钮用哪个容器
                                            striped: true,                      //是否显示行间隔色
                                            cache: false,                       //是否使用缓存，默认为true，所以一般情况下需要设置一下这个属性（*）
                                            pagination: true,                   //是否显示分页（*）
                                            sortable: true,                     //是否启用排序
                                            // sortStable:true,                     //设置为 true
                                            // 将获得稳定的排序，我们会添加_position属性到 row 数据中。 sortOrder: "asc",
                                            //           //定义排序方式 'asc' 或者 'desc'
                                            queryParams: oTableInit.queryParams,//传递参数（*）
                                            sidePagination: "client",           //分页方式：client客户端分页，server服务端分页（*）
                                            pageNumber: 1,                       //初始化加载第一页，默认第一页
                                            pageSize: 10,                       //每页的记录行数（*）
                                            pageList: [10, 25, 50, 100],        //可供选择的每页的行数（*）
                                            search: true,                       //是否显示表格搜索，此搜索是客户端搜索，不会进服务端，所以，个人感觉意义不大
                                            strictSearch: false,                 // 	设置为 true启用
                                                                                 // 全匹配搜索，否则为模糊搜索
                                            showColumns: false,                  //是否显示所有的列
                                            showRefresh: false,                  //是否显示刷新按钮
                                            minimumCountColumns: 2,             //最少允许的列数
                                            clickToSelect: true,                //是否启用点击选中行
                                            // height: window.innerHeight - 300,
                                            // //行高，如果没有设置height属性，表格自动根据记录条数觉得表格高度
                                            uniqueId: "ID",                     //每一行的唯一标识，一般为主键列
                                            showToggle: false,                  //是否显示详细视图和列表视图的切换按钮
                                            cardView: false,                    //是否显示详细视图
                                            detailView: false,                   //是否显示父子表
                                            showExport: true,                     //是否显示导出
                                            exportDataType: "basic",              //basic', 'all', 'selected'.
                                            columns: [{
                                                checkbox: true
                                            },
                                                {
                                                    align: 'center',
                                                    field: 'xmdm',
                                                    title: '项目代码'

                                                }, {
                                                    field: 'xmmc',
                                                    title: '项目名称'
                                                }, {
                                                    field: 'jldw',
                                                    title: '计量单位'
                                                }
                                                , {
                                                    field: 'action',
                                                    title: '操作'
                                                }
                                            ]
                                        });
    };

    //得到查询的参数
    oTableInit.queryParams = function (params) {
        var temp = {   //这里的键的名字和控制器的变量名必须一直，这边改动，控制器也需要改成一样的
            limit: params.limit,   //页面大小
            offset: params.offset,  //页码
        };
        return temp;
    };
    return oTableInit;
};

var ButtonInit = function () {
    var oInit = new Object();
    var postdata = {};

    oInit.Init = function () {
        //初始化页面上面的按钮事件
    };

    return oInit;
};

```

##  页面设置 

```html
<div class="table-responsive">
    <div id="toolbar_xmdm" class="btn-group">
        <select id="sel_exportoption" class="form-control">
            <option value="">导出当前页面数据</option>
            <option value="all">导出全部数据</option>
            <option value="selected">导出选中数据</option>
        </select>
    </div>
    <table class="table table-primary mb30" id="table_xmdm">
        <tbody>


        </tbody>
    </table>

</div><!-- table-responsive -->

```
##  页面展现

![Object](http://jiaohongwei.github.io/images/2017-01/20170103095333.png)

![Object](http://jiaohongwei.github.io/images/2017-01/20170103095556.png)

##  注意

*   看初始化select按钮时的动作，是刷新bootstrap-table的Options，而不是直接导出，查了查好多文档都没有写这个导出是哪一个函数，官方文档也是这样定义了一个select选择类型，然后在选择导出。

*   Bootstrap-table-export demo 实例：http://issues.wenzhixin.net.cn/bootstrap-table/#extensions/export.html
*   Bootstrap-table 官方api文档：http://bootstrap-table.wenzhixin.net.cn/zh-cn/documentation/