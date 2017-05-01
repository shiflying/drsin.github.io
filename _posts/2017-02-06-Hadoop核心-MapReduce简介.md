---
layout: post
title:  "Hadoop核心-MapReduce简介"
date:  2017-02-06 13:09:12
categories:  大数据 
tags:  大数据 Hadoop
excerpt: 大数据-Hadoop
---

* content
{:toc}

##  简要

*   继续学习Hadoop的重要组成部分-MapReduce

*   分而治之，一个大任务分成多个小的子任务（map），并行执行后，合并结果（reduce）

##  基本概念

*   Job & Task

    1.  Job就是一个要执行的任务 ==> 提交Job
      
    2.  一个Job会被分为多个Task（MapTask和ReduceTask） ==> 分片  
       
*  JobTracker：（讲一个Job拆分成多个Map和Reduce任务；分配Map和Reduce任务）
    
    ==> 作业调度、分配任务、监控进度、监控TaskTracker状态

*  TaskTracker：（Map任务分发给下面的TaskTracker做实际 的任务；TaskTracker与DataNode保持对应关系）
                           
    ==> 执行任务、汇报任务状态     
    
![Object](http://jiaohongwei.github.io/images/2017-02/20170206131955.jpg)

*   MapReduce的容错机制:

     1.  重复执行 4次
     2.  推测执行
    