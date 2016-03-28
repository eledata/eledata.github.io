---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之优化VBA"
categories: DATA
tags: DATA
---
## 优化VBA

### Application.ScreenUpdating 

关闭Application.ScreenUpdating 会提高代码的运行速度。

	Sub Screen()
	  Dim i As Integer
	  Dim t As Date
	  Dim t1 As String
	  Dim t2 As String
	  Application.ScreenUpdating = False
	  t = Timer
	  For i = 1 To 30000
		  Cells(i, 1) = i
	  Next
	  t1 = Timer - t
	  Application.ScreenUpdating = True
	  t = Timer
	  For i = 1 To 30000
		  Cells(i, 1) = i
	  Next
	  t2 = Timer - t
	  MsgBox "Close ScreenUpdating runing time:" & Format(t1, "0.00000") & "S" _
		   & Chr(13) & "Open ScreenUpdating runing time:" & Format(t2, "0.00000") & "S"
	End Sub

### VBA中使用工作表函数

心得：VBA中使用工作表函数比仅仅使用VBA代码的运行时间要快得多!


### 少用激活或选择语句

在学习VBA的过程中我们经常通过录制新宏的方法来获得所需的代码，但是在录制宏的过程中会记录所有的动作，代码中有大量的Select和Activate语句，而这些代码往往是不必要而且会影响代码的运行速度。所以通过录制新宏的方法获得的代码在使用时需要进行修改以加快运行速度，如下面的代码所示：

	Private Sub SU_Click()
	  Dim i As Integer
	  Dim t As Date
	  Dim t1 As String
	  Dim t2 As String
	  Application.ScreenUpdating = False
	  Test_Area.Activate
	  t = Timer
	  For i = 1 To 3000
	      Test_Area.Select
	      Test_Area.Range("A1").Select
	      ActiveCell.FormulaR1C1 = "1"
	  Next
	  
	  t1 = Timer - t
	  Application.ScreenUpdating = True
	  t = Timer
	  For i = 1 To 3000
	      Test_Area.Range("A1") = 1
	  Next
	  t2 = Timer - t
	  MsgBox "Close ScreenUpdating runing time:" & Format(t1, "0.00000") & "S" _
	       & Chr(13) & "Open ScreenUpdating runing time:" & Format(t2, "0.00000") & "S"
	End Sub

### 使用With语句引用对象

With语句可以对某个对象执行一系列的语句，而不用重复指出对象的名称。在运行时只需引用对象一次而不是在每个属性赋值时都要引用，从而获得较快的运行速度。

	With Object
	    [statements]
	End With

### 使用更快的单元格操作方法

在对单元格区域进行操作时，使用Find、Replace、SpecialCells等方法可以比使用VBA代码获得更快的速度。

本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
