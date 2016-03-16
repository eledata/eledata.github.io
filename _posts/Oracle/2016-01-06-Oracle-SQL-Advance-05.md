---
layout: post
keywords: oracle date
description: Oracle Advance SQL Skill
title: "Advance SQL 系列之日期函数"
categories: Oracle
tags: Oracle
---

## Oracle 日期函数集合

### 日期集合

Oracle Date 函数： 

sysdate - 系统日期

last_day - 上一天

add_months - 可以往前或是往后加月份数

计算某个日期区间 - between to_char(sysdate - 10) and to_char(sysdate + 1)

	Select to_char(sysdate,'Q') from dual;--指定日期的季度
	Select to_char(sysdate,'MM') from dual;--月份
	Select to_char(sysdate,'WW') from dual;--当年第几周
	Select to_char(sysdate,'W') from dual ;--本月第几周
	Select to_char(sysdate,'DD') from dual;--当月第几天
	Select to_char(sysdate,'D') from dual;--周内第几天
	Select to_char(sysdate,'DY') from duaL;--星期几
	Select last_day(sysdate) from dual;--本月最后一天
	Select add_months(sysdate,2) from dual;--当前日期d后推n个月 
	select months_between(sysdate,to_date('2012-11-12','yyyy-mm-dd'))from dual;--日期f和s间相差月数
	SELECT (next_day(sysdate,1)+1) FROM dual;--指定的日期之后的第一个工作日的日期
	select to_char(add_months(last_day(sysdate),-1),'yyyy-MM-dd') LastDay from dual;--上月末天
	select to_char(add_months(sysdate,-1),'yyyy-MM-dd') PreToday from dual;--上月今天
	select to_char(add_months(last_day(sysdate)+1,-2),'yyyy-MM-dd') firstDay from dual;--上月第一天
	select to_char(sysdate,'ww') from dual group by to_char(sysdate,'ww');--按照每周进行统计
	select to_char(sysdate,'mm') from dual group by to_char(sysdate,'mm');--按照每月进行统计
	select to_char(sysdate,'q') from dual group by to_char(sysdate,'q');--按照每季度进行统计
	
	--找出当前月份的周五的日期
	select to_char(t.d, 'YY-MM-DD')
	  from (select trunc(sysdate, 'MM') + rownum - 1 as d
	          from dba_objects
	         where rownum < 32) t
	 where to_char(t.d, 'MM') = to_char(sysdate, 'MM')
	   and trim(to_char(t.d, 'Day')) = '星期五';

本篇完结！

参考文献

无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@