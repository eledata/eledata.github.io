---
layout: post
keywords: Insert
description: Oracle Advance SQL Skill
title: "Advance SQL 系列之批量Insert into"
categories: Oracle
tags: Oracle
---

# Oracle 批量处理Insert Into Sql代码

## 问题场景

由于物料团队的需求，需要定期更新物料的版本号。一次insert的sql语句条数在90万条左右，sql语句的生成通过神奇的excel加工。
![1](/public/img/posts/2016-03-07_insert_into1.jpg)

	#大概90万条左右
	INSERT INTO INV_SAP_MATLSE_SRCH VALUES ('0001-1126','#');
	INSERT INTO INV_SAP_MATLSE_SRCH VALUES ('0001-1144','');
	INSERT INTO INV_SAP_MATLSE_SRCH VALUES ('0002-9021','**');
	INSERT INTO INV_SAP_MATLSE_SRCH VALUES ('0002-9027','**');

###第一次尝试###

通过Sql Developer客户端，直接打开sql文件，接近50M的文件，打开巨慢，之后执行过程，崩溃，崩溃有没有。。。。。。


###第二次尝试###

利用sqlplus，直接使用@ file path.sql.惊喜不断，大概每秒1000条的insert速度，直接给好评了。原因是前台使用多线程来insert的，所以速度巨快。。
	
	sqlplus username/password@servicename @c:\yourfile.sql

本篇完结！

参考文献

1.http://polyangel.iteye.com/blog/1416586

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@