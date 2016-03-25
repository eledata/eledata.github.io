---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之Workbook"
categories: DATA
tags: DATA
---

## VBA创建一个新的Workbook


	
	Dim Nowbook As Workbook
	Dim ShName As Variant
	Dim Arr As Variant
	Dim i As Integer
	Dim myNewWorkbook As Integer
	myNewWorkbook = Application.SheetsInNewWorkbook
	ShName = Array("余额", "单价", "数量", "金额") '放成数组格式
	Arr = Array("01月", "02月", "03月", "04月", "05月", "06月", "07月", "08月", "09月", "10月", "11月", "12月")
	Application.SheetsInNewWorkbook = 4 'Sheet的数量是4个
	Set Nowbook = Workbooks.Add
	With Nowbook
		For i = 1 To 4
			With .Sheets(i)
				.Name = ShName(i - 1)
				.Range("B1").Resize(1, UBound(Arr) + 1) = Arr
			    .Range("A2") = "品名"
			End With
		Next
		.SaveAs Filename:=ThisWorkbook.Path & "\" & "存货明细.xls" '执行这段代码的文件地址上
		.Close Savechanges:=True
	End With
	Set Nowbook = Nothing
	Application.SheetsInNewWorkbook = myNewWorkbook


本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
