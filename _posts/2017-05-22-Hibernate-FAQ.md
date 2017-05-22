---
layout: post
title: HFConsole收单系统问题
date: 2017-05-22 
categories: FAQ
---
* content
{:toc}

1.生成的*.hbm.xml无法被SaxReader解析解析

	在确保配置文件中属性名和POJO的类属性名一致和配置文件中的字段属性名与数据库字段一致的前提下

	检查Hirbernate的头文件中的dtd文件是否能访问（点击左键即可访问),如果无法访问，可以有两种原因：

(1)配置的xml 的key键不存在，可以自己添加。

	具体步骤：Window>>Preferencesl>>MyEclipse>>FilesandEditors>>XML>>XML_Catolog>>uses_specified_entries> add 

	location:本地下载的DTmD文件

	key:值为你*.hbm.xml 制定本地key。

	如："-//Hibernate/Hibernate Mapping DTD 3.0//EN"

(2) 无法访问外网
