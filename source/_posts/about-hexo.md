---
layout: 
title: Hexo Next 搭建博客相关
date: 2021-08-23 11:25:13
tags:
- Hexo
- Next
categories:
- [学习, coding]
---

记录学习Hexo和Next搭建个人博客中的一些问题
<!-- more -->
> <center> 要提倡谦虚、学习和坚忍的精神。 —— 毛主席</center>
### 1 Hexo与Next的安装
&emsp;&emsp;此部分有很多教程，包括 [Hexo官方文档](https://hexo.io/zh-cn/docs/index.html)和[Next官方文档](https://theme-next.iissnan.com/getting-started.html)，完成安装并将其部署至github.io中即可。
### 2 玩转Next
&emsp;&emsp;此部分主要介绍如何丰富Next的设置。
#### 2.1 主题设置
&emsp;&emsp;在部署blog的文件夹内找到 `\themes\next\_config.yml` 文件，在其中找到 `# Schemes` 主题，选择下面四种主题取消一种注释即可（这里选择的是第二个mist主题）。
```
# Schemes
#scheme: Muse
scheme: Mist
#scheme: Pisces
#scheme: Gemini

# Dark Mode
darkmode: false
```
#### 2.2 个人栏(sidebar)设置
&emsp;&emsp;首先简单设置一下个人信息，在根目录下的 `_config.yml` 文件中更改一些基本信息，包括博客名称与描述等
```
# Site
title: wanderland
subtitle: ''
description: 谦虚 学习 坚忍
keywords:
author: Shi
language: zh-CN
timezone: ''
```
&emsp;&emsp;接着可以设置个人栏中的背景，文件路径为：
`themes\next\source\css\_schemes\Muse\_sidebar.styl`，在siber的style中添加需要设置的背景图位置和加载方式

```
.sidebar {
  background:url(/images/crane4.jpg);	
  background-size: cover;
  background-position:center;
  background-repeat:no-repeat;
  width: $sidebar-desktop;
  z-index: $zindex-2;
  the-transition-ease-out();
  ......
  }
```
图片存放位置是 `themes\next\source\image\` .

### 3 撰写博文
&emsp;&emsp;博客撰写使用的是Markdown，编辑器为Typora，最后存放位置为`\source\_post`。Markdown的语法也较为简单，可以掌握简单的分级标题、列表以及代码样式即可。Markdown的语法查询也有对应的 [链接](https://markdown.com.cn/basic-syntax/).
&emsp;&emsp;对于博文来讲，可以设置tags和categories用来区分和查找，一般是在博文的头部插入layout，例如本篇博文
```
---
layout: 
title: Hexo Next 搭建博客相关
date: 2021-08-23 11:25:13
tags:
- Hexo
- Next
categories:
- [学习, coding]
---
```
&emsp;&emsp;其中tags的设置是相互独立的，而categories的设置是有层级的，由上到下依次细化，而一篇博文同时也可以归属于多个分类，此时通过上面展示的中括号将分类层级以此键入。

### TODO LIST
*关于前端或者blog相关*
- 继续学习和深入了解next相关
- 关于撰写博客的细节
- 学习git相关操作，主要是协作方面
- 学习js，继续网站开发
