---
layout: post
title:  "Spring-Boot 实现热部署"
date:   2017-03-21 11:33:42
categories: 后端
tags: Spring-Boot 框架
excerpt: 使用Spring-Boot实现热部署（hot swapping）
---

* content
{:toc}

## 简要

*   Spring Boot在web开发中非常常用，但是有个很大的问题就是每次在修改完文件之后都需要重新启动来查看效果，因此需要实现Spring Boot的热部署(hot swapping)。本文主要介绍如何实现Spring Boot的热部署。

## POM文件

*   Spring Boot本身提供了实现热部署的设置，首先要在Maven的POM文件中添加以下内容(在官方文档中有)：

```java
    <!--热部署 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>   
    </dependency>
```

![Object](http://jiaohongwei.github.io/images/2017-03/20170322172936.png)


* maven插件配置

```java
    <!-- Package as an executable jar -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.jiaohongwei.Application</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

*  Eclipse注意：

>   针对eclipse的，eclipse --> Project --> Build Automatically要选中，不选中的话不起作用。

##  properties文件

*   在application.properties文件中要加上下面的代码：

```
    #Spring Boot Maven项目中的resource文件夹下的资源不进行缓存处理，即每次都需要去加载。
    #注：这只是开发的时候使用，项目实际运行时必须设为true，这样加载页面速度更快。
    spring.resources.chain.cache=false
```

##  参考文档

>   http://docs.spring.io/spring-boot/docs/1.5.2.RELEASE/reference/htmlsingle/#using-boot-hot-swapping

