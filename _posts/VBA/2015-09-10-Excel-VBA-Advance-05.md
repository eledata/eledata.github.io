---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之Rnage"
categories: DATA
tags: DATA
---

## Range 对象

一个Range对象代表一个单元格、一行、一列、包含一个或者更多单元格区域（可以是连续的单元格，也可以是不连续的单元格）中选定的单元格，甚至是多个工作表上的一组单元格，在操作Excel 内的任何区域之前都需要将其表示为一个Range对象，然后使用该Range对象的方法和属性。

### 用法如下：

1. Range(Cell1,Cell2): 返回一个单元格，或是单元格区域。	
	Sheet1.Range("A3:F6").Select
2. Cells(RowIndex, ColumnIndex)
	For i = 1 To 100
	 Sheet2.Cells(i, 1).Value = i '循环从1到100，填入A1-100.
	Next
3. Offset(Rowoffset,Coloffset):返回一个基于引用的Range对象的单元格区域,正值，向下偏移，负值，向上偏移。
4. Activate：在Excel中，我们在同一时间只能操作一个Sheet，所以也就是在界面上激活一个Sheet。那么在VBA里面，我们就需要用到Activate函数，来帮助我们在不同的Sheet进行切换。
	
	'当前活动的Sheet 是Sheet2.要操作Sheet3，就要提前激活
	Sheet3.Activate
	Sheet3.Range("A1:A10").Activate

5. Set rng = Sheet1.Range("A65536").End(xlUp)：行列最后一个非空单元格
6. Sheet1.UsedRange.Rows.Count：算出整个Sheet内，已使用的Row的数目
7. 查找单元格
	'使用对话框，查找工作表中特定的内容单元格。
	Dim StrFind As String
	Dim Rng As Range
	Dim FindAddress As String
	StrFind = InputBox("Pls input the data:")

	If Trim(StrFind) <> "" Then
	   With Sheet1.Range("A:A")
	       Set Rng = .Find(What:=StrFind, _ '使用Find找到第一个点
	                       After:=.Cells(.Cells.Count), _
	                       LookIn:=xlValues, _
	                       LookAt:=xlWhole, _
	                       SearchOrder:=xlByRows, _
	                       SearchDirection:=xlNext, _
	                      MatchCase:=False)
	
	       If Not Rng Is Nothing Then
	           FindAddress = Rng.Address '找到的位置
	            Do
	                Rng.Interior.ColorIndex = 6
	                Set Rng = .FindNext(Rng) '找到下一个
	           Loop While Not Rng Is Nothing And Rng.Address <> FindAddress
	       End If
	   End With
	End If

8. 使用Like来匹配列的值

	pattern中的字符	符合string中的字符
	?任何单一字符
	*零个或多个字符
	#任何一个数字 (0–9)
	[charlist]	charlist中的任何单一字符
	[!charlist]	不在charlist中的任何单一字符

    Dim rng As Range
    Dim a As Integer
    a = 1
    Test_Area.Activate
    With Test_Area
        .Range("A:A").ClearContents
        For Each rng In .Range("B1:E1000") '确定搜索范围
            If rng.Text Like "*A*" Then	'使用Like，Pattern 
                .Range("A" & a) = rng.Text
                a = a + 1
            End If
        Next
    End With

9. 使用Range对象的Replace方法：Range("A1:A5").Replace "通州", "南通"
10. 
	Application.DisplayAlerts = False
	Sheet1.Range("A1").CurrentRegion.Copy Sheet2.Range("A1")
	Application.DisplayAlerts = True
	' Paste Special Value
	With Range("A1:A10")
	 .Copy
	 .PasteSpecial Paste:=xlPasteValues
	End With

11. Color 
	With Range("A1").Font
		.Name = "华文彩云"
		.FontStyle = "Bold"
		.Size = 18
		.ColorIndex = 3
		.Underline = 2
	End With
12. 在单元格中写入公式
	Sheets("Main_Data").Cells(3, 117).Formula = "=IF(SUM(CZ3:DL3)=0,"" "",""MPS Change"")" '双引号", 在里面要用两个""来替代，其余的就是正常写入


本篇完结！

参考文献
《VBA常用技巧》

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
