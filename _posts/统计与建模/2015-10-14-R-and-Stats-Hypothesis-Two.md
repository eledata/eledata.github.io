---
layout: post
keywords: 假设检验
description: 假设检验之非参数的假设检验
title: "R与统计建模--假设检验（二）"
categories: 统计
tags: 统计
---
##假设检验之基本思想

在对统计假设进行检验之前，我们要知道两方面知识：

`小概率原理：概率很小的事件在一次试验中几部不可能发生！`

`反证法思想：先提出假设，再用适当的方法确定假设成立的可能性大小，如可能性小，则认为假设不成立，若可能性大，则还不能认为不假设成立。`

我个人觉得，假设检验的思想在于，`建立原假设和备选假设，通过显著性水平，判断接纳与否。`

###假设检验的基本步骤 
1.建立假设

在假设检验中，常把一个被检验的假设称为原假设，用H0表示，通常将不应轻易加以否定的假设作为原假设。当H0被拒绝时而接收的假设称为备择假设，用H1表示，它们常常成对出现。

2.选择检验统计量，给出拒绝域形式与显著性水平

拒绝域与显著性水平之间的关系：

` α 越大， 拒绝域越大。 反之越小`

![1](/public/img/posts/jujueyu.png)

3.确定显著性水平后，可以定出检验的拒绝域W。

4.根据W，来判断对H0是否有利，之后做出结论。

***现在用一个例子来回顾假设检验的步骤：

![1](/public/img/posts/jiashejianyan1examp1.png)

		> 2*(1-pnorm(2.15))
			[1] 0.03155521
		#结论小于0.05 大于0.01。我们对P值有这么样的检验标准，P值小于指定的显著性水平α时，拒绝原假设，否则接受原假设。

***之前一直有个误区，P值和拒绝域X判定之间的误解。P < α，那么就是拒绝原假设。对于拒绝域，X > W, 那么拒绝原假设。***	

###正态总体的假设检验
参数检验的三种情况：
![1](/public/img/posts/jiashejianyan1jiasheqingkuang1.png)

####μ假设检验情况
`μ 单个总体检验`：

![1](/public/img/posts/jiashejianyan1junzhidange1.png)

`μ1-μ2 两个总体检验`：

![1](/public/img/posts/jiashejianyan1junzhisuan1.png)
![1](/public/img/posts/jiashejianyan1junzhisuan2.png)

		#P Value Calculation Method
		P_Value <- function(cdf, x, paramet = numeric(0), side = 0){
		  n <- length(x)
		  P <- switch (n + 1,
			cdf(x),
			cdf(x, paramet),
			cdf(x, paramet[1], paramet[2]),
			cdf(x, paramet[1], paramet[2], paramet[3])
		  )
		  if (side < 0) P
		  else if (side > 0) 1 - P
		  else 
			if(P < 0.5) 2*P
			else 2*(1 - P)
		}

		mean_test1 <- function(x, mu  = 0, sigma = -1, side = 0){
		  n <- length(x)
		  xb <- mean(x)
		  if(sigma > 0){
			z <- (xb - mu)/(sigma/sqrt(n))
			P <- P_Value(pnorm, z, side = side)
			data.frame(mean = xb, df = n, Z = z, P_Value = P)
		  }else{
			t <- (xb - mu)/(sd(x)/sqrt(n))
			P <- P_Value(pt, t, paramet = n - 1, side = side)
			data.frame(mean = xb, df = n - 1, T = t, P_Value = P)
		  }
		}

		X<-c(159, 280, 101, 212, 224, 379, 179, 264,
			 222, 362, 168, 250, 149, 260, 485, 170)
		mean_test1(X, mu = 225, side = 1)

		mean_test2 <- function(x, y, var.equal = FALSE, sigma = c(-1, -1), side = 0){
		  nx <- length(x)
		  ny <- length(y)
		  mx <- mean(x)
		  my <- mean(y)
		  if(all(sigma > 0)){
			z <- (mx - my)/(sqrt(sigma[1]^2/nx + sigma[2]^2/ny))
			P <- P_Value(pnorm, z, side = side)
			data.frame(mean = mx - my, df = nx + ny, Z = z, P_Value = P)
		  }else{
			if(var.equal == TRUE){
			  sw <- sqrt(((nx - 1)*sd(x)^2 + (ny - 1)*sd(x)^2)/(nx + ny - 2))
			  t <- (mx - my)/(sw*(sqrt(1/nx + 1/ny)))
			  nu <- nx + ny - 2
			}else{
				s1 <- var(x)
				s2 <- var(y)
				nu <- (s1/nx + s2/ny)^2/(s1^2/(nx^2*(nx - 1)) + s2^2/ny^2*(ny - 1))
				t <- (mx - my)/sqrt(s1/nx + s2/ny)
			  }
			P <- P_Value(pt, t, nu, side = side)
			data.frame(mean = mx - my, df = nu, T = t, P_Value = P)
		  }
		}

		X<-c(78.1, 72.4, 76.2, 74.3, 77.4, 78.4, 76.0, 75.5, 76.7, 77.3)
		Y<-c(79.1, 81.0, 77.3, 79.1, 80.0, 79.1, 79.1, 77.3, 80.2, 82.1)

		mean_test2(X,Y, var.equal=TRUE, side=-1)
		mean_test2(X,Y, side=-1)

####μ假设检验情况
`σ单个总体检验`：

![1](/public/img/posts/jiashejianyan1fangchadan1.png)

`σ两个总体检验`：

![1](/public/img/posts/jiashejianyan1fangchasuan1.png)

		X<-c(78.1, 72.4, 76.2, 74.3, 77.4, 78.4, 76.0, 75.5, 76.7, 77.3)
		Y<-c(79.1, 81.0, 77.3, 79.1, 80.0, 79.1, 79.1, 77.3, 80.2, 82.1)
		
		#单个总体的检验情况
		var_test1 <- function(x, sigma = 1, mu = Inf, side = 0){
		  n <- length(x)
		  if(mu < Inf){
			s <- sum(x - mu)^2/n
			df = n
		  }else{
			s <- var(x)
			df = n - 1
		  }
		  k <- df*s/sigma
		  P <- P_Value(pchisq, k, paramet = df, side = side)
		  data.frame(var = s, df = df, chisq = k, P_Value = P)
		}
		var_test1(X, sigma = 3.5)
		
		#两个总体的检验情况
		var_test2 <- function(x, y, mu = c(Inf, Inf), side = 0){
		  nx <- length(x)
		  ny <- length(y)
		  if(all(mu < Inf)){
			sx <- sum(x - mu[1])^2/nx
			sy <- sum(y - mu[2])^2/ny
			df1 <- nx
			df2 <- ny
		  }else{
			sx <- var(x)
			sy <- var(y)
			df1 <- nx - 1
			df2 <- ny - 1
		  }
		  f <- sx/sy
		  P <- P_Value(pf, f, paramet = c(df1, df2), side = side)
		  data.frame(rate = f, df1 = df1, df2 = df2, F = f, P_Value = P)
		}

		var_test2(X,Y)

关于正态总体的假设检验就介绍到此。

本篇完结！

参考文献

1.《统计建模与R》

@This site is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License@