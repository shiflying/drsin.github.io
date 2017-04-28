---
layout: post
title:  "Hadoop核心-HDFS简介"
date:  2017-02-06 12:24:02
categories:  大数据  
tags:  大数据 Hadoop
excerpt: 大数据-Hadoop
---

* content
{:toc}

##  简要

*   继续学习Hadoop的重要组成部分-HDFS（文件系统）

##  基本概念

*   块（Block）

    1.HDFS的文件被分成块进行存储，HDFS块的默认大小64MB；
    
    2.块是文件存储处理的逻辑单元；
    
    注：默认大小是可以在hdfs-site.xml中手动配置的
    
    ```
    <property>
      <name>dfs.block.size</name>
      <value>67108864</value>
      <description>The default block size for new files.</description>
    </property>
    ```

*   NameNode

    1.NameNode是管理节点，存放文件元数据
        
        ①   文件与数据块的映射表
        
        ②   数据块与数据节点的映射表
                
*   DataNode
    
    1.DataNode是用来存放数据块

 ![Object](http://jiaohongwei.github.io/images/2017-02/20170206124144.jpg)
   
##  HDFS数据管理

*   数据可靠：每个数据存3个副本，分布在两个机架内的三个节点

*   心跳检测： DataNode定期向NameNode发送心跳信息

*   Secondary NameNode：二级NameNode定期同步元数据映像文件和修改日志，NameNode发生故障时，备胎转正。

##  HDFS读写

*   读取文件

 ![Object](http://jiaohongwei.github.io/images/2017-02/20170206125109.jpg)

*   写文件
        
        1.客户端将文件拆分成块
        2.客户端通知NameNode,NameNode返回可用的DataNode节点地址
        3.客户端根据返回的DataNode将数据块写入到可用的DataNode中
        4.因为数据块要有三份，所以会通过一个复制管道将每个数据块复制出另外两份并保存
        5.更新元数据NameNode

 ![Object](http://jiaohongwei.github.io/images/2017-02/20170206125405.jpg)

##  HDFS特点

*   数据冗余，硬件容错
*   流式的数据访问（一次存储多次访问）
*   存储大文件

适用性和局限性

- 适合数据批量读写，吞吐量高
- 不适合交互式应用，低延迟很难满足

- 适合一次写入多次读取，顺序读写
- 不支持对用户并发写相同的文件


##  HDFS使用

*   它提供了 shell 接口，可以进行命令行操作

```shell
    hadoop namenode -format	#格式化namenode
    hadoop fs -ls / #打印 / 目录文件列表
    hadoop fs -mkdir input #创建目录 input
    hadoop fs -put hadoop-env.sh input/ #上传文件 hadoop-env.sh 到 input 目录下
    hadoop fs -get input/abc.sh hadoop-envcomp.sh #从 input 目录中下载文件
    hadoop fs -cat input/hadoop-env.sh #查看文件 input/hadoop-env.sh 
    hadoop dfsadmin -report #dfs报告(所有DNFS信息)
```
