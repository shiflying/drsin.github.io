---
layout: post
title:  "java使用PDF模板生成PDF文件"
date:  2017-02-22 20:38:19
categories:  后端 前端
tags:  Spring-Boot PDF.js
excerpt: 在Spring-Boot中使用PDF模板生成对应的PDF文件并使用PDF.js展示在前端
---

* content
{:toc}

## 简要

*   前两天做了一个根据Word模板插入数据生成PDF文件传给前端展示的小功能，今晚有时间分享一下。

## 步骤

*   使用Adobe Acrobat X Pro将Wrld文档生成对应的 PDF form 模板文件。

>软件：Adobe Acrobat X Pro(带汉化和破解) //可以到我百度网盘中下载 [下载地址](https://pan.baidu.com/s/1dEFJQu9) 密码：eu3h
    
1. 打开 `Adobe Acrobat X Pro`

2. 选择`Create PDF Form or Online Form` （创建PDF表单)

3. 选择源：`Use an existing file`（PDF、Word、Excel或其它文件类型），下一步

4. 定位Word文件路径，下一步

5. Adobe Acrobat X Pro会自动猜测表单字段位置，如图

6. 一般自动生成的表单字段都不符合我们的要求，选中删除即可。建议删除自动生成的表单域，不然会出现无法显示中文内容等问题。

7. 点击右键选择`Text Field`文本框，拖动到适当的位置，设置好域名称，字号，字体（默认的字体是Helvetica,不支持中文）等。
    
    注意：此处有大坑！！在用模板生成pdf文档时，如果文本框字体没有设置成中文兼容的字体（宋体、黑体等），是一定不会显示中文的！！

8. 编辑完保存就好了（另外提醒一句，如果文本框太多，像我这样，建议修改完一点保存下，以防软件崩溃，我做的时候崩了五六次，想死的心都有了）
    
![Object](http://jiaohongwei.github.io/images/2017-02/20170222210010.png)
    

*   使用itext操作PDF模板，填充数据，生成PDF文件

>   需要jar包：itext.jar、iTextAsian-2.1.7.jar  用于生成中文汉字的字体包

>   `itext.jar` 可以通过maven在线获取
    
    
``` xml

<!-- https://mvnrepository.com/artifact/com.lowagie/itext -->
       <dependency>
           <groupId>com.lowagie</groupId>
           <artifactId>itext</artifactId>
           <version>2.1.7</version>
       </dependency>

```


>   `iTextAsian-2.1.7.jar` 可以去我的GitHub下载 [下载地址](https://github.com/JiaoHongwei/persion_jar)
    
1.  先把jar包导入到项目中

2.  Controller层方法
   
     ``` java
    
    @RequestMapping(value = "/djk", method = RequestMethod.GET)
     public void getDjk(HttpServletResponse response) throws Exception {
 
         response.setContentType("application/pdf");
         response.setHeader("content-disposition", "attachment; filename=test.pdf");
         String fileName = "upload/template.pdf"; // pdf模板
         PdfReader reader = new PdfReader(fileName);
         ByteArrayOutputStream bos = new ByteArrayOutputStream();
         PdfStamper ps = new PdfStamper(reader, bos);
              /* 使用中文字体 */
         BaseFont bf = BaseFont.createFont("STSong-Light", "UniGB-UCS2-H", BaseFont.NOT_EMBEDDED);
         ArrayList<BaseFont> fontList = new ArrayList<BaseFont>();
         fontList.add(bf);
         AcroFields fields = ps.getAcroFields();
 
         fillData(fields, data());
            //一定设置为true，不然生成的pdf还可以编辑
         ps.setFormFlattening(true);
         ps.close();
         bos.writeTo(response.getOutputStream());
         reader.close();
         bos.close();
         return;
 
     }
     
       private void fillData(AcroFields fields, Map<String, String> data) throws IOException, DocumentException {
             for (String key : data.keySet()) {
                 String value = data.get(key);
                 fields.setField(key, value);
             }
         }
     
         private Map<String, String> data() throws Exception {
            //在这里给pdf模板添加数据，注意key 就是pdf模板中的文本框的name，对应起来就ok
             Map<String,String> data=new HashMap<>();
             data.put("fill_1","新数据");
             data.put("fill_2","新数据2");
             return data;
         }
 
     ```
   
3.  后端工作完成，开始准备前端。先google一下`PDF.js`这个小插件。

4.  这里还需要谢谢之前的网友，忘记在哪里看的了->尴尬，不需要下载完整的`PDF.js`只需要最后编译完的目录就可以

>   可以直接去我GitHub下载编译后的`generic`文件夹      [下载链接](https://github.com/JiaoHongwei/pdf.js)

5.  直接copy `generic`文件夹 到你的webapp目录下面，在浏览器访问 http://localhost:8080/generic/web/viewer.html 就能看到初始化的pdf.js页面了

6.  在前端页面显示后端传过来的pdf文件流，新建一个<a>标签
    
    ```html
    
    <a target="_blank" href="${basePath}/generic/web/viewer.html?file=/student/djk"></a>

    ```

>   解释一下  `${basePath}` 是我自定义的 项目端口（=localhost:8080）

>   `/generic/web/viewer.html`后面不加参数就是默认的pdf页面，加上参数（对应controller的访问路径） `file=/student/djk` 就是显示你后台传过来的pdf文件，很简单吧。
      
7.  至此就全部搞定了，看一下我做的pdf页面：
    

![Object](http://jiaohongwei.github.io/images/2017-02/20170222223421.png)

    

##  小结

*   总结一下，自己找jar包浪费了好多时间，其实开发很简单。

*   pdf模板太难做了，需要处理的文本框一百多个加上软件还不停地崩溃，弄了一上午……

