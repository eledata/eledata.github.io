---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之VBA调用Outlook发邮件"
categories: DATA
tags: DATA
---

## 利用VBA调用Outlook 发邮件

以下是VBA调用outlook代码，基本用到的功能都全了。


    Dim OutlookApp As Object
    Dim OutlookItem As Object
    Dim Receiver As String
    Dim SubjectText As String
    Dim BodyText As String
    Dim AttachedObject As String
    Dim HTML As String
    Dim i As Integer
    
    Set OutlookApp = CreateObject("Outlook.Application")
    Set OutlookItem = OutlookApp.CreateItem(olMailItem)
    
    Receiver = "mhuang1@ra.rockwell.com"
    SubjectText = "Test_Head"
    BodyText = "Hi All:" & vbCrLf & _
            vbCrLf & _
            "Please find attached doc of AP_Inv_Trend_Daily" & vbCrLf & _
            vbCrLf & _
            "Share Disk Link:" & vbCrLf & _
            "\\apsgtuafile001\APR Logistics DW\AP Planning Shared folder\AP Inventory Report\" & vbCrLf & _
            "" & vbCrLf & _
            "Moyue Huang" & vbCrLf & _
            "Data Analyst" & vbCrLf & _
            "VOIP:25817" & vbCrLf & _
            "Rockwell Automation" & vbCrLf & _
            "LISTEN. THINK. SOLVE." & vbCrLf & _
        "**************************************************************************" & vbCrLf & _
        "(This's an automated e-mail notification, please do not reply this message.)"
               
    AttachedObject = "C:\\unintall.log"
    With OutlookItem
        .To = Receiver
        .CC = Receiver
        .BCC = Receiver
        .Subject = SubjectText
        .Body = BodyText
        .Attachments.Add AttachedObject
        .Send
    End With
    Set OutlookItem = Nothing
    Set OutlookApp = Nothing

	#Mail Item 参考
	MaiItem object:
	SentOnBehalfOfName
	SenderName
	ReceivedByName
	ReceivedOnBehalfOfName
	ReplyRecipientNames
	To
	CC
	BCC
	Body
	HTMLBody
	Recipients
	SenderEmailAddress
	

本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
