---
layout: post
title: Python 爬虫实战
date: 2017-03-01
tags: [Python, 爬虫, MP3下载, 文本过滤]
---

# Python 语法笔记
[Markdown 语法介绍 – CODING 帮助中心](https://coding.net/help/doc/project/markdown.html#i)

### python
- __Python中else的语法__
> 1. 本文讨论Python的for…else、while…else、try..except…else等语法
> 2. Python中的for、while是在循环体内没有break语句、没有return语句，或者没有异常出现时才执行else语句，try..except…else是在try中无异常产生时执行，注意不包括continue。
> 3. 总的来说，是for、while、try语句中‘没毛病’时才执行，而在if..else语句中是if语句判断为假（有毛病）才执行else，这一点要区分开来
> 4. Python3 语法表 [Python3 教程 \| 菜鸟教程](https://www.runoob.com/python3/python3-tutorial.html)

- __Python 正则表达式__
> [Python正则表达式指南 - AstralWind - 博客园](https://www.cnblogs.com/huxi/archive/2010/07/04/1771073.html)

- __urllib urllib2 urllib3 在 python2.X 和 python3.x使用__
> [python中urllib, urllib2,urllib3, httplib,httplib2, request的区别 - permike的专栏 - CSDN博客](https://blog.csdn.net/permike/article/details/52437492)

- __爬取分页网页数据__
> 第一种是分析js的实现来模拟翻页。
> 第二种比较简单暴力，也是我这里用的方法，那就是模拟点击网页中的下一页按钮，然后重新读取html代码来解析。
> [python利用beautifulsoup+selenium自动翻页抓取网页内容 - 为程序员服务](http://ju.outofmemory.cn/entry/304084)

- __Python+MySQL数据库开发__
>目前知道的两种方式 1）通过 PyMySQL 操作数据库 2）通过 MySQLdb 操作数据库
>[Python实现连接操作MySql数据库 - 让编程改变世界 - CSDN博客](https://blog.csdn.net/zhihua_w/article/details/54313258)
>[python——SQL基本使用 - Eva_J - 博客园](https://www.cnblogs.com/Eva-J/p/5133716.html)
>

- __爬虫开发感悟__
> _1.要对html的结构非常熟悉，至少知道常用标签，才好编写爬虫
> 2.对于动态网页，如果是ajax请求返回的数据，在python中可以直接调用 `get/post` 请求接口解析返回数据；其实解析静态HTML代码目的也是为了提取数据和调用API获取到接口数据是一样的。_