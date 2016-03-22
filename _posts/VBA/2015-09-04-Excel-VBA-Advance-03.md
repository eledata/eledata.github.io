---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之VBA文件对话框"
categories: DATA
tags: DATA
---

## VBA文件对话框应用详解

在VBA中经常要用到文件对话框来进行打开文件、选择文件或选择文件夹的操作。

#### 用法如下：

1. Application.FileDialog(fileDialogType)

	fileDialogType  MsoFileDialogType 类型，必需。文件对话框的类型。
	MsoFileDialogType 可为以下 MsoFileDialogType 常量之一。
	msoFileDialogFilePicker  允许用户选择文件。
	msoFileDialogFolderPicker  允许用户选择一个文件夹。
	msoFileDialogOpen  允许用户打开文件。用Excel打开。
	msoFileDialogSaveAs  允许用户保存一个文件。

2. msoFileDialogFilePicker

范例：

	Sub SelectFile()
	    '选择单一文件
	    With Application.FileDialog(msoFileDialogFilePicker)
	        .AllowMultiSelect = False   '单选择
	        .Filters.Clear   '清除文件过滤器
	        .Filters.Add "Excel Files", "*.xls;*.xlw"
	        .Filters.Add "All Files", "*.*"          '设置两个文件过滤器
	        If .Show = -1 Then    'FileDialog 对象的 Show 方法显示对话框，并且返回 -1（如果您按 OK）和 0（如果您按 Cancel）。
	            MsgBox "您选择的文件是：" & .SelectedItems(1), vbOKOnly + vbInformation, "智能Excel"
	        End If
	    End With
	End Sub

    Dim i As Integer
    '打开多个文件
    With Application.FileDialog(msoFileDialogFilePicker)
        .AllowMultiSelect = True
        .Filters.Clear
        .Filters.Add "Excel Files", "*.xls;*.xlw"
        .Filters.Add "All Files", "*.*"
        .Show
        For i = 1 To .SelectedItems.Count
            MsgBox "The file you select: " & .SelectedItems(i), vbOKOnly + vbInformation, "Excel"
        Next
    End With

	'打开多个文件，然后复制到一个文件中
    Dim fd As FileDialog
    Set fd = Application.FileDialog(msoFileDialogFilePicker)
    Set File_Cb = ActiveWorkbook
    
    With fd
    If .Show = -1 Then
        Dim vrtSelectedItem As Variant
        Dim i As Integer
        
        i = 1
        For Each vrtSelectedItem In .SelectedItems
            Dim tempwb As Workbook
            Set tempwb = Workbooks.Open(vrtSelectedItem)
            tempwb.Worksheets(1).Copy Before:=File_Cb.Worksheets(i)
            
            File_Cb.Worksheets(i).Name = Replace(tempwb.Name, ".xls", "")
            tempwb.Close Savechanges:=False
            i = i + 1
        Next vrtSelectedItem
    End If
    End With
    Set fd = Nothing

本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
