---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之VBA连接数据库"
categories: DATA
tags: DATA
---

## 利用VBA连接数据库

以下是VBA调用连接Oracle代码，基本用到的功能都全了。

    Set conn = CreateObject("ADODB.Connection")
    Set rst_query_1 = CreateObject("ADODB.Recordset")
    
    '利用ADO打开数据连接
    conn.Open "Provider = msdaora; Data Source = *** ; User Id = ***; Password = ***;"
    OraOpen = True
    conn.CursorLocation = 3
    
	'执行SQL语句，读取data
    Set rst_query_1 = conn.Execute(sql_day_1)

    If rst_stockout_com.RecordCount > 0 Then
        'Read Data to Excel
        LWC.Range("A2").CopyFromRecordset rst_stockout_com
    End If

	'关闭连接部分
	rst_query_1.Close
	conn.Close
    Set rst_query_1 = Nothing
    Set conn = Nothing

#### conn.CursorLocation = 3（adUseClient）
 
使用本地游标库提供的客户端的游标。本地游标服务通常允许执行驱动程序提供的游标所不允许的许多功能，因此使用此设置可以充分利用即将启用的功能。为进行向后兼容，亦支持同义字 adUseClientBatch。adUseClient = 1不使用游标服务。（此常量已作废并且只是为了向后兼容才出现。）adUseClient = 2默认值。使用数据提供者或驱动程序提供的游标。这些游标有时很灵活，可以额外感知其他人对数据源所做的更改使用2就是服务器端游标,这种情况下,很多的操作不是我们决定的而是数据库服务器来决定的。但是adUseClient = 3就是客户机游标,这种情况下,是一定能得到recordCount的。

本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
