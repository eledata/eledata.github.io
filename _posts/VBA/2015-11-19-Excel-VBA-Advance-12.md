---
layout: post
keywords: VBA
description: Advance Excel VBA Skill
title: "Advance Excel VBA 系列之取整"
categories: DATA
tags: DATA
---
## 取整

### VBA取整函数集合

1、四舍五入取整，一般用于取近似数

（1）CInt：只能在VBA中使用

	CInt(12.64)=13，
	CInt(12.34)=12，
	CInt(-12.64)=-13，
	CInt(-12.34)=-12

（2）Round：在VBA中使用和CInt相同

	Round(12.64)=13，
	Round(12.46)=12，
	Round(-12.64)=-13，
	Round(-12.46)=-12，

此函数实际上有两个参数，第二个参数表示取小数的位数，或略表示取整，即小数位数为0。该函数还可以在工作表中使用，使用时两个参数必须写全，即：

	Round(12.64,0)=13
	Round(12.64,1)=12.6

2、取整数部分，小数舍弃，常用于取整数和余数

（1）Fix：只能在VBA中使用

	Fix(12.64)=12，
	Fix(12.34)=12，
	Fix(-12.64)=-12，
	Fix(-12.34)=-12

（2）Int：在VBA和工作表中都可以使用

此函数取正数时和Fix相同，负数时往绝对值高的方向取，就是说，取小于其值得整数， 

	Int(12.64)=12，
	Int(12.46)=12，
	Int(-12.64)=-13，
	Int(-12.46)=-13

举例：买50个鸡蛋，12个鸡蛋一盒，那么需要Fix(50/12)=4盒，零头50 mod 12=2个

3、往上取整，只要有小数，整数部分就加1，常用于资费计算

邮件资费定价时常用不足500g按500g计算，即501g相当于2个500g计算资费。本功能VBA没有专门的函数，不过可以用数值+0.5再四舍五入取整实现，例如：

	CInt(12.64+0.5)=13，
	CInt(12.46+0.5)=13，
	CInt(12.01+0.5)=13

不过，工作表中还是有一个Ceiling函数可以实现这个功能，Ceiling(12.01,1)=13，第二个参数1表示舍入到最近的整数。VBA中可以用下列方式引用：

	a = Application.Ceiling(12.06, 1)         'a=13

Ceiling函数功能比较复杂，这儿就不详细介绍了。

4、关于Round函数进行四舍五入

VBA中Round函数进行四舍五入并不是逢5就入，例如：

round(0.5)=0、 round(1.5)=2 、 round(2.5)=2 、round(3.5)= 4 、round(4.5)=4 ，难到还分奇偶？答案是确实分奇偶，在VBA中Round函数是采用“银行家舍入”，建议大家在VBA中慎重使用Round函数来四舍五入。什么是“银行家舍入”呢，定义如下：
“四舍六入五考虑，五后非零就进一，五后为零看奇偶，五前为偶应舍去，五前为奇要进一”。这个四舍五入法是一个国际标准，大部分的编程软件都使用的是这种方法，据说国际上一般都是用这种方法的。
如果在Excel VBA中进行四舍五入处理，也可以直接调用Excel工作表函数，达到直接四舍五入的目的Application.Round(A,B)，例如，下面例句可以看出运行效果：

    a = Application.Round(12.5, 0)  'a=13
    b = Round(12.5)                        'b=12


本篇完结！

参考文献
无

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@
