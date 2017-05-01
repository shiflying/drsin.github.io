---
layout: post
title:  "Maven-手动安装Oracle驱动"
date:   2016-12-01 21:34:11
categories: 后端
tags: Maven Oracle
excerpt: Maven-手动安装Oracle驱动
---

* content
{:toc}

## 简要

*  由于Oracle授权问题，Maven3不提供Oracle JDBC driver，为了在Maven项目中应用Oracle JDBC driver,必须手动添加到本地仓库。

## 获取Oracle JDBC Driver

*   通过Oracle官方网站下载相应版本：http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html
*   通过Oracle的安装目录获得，位置在“{ORACLE_HOME}\jdbc\lib\ojdbc6.jar”

## 安装

*   将ojdbc6.jar 文件放在主项目路径下

```

    mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.1.0 -Dpackaging=jar -Dfile=ojdbc6.jar

```
*   安装成功截图

![Object](http://jiaohongwei.github.io/images/2016-12/20161206204445.png)

##  添加引用

``` java
    <dependency>
    <groupId>com.oracle</groupId>
    <artifactId>ojdbc6</artifactId>
    <version>11.2.0.1.0</version>
    </dependency>
```