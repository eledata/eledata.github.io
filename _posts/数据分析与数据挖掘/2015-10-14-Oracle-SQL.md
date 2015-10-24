---
layout: post
keywords: SQL
description: Collective Intelligence
title: "集体编程智慧--集体智慧导言"
categories: ML&DM
tags: ML&DM
---

##表空间管理
###增加表空间
在数据增长量不大的情况下，我们不大会碰到表空间不够用的情况。但是，在很多场景，数据的体量是不断的增长的。会不断的侵蚀我们分配的表空间，此时会出现
表空间不够用的情况，那么就需要我们增加表空间的大小。


	#增加一个表空间文件，大小为20000M
	ALTER TABLESPACE SYSTEM ADD DATAFILE 'C:\***\SYSTEM03.DBF' SIZE 20000M;

###查询表空间大小
在Oracle中，表空间的信息都储存在`DBA_DATA_FILES`。我们可以通过此表来算出表空间文件的大小。


	#查询表空间大小
	SELECT TABLESPACE_NAME, ROUND(SUM(BYTES/(1024*1024)),0) AS TS_SIZE FROM DBA_DATA_FILES GROUP BY TABLESPACE_NAME;

	

