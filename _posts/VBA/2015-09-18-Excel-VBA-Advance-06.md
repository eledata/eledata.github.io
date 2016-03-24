---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之单击，双击函数"
categories: DATA
tags: DATA
---

## 单击，双击函数

### 单击函数

Worksheet_SelectionChange VBA 内置的函数：

	Private Sub Worksheet_SelectionChange(ByVal Target As Range)  '类似单击事件
	Application.EnableEvents = False
	   If Target.Column = 1 And Target.Row >= 2 And Target.Row <= 9 And Target.Cells.Count = 1 Then
	         If Cells(Target.Row, 2) = "√" Then
	            Cells(Target.Row, 2) = ""
	            Cells(Target.Row, 2).Select
	         Else
	            Cells(Target.Row, 2) = "√"
	            Cells(Target.Row, 2).Select
	         End If
	   End If
	Application.EnableEvents = True
	End Sub

### 双击函数

Worksheet_BeforeDoubleClick VBA 内置的函数：

	Private Sub Worksheet_BeforeDoubleClick(ByVal Target As Range, Cancel As Boolean)
		If Target.Locked = True Then
			 MsgBox "此单元格已保护，不能编辑!"
			 Cancel = True
		End If
	End Sub

本篇完结！

参考文献
《VBA常用技巧》

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
