---
layout: post
keywords: SQL
description: Oracle Advance SQL Skill
title: "Advance SQL 子查询WITH AS"
categories: Oracle
tags: Oracle
---

### 子查询WITH AS
使用WITH AS 语句可以为一个子查询语句块定义一个名称，使用这个子查询名称可以在查询语句的很多地方引用这个子查询。Oracle 数据库像对待内联视图或临时表一样对待被引用的子查询名称，从而起到一定的优化作用。


	#增加一个表空间文件，大小为20000M
	SELECT ID,
	  MAX_DIS_NAME,
	  MAX_DIS_USAGE_QTY,
	  TOT_USAGE_QTY,
	  ROUND(PERSENTAGE,4) AS PER_BIGGEST,
	  DISTRIBUTOR_COUNT
	FROM
	  (SELECT ID,
	    MAX_DIS_USAGE_QTY,
	    MAX_DIS_NAME,
	    TOT_USAGE_QTY,
	    PERSENTAGE,
	    DISTRIBUTOR_COUNT,
	    ROWID
	  FROM INV_SAP_DSTR_STATS
	  WHERE (ID,ROWID) IN
	    (SELECT ID,MIN(ROWID) FROM INV_SAP_DSTR_STATS GROUP BY ID
	    )
	  );
	
	-- 使用了WITH AS语句， 将查询的语句变成可复用的子语句。
	WITH A AS
	  (SELECT ID,
	    MAX_DIS_USAGE_QTY,
	    MAX_DIS_NAME,
	    TOT_USAGE_QTY,
	    PERSENTAGE,
	    DISTRIBUTOR_COUNT,
	    ROWID
	  FROM INV_SAP_DSTR_STATS
	  ),
	  B AS
	  (SELECT ID, MIN(ROWID) FROM INV_SAP_DSTR_STATS GROUP BY ID
	  )
	SELECT A.ID,
	  A.MAX_DIS_USAGE_QTY,
	  A.MAX_DIS_NAME,
	  A.TOT_USAGE_QTY,
	  A.PERSENTAGE,
	  A.DISTRIBUTOR_COUNT,
	  A.ROWID
	FROM A
	WHERE (A.ID,
	  A.ROWID) IN
	  (SELECT * FROM B
	  )

本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
