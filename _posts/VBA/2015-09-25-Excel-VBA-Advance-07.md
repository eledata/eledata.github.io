---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之公式下拉实现过程"
categories: DATA
tags: DATA
---

## 公式下拉实现
	
	'填入公式
	Sheets("Main_Data").Cells(3, 117).Formula = "=IF(SUM(CZ3:DL3)=0,"" "",""MPS Change"")"
	
	'将公式下拉，实现手工下拉效果
    'INV AVG
    Sheets("Main_Data").Activate
    Sheets("Main_Data").Range("U3:W3").Select
    Selection.AutoFill Destination:=Sheets("Main_Data").Range("U3:W" & result_number)
    Sheets("Main_Data").Range("U3:W" & result_number).Select


本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
