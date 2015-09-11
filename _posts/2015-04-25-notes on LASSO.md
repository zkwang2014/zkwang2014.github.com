---
layout: post
title: "notes on LASSO"
description: "notes"
category: notes
tags: [LASSO]
---
The definition of the Lasso is in the paper “Regression Shrinkage and Selection via the Lasso” by Tibshirani.  Then R package “glmnet” can be used to implement the Lasso.  

The program sim.2r, below is discussed in class.  The output of sim.2r is included after sim.2r.  

{% highlight r %}
> sim2.r
function (n=150,varcon=2) 
{
library(MASS)
library(glmnet)
cov_x<-varcon*1
cov_y<-varcon*diag(21)
vg1<-rep(1/sqrt(5),5)
vg0<-rep(0,16)
vg<-c(vg1,vg0)

beta_g<-vg
cov_yx<-beta_g
cov_xy<-t(beta_g)
truecor<-cov_xy%*%beta_g/varcon

cov<-matrix(,22,22)
cov[1,1]<-cov_x
cov[c(2:22),c(2:22)]<-cov_y
cov[1,c(2:22)]<-cov_xy
cov[c(2:22),1]<-cov_yx
#set.seed(20)
z<-mvrnorm(n=n,mu=rep(0,22),Sigma=cov,tol = 1e-5, empirical = FALSE)
x<-z[,1]
y<-z[,c(2:22)]
dimnames(y)[[2]]<-c(1:21)
dat<-data.frame(cbind(x,y))
lm.out<-lm(x~-1+.,dat)$coef
lam<-cv.glmnet(x=y,y=x,family="gaussian")
ids<-c(1:length(lam$lambda))
whichmin<-ids[lam$lambda==lam$lambda.min]
beta<-lam$glmnet.fit$beta
penalized<-round(beta[,whichmin],5)
unpenalized<-round(lm.out,5)
## step function
s<-matrix(0,n,21)
s[,1]<-y[,1]
for (i in 2:21)
s[,i]<-apply(y[,1:i],1,sum)
lam2<-cv.glmnet(x=s,y=x,family="gaussian")
ids2<-c(1:length(lam2$lambda))
whichmin2<-ids[lam2$lambda==lam2$lambda.min]
tmp<-round(lam2$glmnet.fit$beta[,whichmin2],5)
beta2<-rep(0,21)
for (i in 1:21)
beta2[i]<-sum(tmp[21:i])
penalizedstep<-beta2
##
par(mfrow=c(3,1))
plot(penalized)
abline(0,0)
plot(penalizedstep)
abline(0,0)
plot(unpenalized)
abline(0,0)
pencor<-cor(x,y%*%penalized)
penstepcor<-cor(x,y%*%penalizedstep)
unpencor<-cor(x,y%*%unpenalized)
list(lam=lam$lambda.min,nlam=length(lam$lambda),beta=round(beta[,whichmin],5),lmbeta=round(lm.out,5),
pencor=pencor,penstepcor=penstepcor,unpencor=unpencor,truecor=truecor)
}

$lam
[1] 0.08031

$nlam
[1] 64

$beta
       1        2        3        4        5        6        7        8        9       10       11 
 0.22048  0.17928  0.11362  0.23612  0.21675  0.01679  0.00000 -0.08712  0.01972  0.00000 -0.14530 
      12       13       14       15       16       17       18       19       20       21 
 0.00000  0.00000  0.00000  0.00000  0.00000  0.01039  0.00000 -0.08620 -0.01936  0.01104 

$lmbeta
      X1       X2       X3       X4       X5       X6       X7       X8       X9      X10      X11 
 0.32184  0.20241  0.16064  0.30830  0.27180  0.10042  0.02708 -0.15660  0.09694 -0.03185 -0.19897 
     X12      X13      X14      X15      X16      X17      X18      X19      X20      X21 
 0.01429  0.02767 -0.00613 -0.04770 -0.01259  0.06627  0.04850 -0.13220 -0.07541  0.07728 

$pencor
        [,1]
[1,] 0.65817

$penstepcor
        [,1]
[1,] 0.58997

$unpencor
        [,1]
[1,] 0.67855

$truecor
     [,1]
[1,]  0.5
{% endhighlight %}