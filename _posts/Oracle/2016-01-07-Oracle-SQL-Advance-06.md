---
layout: post
keywords: oracle date
description: Oracle Advance SQL Skill
title: "Advance SQL 系列之ROWNUM与ROWID区别"
categories: Oracle
tags: Oracle
---

## Oracle ROWNUM与ROWID区别

### ROWNUM

根据sql查询出的结果给每行分配一个逻辑编号，所以你的sql不同也就会导致最终rownum不同。因此，这就导致了他们的使用场景不同了，通常在sql分页时或是查找某一范围内的记录时，我们会使用rownum。

Sample 1： 查找2到10范围内的记录（这里包括2和10的记录）

	select *
	  from (select rownum rn, a.* from emp a) t
	where t.rn between 2 and 10;

Sample 2：查找前三名的记录

这里我们要注意，直接用rownum查找的范围必须要包含1；因为rownum是从1开始记录的，当然你可以把rownum查出来后放在一个虚表中作为这个虚表的字段再根据条件查询。

	select *
	  from (select rownum rn, a.* from emp a) t
	where t.rn > 2;

### ROWID

物理结构上的，在每条记录insert到数据库中时，都会有一个唯一的物理记录 （不会变），例如  AAAMgzAAEAAAAAgAAB 7499 ALLEN SALESMAN 7698 1981/2/20 1600.00 300.00 30 这里的AAAMgzAAEAAAAAgAAB物理位置对应了这条记录，这个记录是不会随着sql的改变而改变。

		DECLARE
		  CURSOR cur
		  IS
			SELECT a.CATALOG_STRING1,
			  b.ROWID ROW_ID
			FROM INV_SAP_MATERIAL_CATALOG a,
			  INV_SAP_NODASH_MAT_CATA b
			WHERE a.MATERIALID = b.MATERIALID
			ORDER BY b.ROWID; ---order by rowid
		  V_COUNTER NUMBER;
		BEGIN
		  V_COUNTER := 0;
		  FOR row IN cur
		  LOOP
			UPDATE INV_SAP_NODASH_MAT_CATA SET CATALOG_STRING2 = REPLACE(row.CATALOG_STRING1, '-') WHERE ROWID = row.ROW_ID;
			V_COUNTER     := V_COUNTER + 1;
			IF (V_COUNTER >= 1000) THEN
			  COMMIT;
			  V_COUNTER := 0;
			END IF;
		  END LOOP;
		  COMMIT;
		END;

本篇完结！

参考文献

无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@