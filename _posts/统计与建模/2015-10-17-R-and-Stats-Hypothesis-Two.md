---
layout: post
keywords: 假设检验
description: 假设检验之非参数的假设检验
title: "R与统计建模--假设检验（二）"
categories: 统计
tags: 统计
---
## Pearson 卡方拟合优度检验
用卡方统计量进行统计显著性检验的重要内容之一。它是依据总体分布状况，计算出分类变量中各类别的期望频数，与分布的观察频数进行对比，

判断期望频数与观察频数是否有显著差异，从而达到从分类变量进行分析的目的。`检验来自总体中一类数据其分布是否和理论分布相一致的统计方法`

Pearson 卡方统计量：
![1](/public/img/posts/jiashejianyan2pearsonk1.png)

P_Value检验：
![1](/public/img/posts/jiashejianyan2pearsonp1.png)

Pearson检验的几种用途：

a. 检验来自总体中一类数据不同水平的差异：

![1](/public/img/posts/jiashejianyan2pearsonq2.png)

		#判断某个总体的不同的水平下是否存在显著差异。
		X <- c(210, 312, 170, 85, 223)
		n <- sum(X)
		m <- length(X)
		p <- rep(1/m, m)
		K <- sum((X - n*p)^2/(n*p))
		K
		pr <- 1 - pchisq(K, m - 1)
		pr


b. 检验来自总体中一类数据其分布是否和理论分布相一致：

![1](/public/img/posts/jiashejianyan2pearsonq1.png)

		#检验某个总体是否符合理论分布
		X <- 0:4
		Y <- c(7, 10, 12, 8, 5)
		q <- ppois(X, mean(rep(X,Y)))
		n <- length(Y)
		p[1] <- q[1]
		p[n] <- 1 - q[n-1]
		for(i in 2:(n - 1)){
		  p[i] <- q[i] - q[i - 1]
		}

		chisq.test(Y,p=p)

### 列联表数据的独立性检验

什么是列联表？

![1](/public/img/posts/jiashejianyan2lianliebiao2.png)

列联表pearson 卡方统计量和P值：
![1](/public/img/posts/jiashejianyan2lianliebiaoK.png) 
![1](/public/img/posts/jiashejianyan2lielianbiaop.png)

例子：

![1](/public/img/posts/jiashejianyan2lianliebiaoq1.png) 

		#列联表
		X <- data.frame(
		  a1 <- c(20, 24, 80, 82),
		  a2 <- c(22, 38, 104, 125),
		  a3 <- c(13, 28, 81, 113),
		  a4 <- c(7, 18, 54, 92)
		)
		chisq.test(X)

### Fisher精确独立检验

针对2X2列联表的独立检验

![1](/public/img/posts/jiashejianyan2fisherq.png)

		#Fisher 精确独立检验
		x <- c(4,5,18,6)
		dim(x) <- c(2,2)
		fisher.test(x)

### 符号检验

binom.test()可以用于符号检验

### 秩相关检验

秩相关检验不要求验证的数据来自于正态分布。

1. spearman 秩检验

cor.test(x,y,alternative = c("two.sided", "less","greater"), method = "spearman", conf.level = 0.95)

![1](/public/img/posts/jiashejianyan2spearman.png)

		> #spearman
		> x <- c(1,2,3,4,5,6)
		> y <- c(6,5,4,3,2,1)
		> cor.test(x,y,method = "spearman")

			Spearman's rank correlation rho

		data:  x and y
		S = 70, p-value = 0.002778
		alternative hypothesis: true rho is not equal to 0
		sample estimates:
		rho 
		 -1 

2. Kendall 检验

cor.test(x,y,alternative = c("two.sided", "less","greater"), method = "kendall", conf.level = 0.95)

![1](/public/img/posts/jiashejianyan2kendall1.png)
![1](/public/img/posts/jiashejianyan2kendall2.png)

3. Wilconxon 检验

		
关于正态总体的假设检验就介绍到此。

本篇完结！

参考文献

1.《统计建模与R》

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@