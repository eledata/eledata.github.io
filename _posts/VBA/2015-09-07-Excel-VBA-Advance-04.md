---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之Windows Scripting Host 处理文件"
categories: DATA
tags: DATA
---

## Windows Scripting Host 处理文件

Windows Scripting Host(WSH)可以创建一些控制Windows操作系统和应用程序以及从操作系统中获取信息的小程序，而使用WSH的FileSystemObject对象可以用来处理文件系统。

在使用WSH处理文件时，必需使用CreateObject函数创建一个ActiveX对象（FileSystemObject对象），用来提供访问计算机的文件系统，如下面的代码所示：

	Dim MyFile As Object
	Set MyFile = CreateObject("Scripting.FileSystemObject")

### 用法如下：

属性	描述：

	Name	文件名称
	DateCreated	文件创建日期
	DateLastModified	文件最后修改日期
	DateLastAccessed	文件最后访问日期
	ParentFolder	文件的父文件夹
	Path	文件的完整路径
	Type	文件类型
	Size	以字节表示的文件大小

范例：

    Dim myFile As Object
    Dim path As String
    Dim fileInfo As String
    
    path = ThisWorkbook.path & "\PP_5040.txt"
    
    Set myFile = CreateObject("Scripting.FileSystemObject")
    With myFile.Getfile(path)
        fileInfo = StrMsg & "File Name:" & .Name & Chr(13) & "File Create Date: " & .DateCreated & Chr(13) & "File Change Date: " & .DateLastModified & Chr(13) & "File Access Date: " & .DateLastAccessed & Chr(13)
    End With
    MsgBox fileInfo
    Set myFile = Nothing

本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
