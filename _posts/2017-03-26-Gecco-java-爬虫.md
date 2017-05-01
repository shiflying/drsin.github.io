---
layout: post
title:  "使用Gecco爬取博客（一）"
date:   2017-03-26 15:12:50
categories: 架构设计与开源技术
tags: Gecco 爬虫
excerpt: Gecco简单使用之爬取博客列表
---

* content
{:toc}

## 简要

*   Gecco是一款用java语言开发的轻量化的易用的网络爬虫，不同于Nutch这样的面向搜索引擎的通用爬虫，Gecco是面向主题的爬虫。

*   官网：     http://www.geccocrawler.com/

    

## 开发

*   新建一个实体`MyGitHubArticle.java`类来存储所要爬取的文章列表

```java
   package com.jiaohongwei.demo.myGitHub;
   
   import com.geccocrawler.gecco.GeccoEngine;
   import com.geccocrawler.gecco.annotation.Gecco;
   import com.geccocrawler.gecco.annotation.HtmlField;
   import com.geccocrawler.gecco.annotation.Text;
   import com.geccocrawler.gecco.spider.HtmlBean;
   
   import java.util.List;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   /**
    * Created by jiaohongwei on 2017/3/22.
    */
@Data       // 使用lombok 消除 Java 代码的冗长。
@lombok.Setter  //==所有属性的的get方法
@lombok.Getter  //== set方法
@NoArgsConstructor  //==无参构造
@AllArgsConstructor     //==全参构造
   @Gecco(matchUrl = "http://blog.jiaohongwei.cn/archive/", pipelines = "myGitHubPipeline")
   public class MyGitHubArticle implements HtmlBean {
   
       /**
        * 此demo 用来爬取我的GitHub博客列表，地址： http://blog.jiaohongwei.cn/archive/
        */
       private static final long serialVersionUID = -7127412585200687225L;
   
       //文章集合
       @Text
       @HtmlField(cssPath = "body > div.page.clearfix > div.left > ul > li")
       private List<Blog> blogs;
   
       public static void main(String[] args) {
           GeccoEngine.create()
                   .classpath("com.jiaohongwei.demo.myGitHub")
                   .start("http://blog.jiaohongwei.cn/archive/")
                   .thread(1)
                   .interval(2000)
                   .debug(false)
                   .start();
       }
   }

```

*   存储博客信息的实体类`Blog.java`

```java
  package com.jiaohongwei.demo.myGitHub;
  
  import com.geccocrawler.gecco.annotation.HtmlField;
  import com.geccocrawler.gecco.annotation.Text;
  import com.geccocrawler.gecco.spider.HtmlBean;
  
  import java.util.List;
  
  import lombok.AllArgsConstructor;
  import lombok.Data;
  import lombok.NoArgsConstructor;
  
  /**
   * Created by jiaohongwei on 2017/3/22.
   */
  @Data
  @lombok.Setter
  @lombok.Getter
  @NoArgsConstructor
  @AllArgsConstructor
  public class Blog implements HtmlBean {
  
      private static final long serialVersionUID = -7127412585200687225L;
  
  
      //时间
      @Text
      @HtmlField(cssPath = "li > time")
      private String time;
  
      //标题
      @Text
      @HtmlField(cssPath = "li > a")
      private String title;
  
      //分类
      @Text
      @HtmlField(cssPath = "li > span.categories > a")
      private List<String> categories;
  
      //标签
      @Text
      @HtmlField(cssPath = "li> span.pageTag > a")
      private  List<String> tags;
  
      @Override
      public String toString() {
          return "Blog{" +
                  "time='" + time + '\'' +
                  ", title='" + title + '\'' +
                  ", categories=" + categories +
                  ", tags=" + tags +
                  '}';
      }
  
  }

```


>   接口HtmlBean说明该爬虫是一个解析html页面的爬虫（gecco还支持json格式的解析）

>   注解@Gecco告知该爬虫匹配的url格式(matchUrl)和内容抽取后的bean处理类（pipelines处理类采用管道过滤器模式，可以定义多个处理类）。

>    注解@RequestParameter可以注入url中的请求参数，如@RequestParameter("user")表示匹配url中的{user}

>    注解@HtmlField表示抽取html中的元素，cssPath采用类似jquery的css selector选取元素

>    注解@Text表示获取@HtmlField抽取出来的元素的text内容

>    注解@Html表示获取@HtmlField抽取出来的元素的html内容（如果不指定默认为@Html）

>    GeccoEngine表示爬虫引擎，通过create()初始化，通过start()/run()运行。可以配置一些启动参数如：扫描@Gecco注解的包名classpath；开始抓取的url地址star；抓取线程数thread；抓取完一个页面后的间隔时间interval(ms)等
    gecco


*  业务逻辑处理类`MyGitHubPipeline.java`

>   gecco采用管道过滤器模式灵活的实现业务逻辑处理，首先实现一个特定的管道过滤器，


```java
    package com.jiaohongwei.demo.myGitHub;
    
    import com.geccocrawler.gecco.annotation.PipelineName;
    import com.geccocrawler.gecco.pipeline.Pipeline;
    
    import java.util.List;
    
    /**
     * Created by jiaohongwei on 2017/3/22.
     */
    @PipelineName("myGitHubPipeline")
    public class MyGitHubPipeline implements Pipeline<MyGitHubArticle> {
    
        /**
         * gecco采用管道过滤器模式灵活的实现业务逻辑处理，首先实现一个特定的管道过滤器，
         * @param myGitHubArticle
         */
        @Override
        public void process(MyGitHubArticle myGitHubArticle) {
            //TODO  可以持久化到数据库或文件
            List<Blog> list = myGitHubArticle.getBlogs();
            for (Blog blog : list) {
                System.out.println(blog.toString());
            }
    
        }
    }

```

##  运行

*   运行`MyGitHubArticle.java`类中的main方法

![Object](http://jiaohongwei.github.io/images/2017-03/20170326153732.png)

>   这只是一个最简单的小demo，后续有时间会继续把和spring 结合业务的demo放上来

##  源码下载

>   https://github.com/JiaoHongwei/Gecco-demo


