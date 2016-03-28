---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之CountIf"
categories: DATA
tags: DATA
---
## CountIf 函数

### 简介

许多EXCEL问题都涉及数据计数，对于一些简单的计数，通常使用COUNT函数或COUNTA函数就可以解决。然而，在实际的业务处理当中，往往存在诸多条件的限制，仅仅使用简单的统计函数是无法满足人们的实际需求的，例如多条件计数、统计不重复个数等。

例如，在一个EXCEL表格中，D列是考生的数学考试成绩，我们想统计数学成绩及格的人数。可以使用“=COUNTIF(D:D,">=60")”来实现。

COUNTIF函数可以对区域中满足单个条件的单元格区域进行计数。

#### 语法如下：

COUNTIF(参数一，参数二)，其中参数一为需要计算其中满足条件的单元格数目的单元格区域，参数二是用于定义将对哪些单元格进行计数的数字、表达式、单元格引用或文本字符串。

#### COUNTIF函数的常见用法如下：（假如数据所在单元格区域命名为“ Data ”）

	=COUNTIF(Data,"=") '返回真空单元格个数（真空单元格是指什么都没有的单元格）
	=COUNTIF(Data,"") '返回真空+假真空单元格个数（假真空是指0字符的空文本） 
	=COUNTIF(Data,"<>")'返回非真空单元格个数 
	=COUNTIF(Data,"*") '返回文本型单元格个数
	=COUNTIF(Data,"<9.99E+307")'返回数值型单元格个数
	=COUNTIF(Data,"<>""") '返回区域内所有单元格个数
	=COUNTIF(Data,"<0") '返回偶包含负值的单元格个数
	=COUNTIF(Data,"<>0") '返回真不等于零的单元格个个数
	=COUNTIF(Data,60) '返回值等于60的单元格个数
	=COUNTIF(Data,">60") '返回值大于60的单元格个数
	=COUNTIF(Data,"<60") '返回值小于60的单元格个数
	=COUNTIF(Data,">=60") '返回值大于等于60的单元格个数
	=COUNTIF(Data,"<=60") '返回值小于等于60的单元格个数
	=COUNTIF(Data,A1) '返回值与A1单元格内容相同的单元格个数
	=COUNTIF(Data,">"&A1) '返回值大于A1单元格内容的单元格个数
	=COUNTIF(Data,"<"&A1) '返回值小于A1单元格内容的单元格个数
	=COUNTIF(Data,"???") '返回字符等于3的单元格个数
	=COUNTIF(Data,"YDL") '返回值等于YDL的单元格个数
	=COUNTIF(Data,"YDL?") '返回以字母YDL开头且字符数等于4的单元格个数
	=COUNTIF(Data,"YDL*") '返回以字母YDL开头的文本单元格的个数
	=COUNTIF(Data,"?YDL*") '返回第2，3，4字符为YDL的单元格个数
	=COUNTIF(Data,"*YDL*") '返回含的YDL字符的单元格个数
	=COUNTIF(Data,"*"&A1&"*") '返回包含A1单元格内容的文本单元格个数
	=COUNTIF(Data,TODAY()) '返回值等于当前日期的单元格个数
	=COUNTIF(Data,">"&AVERAGE(Data)) '返回大于均值的单元格个数
	=SUM(COUNTIF(Data,">"&{10,15})*{1,-1}) '返回大于10小于等于15的单元格个数
	=SUM(COUNTIF(Data,{TRUE,FALSE})) '返回包含逻辑值的单元格个数
	
	'特别指出的是，在EXCEL2010中，新增了一个多条件计数函数，那就是“COUNTIFS” ，假如在一个EXCEL表格中，D3:D50单元格的内容是职工的年龄，E3:E50单元格的内容是是否有房，F3:F50单元格的内容是是否有车，那么统计职工中35岁以上有房有车的人数应该用如下公式：
	COUNTIFS(D3:D50,">35",E3:E50,"是",F3:F50,"是")

本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
