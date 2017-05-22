---
layout: post
title: "crete blog page"
date: 2017-05-01
categories: 工具
tags: jekyll
---
* content
{:toc}
--------
系统环境:ubuntu16.04 _X86_64
所需的应用程序: git,nodejs,ruby-dev,zliblg-dev

基础的环境可以参考jekyll 的官方网站 [jekyll](http://jekyllrb.com/docs/)

Jekyll Document

[download]({{ site.url }}/assets/pdf.Jekyll.pdf)
[jekyll plugins](http://jekyllrb.com/docs/plugins)


install application

 * apt-get install nodejs
 * apt-get install ruby-dev
 * apt-get install zliblg-dev

create dir 

 * mkdir myblog
 * cd myblog
 * git clone https://github.com/drsin/drsin.github.io.git

install env

 * ./script/bootstrap 

create sit 

 * ./script/cbuild

start server

 * ./script/server  
