---
layout: post
keywords: 参数估计
description: 参数估计
title: "R与统计建模--参数估计"
categories: 统计
tags: 统计
---

1. 矩法估计

方法：用样本矩来估计总体中相应的参数. 最简单的矩估计法是用一阶样本原点矩来估计总体的期望而用二阶样本中心矩来估计总体的方差.

`E(X^k) = 1/n * sum(Xi^k)`。

	#通过计算一阶原点矩来计算总体的期望和二阶中心距来估计总体的方差，来估计均值和方差：
	1. A1 = 1/n * sum(Xi) = E(X)
	2. A2 = 1/n * sum(Xi^2) = Var(X)
	3. 方差的样本矩≠S^2. == ((n - 1)/n) * S^2


2.极大似然估计

原理：一个随机试验如有若干个可能的结果A，B，C，…。若在仅仅作一次试验中，结果A出现，则一般认为试验条件对A出现有利，也即A出现的概率很大。一般地，事件A发生的概率与参数θ相关，A发生的概率记为P(A，θ)，则θ的估计应该使上述概率达到最大，这样的θ顾名思义称为极大似然估计。

	#求解极大似然估计的方法
	（1）写出似然函数
	（2）对似然函数取对数，并整理
	（3）求导数
	（4）解似然方程

####估计量优良准则
	1. 无偏估计 --E(Ô) = θ # 一阶样本原点矩估计都是无偏估计，二阶属于渐进无偏估计。
	2. 有效性 -- Var(Ô1) > Var(Ô2), 那么认为Ô1 比 Ô2 有效。
	3. 一致性 -- lim P{|Ô-θ| < ε} = 1


###区间估计
区间估计（interval estimation）是从点估计值和抽样标准误出发，按给定的概率值建立包含待估计参数的区间。

其中这个给定的概率值(1-α)称为置信度或置信水平(confidence level）`指总体参数值落在样本统计值某一区内的概率`。置信区间是指在某一置信水平下，样本统计值与总体参数值间误差范围。`置信区间越大，置信水平越高。`划定置信区间的两个数值分别称为`置信下限(lower confidence limit,lcl）和置信上限（upper confidence limit,ucl)`

#### 一个正态总体的情况
#####均值μ和σ的区间估计
1. σ已知时μ的置信区间与σ未知时μ的置信区间两种情况：

		#用R代码来解释
		interval_estimate_m <- function(x, sigma = -1, alpha = 0.05){
		    n <- length(x)
		    m <- mean(x)
		    
		    if(sigma >= 0){
		      tmp <- (sigma/sqrt(n))*qnorm(1 - alpha/2) #σ 已知的情况
		      df <- n
		    }
		    else{
		      tmp <- (sd(x)/sqrt(n))*qt(1 - alpha/2, n-1) # σ 未知的情况，用样本方差
		      df <- n - 1
		    }
		    data.frame(mean = m, df = df, a = m - tmp, b = m + tmp)
		}

2. μ已知时σ的置信区间与μ未知时σ的置信区间两种情况：

		#用R代码来解释
		interval_estimate_var <- function(x, m = Inf, alpha = 0.05){
		  n <- length(x)
		  
		  if(m < Inf){
		    S <- sum((x-m)^2)/n
		    df <- n
		  }
		  else{
		    S <- sum((x-m)^2)/(n - 1)
		    df <- n - 1
		  }
		  a <- df*S/qchisq(1 - alpha/2, df)
		  b <- df*S/dchisq(alpha/2, df)
		  data.frame(var = S, df = df, a = a, b = b)
		}

####两个正态总体的情况
1.μ1-μ2 的情况。
	
		#用R代码来理解
		interval_estimate_mm <- function(x, y, sigma = c(-1, -1), var.equal = FALSE, alpha = 0.05){
			n1 <- length(x)
			n2 <- length(y)
			xm <- mean(x)
			ym <- mean(y)
			if (all(sigma >= 0 )){
			  tmp <- qnorm(1-alpha/2)*sqrt(sigma[1]^2/n1 + sigma[2]^2/n2) # σ可知
			  df <- n1 + n2
			}else{ #σ 未知
			  if(var.equal == TRUE){ # σ 未知但知道其相等
			    sw <- sqrt(((n1-1)*var(x)^2 + (n2-1)*var(y)^2)/(n1 + n2 -2))
			    tmp <- qt(1-alpha/2, n1+n2-2)*sqrt(1/n1 + 1/n2)*sw
			    df <- n1 + n2 -2
			  }else{
			    S1 <- var(x)
			    S2 <- var(y)
			    nu <- (S1^2/n1 + S2^2/n2)^2/((S1)^4/(n1^2*(n1-1)) + (S2)^4/(n2^2*(n2-1)))
			    tmp <- qt(1-alpha/2,nu)*sqrt(S1^2/n1 + S2^2/n2)
			    df <- nu
			  }
			}
			data.frame(mean  = xm - ym, df = df, a = xm-ym-tmp, b = xm-ym + tmp)
		}


2. 方差比：σ1^2/σ2^2 的情况
	
		#用R代码来理解
		interval_var2<-function(x,y, 
		   mu=c(Inf, Inf), alpha=0.05){ 
		   n1<-length(x); n2<-length(y) 
		   if (all(mu<Inf)){
		      Sx2<-1/n1*sum((x-mu[1])^2); Sy2<-1/n2*sum((y-mu[2])^2)
		      df1<-n1; df2<-n2
		   }
		   else{
		      Sx2<-var(x); Sy2<-var(y); df1<-n1-1; df2<-n2-1
		   }
		   r<-Sx2/Sy2
		   a<-r/qf(1-alpha/2,df1,df2)
		   b<-r/qf(alpha/2,df1,df2)
		   data.frame(rate=r, df1=df1, df2=df2, a=a, b=b)
		}