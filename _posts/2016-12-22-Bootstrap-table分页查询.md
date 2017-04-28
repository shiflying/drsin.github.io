---
layout: post
title:  "Bootstrap-Table 分页查询"
date:  2016-12-22 16:20:53
categories:  前端 
tags:  Bootstrap 前端 Bootstrap-Table
excerpt: Bootstrap-Table的分页查询实例
---

* content
{:toc}

## 简要

*   本篇主要介绍在项目中用到的Bootstrap-Table插件的两种分页方式，即服务器端分页和客户端分页。

## 概述



## 实例

一、  服务端分页

*   设置如下属性 
``` java
    sidePagination: "server",   //服务端分页
```
*   必须包含：total节点（总记录数），rows节点（分页后数据） 

    即返回json数据格式为：
     
```
{“total”:24  , ”rows”:[…]}
```

```js
'#stu-table').bootstrapTable({
                                           responsive: true,
                                           url: '/admin/getStu/'+nj+"/"+xy,         //请求后台的URL（*）
                                           method: 'get',                      //请求方式（*）
                                           toolbar: '#toolbar',                //工具按钮用哪个容器
                                           striped: true,                      //是否显示行间隔色
                                           cache: false,                       //是否使用缓存，默认为true，所以一般情况下需要设置一下这个属性（*）
                                           pagination: true,                   //是否显示分页（*）
                                           sortable: true,                     //是否启用排序
                                           sortStable:true,                     //设置为 true 将获得稳定的排序，我们会添加_position属性到 row 数据中。
                                           sortOrder: "asc",                   //定义排序方式 'asc' 或者 'desc'
                                           queryParams: oTableInit.queryParams,//传递参数（*）
                                           sidePagination: "server",           //分页方式：client客户端分页，server服务端分页（*）
                                           pageNumber: 1,                       //初始化加载第一页，默认第一页
                                           pageSize: 10,                       //每页的记录行数（*）
                                           pageList: [10, 25, 50, 100],        //可供选择的每页的行数（*）
                                           search: true,                       //是否显示表格搜索，此搜索是客户端搜索，不会进服务端，所以，个人感觉意义不大
                                           strictSearch: false,                 // 	设置为 true启用 全匹配搜索，否则为模糊搜索
                                           showColumns: true,                  //是否显示所有的列
                                           showRefresh: true,                  //是否显示刷新按钮
                                           minimumCountColumns: 2,             //最少允许的列数
                                           clickToSelect: true,                //是否启用点击选中行
                                           singleSelect:true,
                                           checkboxHeader:true,
                                           height: window.innerHeight-300,
                                           // //行高，如果没有设置height属性，表格自动根据记录条数觉得表格高度
                                           uniqueId: "ID",                     //每一行的唯一标识，一般为主键列
                                           showToggle: true,                  //是否显示详细视图和列表视图的切换按钮
                                           cardView: false,                    //是否显示详细视图
                                           detailView: false,                   //是否显示父子表
                                           showExport: true,                     //是否显示导出
                                           exportDataType: "all",   //basic', 'all',
                                           // 'selected'.
                                           rowStyle: function (row, index) {
                                               //这里有5个取值代表5中颜色['active', 'success', 'info',
                                               // 'warning', 'danger'];
                                               var strclass = "";
                                               if (row.pcStatus == "RUNNING") {
                                                   strclass = 'success';//还有一个active
                                               }
                                               else if (row.pcStatus == "FINISH") {
                                                   strclass = 'danger';
                                               }
                                               else {
                                                   return {};
                                               }
                                               return {classes: strclass}
                                           },
                                           columns: [{
                                               checkbox: true
                                           }
                                           , {
                                               field: 'NJDM',
                                               title: '年级编号'
                                           },
                                               {
                                                   field: 'BJH',
                                                   title: '班号'
                                               }, {
                                                   field: 'BM',
                                                   title: '班级'
                                               }, {
                                                   field: 'XH',
                                                   title: '学籍号'
                                               },
                                               {
                                                   field: 'MZDM',
                                                   title: '民族代码'
                                               },
                                               {
                                                   field: 'XM',
                                                   title: '姓名'
                                               },
                                               {
                                                   field: 'XB',
                                                   title: '性别'
                                               },
                                               {
                                                   field: 'CSRQ',
                                                   title: '出生日期'
                                               },
                                               {
                                                   field: 'KQH',
                                                   title: '学生来源'
                                               },
                                               {
                                                   field: 'SFZH',
                                                   title: '身份证号'
                                               },
                                               {
                                                   field: 'TXDZ',
                                                   title: '家庭住址'
                                               }
                                           ]
                                       });
    };
```

二、客户端分页
*   设置如下属性 
``` java
    sidePagination: "client",   //客户端分页
```
*   当数据量较少，只有上千条数据时，一次性将所有数据返回给客户端，无论点下一页，或搜索条件时，不向服务端发请求，实现全文检索。
*   服务器返回json数据必须包含data节点（展示数据）

*   即返回的数据格式：

```
    [{'key'：value,},{'key':'value'}]
```
