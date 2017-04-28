---
layout: post
title:  "Spring-Boot+Jsp demo"
date:   2016-11-18 16:25:11
categories: 后端
tags: Spring-Boot jsp 框架
excerpt: 使用Spring-Boot 整合jsp搭建项目
---

* content
{:toc}

## 简要

*   Spring-Boot 官方并不推荐使用jsp（好像是推荐用Thymeleaf），但是有些情况下的业务还是使用jsp更加合适，出于此我是阅历无数博客论坛，在经历千万次失败之后终于修得正果。
*   现在分享给大家，望后人少走弯路。

## 开始

*   使用maven快速搭建一个项目Spring-Boot-Jsp-demo，pom文件如下：

```java

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>Temp</groupId>
    <artifactId>Spring-Boot-Jsp-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.1.RELEASE</version>
    </parent>

    <description>Physical-fitness-test-center</description>
    <properties>
        <main.basedir>${basedir}/../..</main.basedir>
        <m2eclipse.wtp.contextRoot>/</m2eclipse.wtp.contextRoot>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--若不是使用intellij作为集成开发环境，则scope需要设置为provided <scope>provided</scope> -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <useSystemClassLoader>false</useSystemClassLoader>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

* 文件目录如下：

![Object](http://jiaohongwei.github.io/images/2016-11/20161118165131.png)

* 创建PageController

```java

package vg.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.Date;
import java.util.Map;

/**
 * Created by jiaohongwei on 2016/11/18.
 */
@Controller     //这里一定用@Controller  而不是@RestController !!!!
public class PageController {

    @Value("${application.message:Hello World}")
    private String message = "Hello World";

    @GetMapping("/")
    public String welcome(Map<String, Object> model) {
        model.put("time", new Date());
        model.put("message", this.message);
        return "welcome";
    }

    @RequestMapping("/foo")
    public String foo(Map<String, Object> model) {
        throw new RuntimeException("Foo");
    }
}

```

*  创建Application启动类

```java

package vg;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.support.SpringBootServletInitializer;

/**
 * Created by jiaohongwei on 2016/11/18.
 */
@SpringBootApplication
public class Application extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}

```

*   创建 application.yaml 配置文件

```

spring:
  mvc:
    view:
      suffix: .jsp
      prefix: /WEB-INF/jsp/
server:
  port: 8080

application.message:  Hello Phil

```


*   创建welcome.jsp文件

```java
<!DOCTYPE html>

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags"%>

<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<html lang="en">

<body>
<c:url value="/resources/text.txt" var="url"/>
<spring:url value="/resources/text.txt" htmlEscape="true" var="springUrl" />
Spring URL: ${springUrl} at ${time}
<br>
JSTL URL: ${url}
<br>
Message: ${message}
</body>
</html>


```


##  运行

*   至此准备完成，开始运行，注意了，不要从tomcat启动，
*   1.直接运行main方法；
*   2.敲命令 `mvn spring-boot:run` 运行；
*   接下来访问 http://localhost:8080/  便能看到结果了。

![Object](http://jiaohongwei.github.io/images/2016-11/20161118170458.png)

##  部署

*   使用 `mvn clean install` 命令打成war包
*   上传至服务器，通过 `java -jar xx.war` 启动服务

##  源码

贴上项目源码：[Spring-Boot-Jsp-demo](https://github.com/JiaoHongwei/Spring-Boot-Jsp-demo)



