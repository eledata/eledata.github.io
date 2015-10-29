---
layout: post
keywords: 聚类算法
description: 聚类算法
title: "数据挖掘--聚类算法之"
categories: 数据挖掘
tags: 数据挖掘
---

##聚类分析
###基本概念
聚类，将对象划分成组，在将这些对象指派到这些组中。



	#增加一个表空间文件，大小为20000M
	ALTER TABLESPACE SYSTEM ADD DATAFILE 'C:\***\SYSTEM03.DBF' SIZE 20000M;

###查询表空间大小
在Oracle中，表空间的信息都储存在`DBA_DATA_FILES`。我们可以通过此表来算出表空间文件的大小。


	#查询表空间大小
	SELECT TABLESPACE_NAME, ROUND(SUM(BYTES/(1024*1024)),0) AS TS_SIZE FROM DBA_DATA_FILES GROUP BY TABLESPACE_NAME;

	

