---
layout: post
title:  "Jekyll 添加 FancyBox 图片插件"
date:   2017-03-26 16:49:50
categories: 架构设计与开源技术
tags: GitHub 插件
excerpt: 添加FancyBox插件,实现博客中图片点击放大预览功能
---

* content
{:toc}

## 简要

*   今天终于解决了我的博客中一个大问题，插入的图片由于过大无法完整展示，导致压缩不清晰现象，严重影响博客质量。

*   Markdown 语法中的图片我们一般是如此写法

```
    ![tag](url "name")
```
    
*   生成的 html 为

```html
    <image title="name" alt="name" src="url">
```

*   我们如果要使用（譬如）FancyBox 的话，则需要如下的链接才可

```html
   <a href="url" id="id">
     <img src="url">
   </a>
```
    
## 下载使用

*   首先下载 [FancyBox](https://github.com/fancyapps/fancyBox/zipball/v2.1.5) ，解压到你的主题文件夹，譬如我的是 `theme/fancybox` 下，

*   编辑`_includes`文件夹中模板文件，在`head.html`中添加

```html
   <link href="/theme/fancybox/source/jquery.fancybox.css?v=2.1.5" rel="stylesheet" media="all" />
```

*   在 `footer.html` 中添加

```html
    <script src="/theme/fancybox/lib/jquery-1.10.1.min.js"></script>
    <script type="text/javascript" src="/theme/fancybox/source/jquery.fancybox.pack.js?v=2.1.5"></script>
    <script>
        // 给图片添加链接
        $(document).ready(function() {
            $("p img").each(function() {
                var strA = "<a id='yourid' href='" + this.src + "'></a>";
                $(this).wrapAll(strA);
            });
        });
    
        // fancybox
        $("#yourid").fancybox({
                                  openEffect    : 'elastic',
                                  closeEffect   : 'elastic',
                              });
    </script>
```


##  ok

*   将代码提交上去，重新打开你的博客，点击图片，就会出现以下效果

![Object](http://jiaohongwei.github.io/images/2017-03/20170326171028.png)

>   简直6到没朋友了，再也不愁写博客时图片被压缩显示的现象了，给FancyBox一个大写的赞！

>   `FancyBox` 具体用法 http://fancyapps.com/fancybox/