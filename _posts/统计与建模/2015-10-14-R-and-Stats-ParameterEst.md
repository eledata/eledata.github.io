---
layout: post
keywords: 参数估计
description: 参数估计
title: "R与统计建模--参数估计"
categories: 统计
tags: 统计
---

##参数估计
根据样本来估计总体分布所包含的未知参数。通常有两种形式：`点估计（未知参数大致是多少）和区间估计（两个统计量所构成的区间来估计一个未知的参数）`

###点估计
1. 矩法估计

方法：用样本矩来估计总体中相应的参数. 最简单的矩估计法是用一阶样本原点矩来估计总体的期望而用二阶样本中心矩来估计总体的方差.

`E(X^k) = 1/n * sum(Xi^k)`。

	#通过计算一阶原点矩来计算总体的期望和二阶中心距来估计总体的方差，来估计均值和方差：
	1. A1 = 1/n * sum(Xi) = E(X)
	2. A2 = 1/n * sum(Xi^2) = Var(X)
	3. 方差的样本矩≠S^2. == ((n - 1)/n) * S^2


2.极大似然估计

原理：一个随机试验如有若干个可能的结果A，B，C，…。若在仅仅作一次试验中，结果A出现，则一般认为试验条件对A出现有利，也即A出现的概率很大。一般地，事件A发生的概率与参数theta相关，A发生的概率记为P(A，θ)，则theta的估计应该使上述概率达到最大，这样的θ顾名思义称为极大似然估计。

	#求解极大似然估计的方法
	（1）写出似然函数；
	（2）对似然函数取对数，并整理；
	（3）求导数；
	（4）解似然方程 。

####估计量优良准则
	1. 无偏估计 --E(Ô) = θ # 一阶样本原点矩估计都是无偏估计，二阶属于渐进无偏估计。
	2. 有效性 -- Var(Ô1) > Var(Ô2), 那么认为Ô1 比 Ô2 有效。
	3. 一致性 -- lim P{|Ô-θ| < ε} = 1


###区间估计
区间估计（interval estimation）是从点估计值和抽样标准误出发，按给定的概率值建立包含待估计参数的区间.其中这个给定的概率值称为置信度或置信水平(confidence level），这个建立起来的包含待估计参数的区间称为置信区间（confidence interval），指总体参数值落在样本统计值某一区内的概率；而置信区间是指在某一置信水平下，样本统计值与总体参数值间误差范围。置信区间越大，置信水平越高。划定置信区间的两个数值分别称为置信下限(lower confidence limit,lcl）和置信上限（upper confidence limit,ucl)

R提供许多函数来验证二元数据，例如：计算`协方差的函数: cov(x, y = NULL, use = "everything", method = c("pearson", "kendall", "spearman"))`

`计算相关性系数的函数： cor(x, y = NULL, use = "everything",method = c("pearson", "kendall", "spearman"))`

	#二元数据相关性计算
	> ore<-data.frame(
	+      x=c(67, 54, 72, 64, 39, 22, 58, 43, 46, 34),
	+      y=c(24, 15, 23, 19, 16, 11, 20, 16.1, 17, 13)
	+ )
	> ore.mx<-mean(ore$x); ore.mx
	[1] 49.9 #平均数
	> ore.my<-mean(ore$y); ore.my
	[1] 17.41
	> ore.s<-cov(ore); ore.s
			  x        y  #协方差矩阵
	x 252.76667 60.52333   Sxx	Sxy
	y  60.52333 17.12544   Sxy  Syy
	> ore.r<-cor(ore); ore.r
			 x        y   #相关性系数矩阵
	x 1.000000 0.919903   ρXX  ρXY
	y 0.919903 1.000000	  ρXY  ρYY

####接下来有个问题，样本的相关系数与总体的相关系数有什么关系？
有这么几种方法：

- pearson 相关性检验 `cor.test(x, y,alternative = c("two.sided", "less", "greater"),method = c("pearson", "kendall", "spearman"),exact = NULL, conf.level = 0.95, continuity = FALSE, ...)`
- kendall 相关性检验

也可以用鲁宾废除的总体相关系数的区间估计的近似逼近公式

	ruben.test<-function(n, r, alpha=0.05){
	   u<-qnorm(1-alpha/2)
	   r_star<-r/sqrt(1-r^2)
	   a<-2*n-3-u^2; b<-r_star*sqrt((2*n-3)*(2*n-5))
	   c<-(2*n-5-u^2)*r_star^2-2*u^2
	   y1<-(b-sqrt(b^2-a*c))/a
	   y2<-(b+sqrt(b^2-a*c))/a
	   data.frame(n=n, r=r, conf=1-alpha, 
		  L=y1/sqrt(1+y1^2), U=y2/sqrt(1+y2^2))
	}
	source("ruben.R")
	ruben(....)
