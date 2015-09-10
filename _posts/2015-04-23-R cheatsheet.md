{% highlight r %}
<p>
```r

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

```
</p>

{% endhighlight %}
