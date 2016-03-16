---
layout: post
keywords: page
description: Oracle Advance SQL Skill
title: "Advance SQL 系列之分页"
categories: Oracle
tags: Oracle
---

## Oracle 分页技术
### ROWNUM 分页
在Oracle中，分页实现主要通过ROWNUM来取得。通过嵌套SQL语句，进行双层的页面控制。文中的代码，给出了效率最高的版本和常用版本。

#### 效率最高版

	#分页代码
	SELECT * FROM
	(
	SELECT *
	FROM
	  (SELECT ROWNUM AS RN,
	    MATERIALID
	    ||'_'
	    ||PLANTID                           AS ID,
	    MATERIALID                          AS MATERIALID,
	    PLANTID                             AS PLANTID,
	    CEIL(SUM(PLNMG_PLANNEDQUANTITY)/13) AS FC_AVG13_WEEK_QTY
	  FROM INV_SAP_PP_FRCST_PBIM_PBED
	  WHERE (PDATU_DELIV_ORDFINISHDATE BETWEEN TO_CHAR(sysdate) AND TO_CHAR(sysdate + 91))
	  AND VERSBP_VERSION = '00'
	  AND PLANTID        = '5040'
	  GROUP BY MATERIALID,
	    ROWNUM,
	    PLANTID
	  )
	WHERE RN >= 20 #Start Page
	ORDER BY RN ASC
	) WHERE RN <= 40; #End Page

#### 效率一般，容易理解
	
	#分页代码
	SELECT *
	  FROM (SELECT ROWNUM AS rowno, t.*
	          FROM k_task t
	         WHERE flight_date BETWEEN TO_DATE ('20060501', 'yyyymmdd')
	                               AND TO_DATE ('20060731', 'yyyymmdd')) table_alias
	 WHERE table_alias.rowno <= 20 AND table_alias.rowno >= 10;

#### 利用With 语句来写
	
	#分页代码
	WITH partdata AS
	     (
	        SELECT ROWNUM AS rowno, tt.*
	          FROM (  SELECT *
	                    FROM k_task t
	                   WHERE flight_date BETWEEN TO_DATE ('20060501', 'yyyymmdd')
	                                         AND TO_DATE ('20060531', 'yyyymmdd')
	                ORDER BY fact_up_time, flight_no) tt
	         WHERE ROWNUM <= 20)
	SELECT *
	  FROM partdata
	 WHERE rowno >= 10;

本篇完结！

参考文献

1.http://blog.sina.com.cn/s/blog_8604ca230100vro9.html

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@

