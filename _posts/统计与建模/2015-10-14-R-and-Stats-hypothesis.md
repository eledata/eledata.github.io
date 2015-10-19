---
layout: post
keywords: 假设检验
description: 假设检验
title: "R与统计建模--假设检验"
categories: 统计
tags: 统计
---

##二元数据相关性检验
###相关性系数
对于样本的二元数据，我们可以利用相关性系数r。

`r 公式：ρXY = cov(X,Y)/sqrt(var(X)*var(Y))`。

	#相关性系数有以下几个性质：
	1. | ρXY | = 1的充要条件是，存在常数a，b，使得P{Y=a+bX}=1
	2. 若X,Y 相互独立，且var(X)和var(Y) 存在， 那么ρXY = 0. 特别在正态分布中，X,Y 相互独立的充要条件是ρXY = 0

对于观测样本，按样本方差来计算。Sxx, Syy, Sxy.-- Sxx 变量为X的观测样本方差， Syy 变量Y的观测样本方差， Sxy 变量X, Y观测样本的协方差。

`r：ρXY = Sxy/sqrt(Sxx*Sxy)`


###如何利用R来进行二元相关检验
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

利用R提供的cor.test 来检验也可以：

	> newdata <- data.frame(
	+     x = c(6,	7,	3,	1,	2,	3,	10,	17,	12,	12,	12,	9,	20,	22,	18,	21,	13,	13,	15,	15,	16,	19,	14,	17,	14,	19,	18,	23,	20,	18),
	+     y = c(51,	25,	44,	25,	31,	10,	52,	11,	23,	15,	36,	15,	3,	26,	8,	40,	23,	9,	12,	18,	24,	12,	16,	9,	11,	19,	38,	30,	0,	0)
	+ )
	> 
	> newdata.mx<-mean(newdata$x); newdata.mx
	[1] 13.63333
	> newdata.my<-mean(newdata$y); newdata.my
	[1] 21.2
	> newdata.s<-cov(newdata); newdata.s
			  x         y
	x  38.17126 -29.33793
	y -29.33793 199.13103
	> newdata.r<-cor(newdata); newdata.r
			   x          y
	x  1.0000000 -0.3365052
	y -0.3365052  1.0000000
	> 
	> attach(newdata)
	The following objects are masked from newdata (pos = 3):

		x, y

	The following objects are masked from newdata (pos = 4):

		x, y

	> cor.test(x,y)

		Pearson's product-moment correlation

	data:  x and y
	t = -1.8909, df = 28, p-value = 0.06903 # P-value > 0.05 可以认为两组数据不相关
	alternative hypothesis: true correlation is not equal to 0
	95 percent confidence interval:
	 -0.62143616  0.02704248
	sample estimates:
		   cor 
	-0.3365052 