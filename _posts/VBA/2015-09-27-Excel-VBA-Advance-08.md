---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之Workbook 和 Worksheet操作实战"
categories: DATA
tags: DATA
---
## Workbook 和 Worksheet操作实战

### VBA创建一个新的Workbook

以下代码的功能是，创建一个Excel文件，然后在Sheet里面填写一些内容。
	
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

### Work Sheet 上下分页

Down Sheet 作用是向下翻页，Up Sheet 向上翻页。 其中利用Activate

	Sub DownSheet()
	  Dim i As Integer
	  i = Worksheets.Count 'sheet 的数目
	  If ActiveSheet.Index < i Then
		  Worksheets(ActiveSheet.Index + 1).Activate '翻页的动作
	  Else
		  Worksheets(1).Activate
	  End If
	End Sub
	Sub UpSheet()
	  Dim i As Integer
	  i = Worksheets.Count
	  If ActiveSheet.Index > 1 Then
		  Worksheets(ActiveSheet.Index - 1).Activate
	  Else
		  Worksheets(i).Activate
	  End If
	End Sub

### Sheet Add

expression.Add(Before, After, Count, Type)

	Sub Add()
		Dim sh As Worksheet
		For Each Sh In Worksheets '检查是否有名称重复的
			If sh.Name = "Test" Then MsgBox "Sheet Exist!!"
				Exit Sub
			End If
		Next
		Set sh = Worksheets.Add(after := Worksheets(Worksheets.Count))
		sh.Name = "Test"
	End Sub

### 删除工作表中的空行

注意： 此处一定要从最大行数至最小行数开始循环判断，因为如果工作表中存在两行及两行以上的相邻空行，从最小行数开始循环删除的话，当第一行空行被删除后，被删除行下面的一行会往上移位，而此时For...Next循环的计数器已经加1，所以会出现漏删除的现象。

	Sub DelBlankRow()
	  Dim rRow As Long
	  Dim LRow As Long
	  Dim i As Long
	  rRow = Sheet1.UsedRange.Row '获得已使用区域的首行
	  LRow = rRow + Sheet1.UsedRange.Rows.Count - 1 '最后一行
	  For i = LRow To rRow Step -1 '从最大开始，遍历整个区域。由于删行会网上跳，所以从大到小比较合适
		  If Application.WorksheetFunction.CountA(Sheet1.Rows(i)) = 0 Then
			  Sheet1.Rows(i).Delete
		  End If
	  Next
	End Sub

### 删除工作表的重复行

利用CountIf来统计重复的次数，然后判断是否删掉当前的数据。

	Sub DeleteRow()
	  Dim R As Integer
	  Dim i As Integer
	  With Sheet1
		  R = .[a65536].End(xlUp).Row
		  For i = R To 1 Step -1
			  If WorksheetFunction.CountIf(.Columns(1), .Cells(i, 1)) > 1 Then
				  .Rows(i).Delete
			  End If
		  Next
	  End With
	End Sub

### 利用高级筛选出不重复的结果，并复制到另外一个sheet

	Sheet1.Range("A1").CurrentRegion.AdvancedFilter _
	Action:=xlFilterCopy, Unique:=True, _
	CopyToRange:=Sheet2.Range("A1")

本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
