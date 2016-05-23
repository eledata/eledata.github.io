---
layout: post
keywords: oracle date
description: Oracle Advance SQL Skill
title: "Advance SQL 系列之取销售数量前x%的订单"
categories: Oracle
tags: Oracle
---

## Oracle 取销售数量前x%的订单

### Step 1： 计算出每个型号当前销售的数量

在计算销售数量前x%的订单之前，我们需要知道，我们每个型号当前的销售数量如何？我们可以利用SUM()函数来统计，然后ORDER BY 从大到小来排列。

### Step 2： 利用ROWNUM来如前x%行，完成x%订单的任务

ROWNUM：根据sql查询出的结果给每行分配一个逻辑编号。从1开始的序列。利用ROWNUM的特性，我们可以在排序之后，完成对销售数量前x%的查询。

接下来，用代码来解释：
	
	SELECT * FROM
	(
	SELECT MATERIAL,PLANT, SUM_OPEN_QTY
	FROM(
	SELECT 
	MATERIAL,PLANT,
	SUM(OPEN_QTY) AS SUM_OPEN_QTY --求和，计算数量
	FROM(
	SELECT MATERIAL,PLANT,
	  NVL(OPEN_QTY,0) AS OPEN_QTY
	FROM INV_SAP_SALES_VBAK_VBAP_VBUP
	)
	WHERE MATERIAL IN
	  (SELECT DISTINCT MATERIAL FROM INV_SAP_SALES_VBAK_VBAP_VBUP
	  )
	GROUP BY MATERIAL,PLANT
	)ORDER BY SUM_OPEN_QTY DESC --升序排列
	)WHERE ROWNUM <= ROUND(0.01 * (SELECT COUNT(*) FROM INV_SAP_SALES_VBAK_VBAP_VBUP));	--ROWNUM取值情况

查询结果：

	MATERIAL        PLANT	SUM_OPEN_QTY
	MRO-MATERIAL	4080	322580.5
	1492-J3 A	1090	36900
	1492-J4 A	1090	36600
	1492-EAJ35 A	1090	21800
	PN-202462	4030	20000
	PN-290899	1090	18000
	PN-30362	1080	16800
	D57818-01 A	1090	16260
	800F-ALP A	4000	13310
	100-K09ZQ10M A	4000	13300


本篇完结！

参考文献

无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@