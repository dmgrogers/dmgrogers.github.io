---
layout: default
title: R basic linear modeling and inference
---

Eventually it would be nice to translate this into R markdown instead:

~~~r

##################################
### Basic Linear Least Squares ###
##################################


year<-seq(0,4,by=1)
rate<-c(9.34,8.5,7.62,6.93,6.60)
dev.new()
dev.set(2)
plot(year,rate,main="Commercial Interest Rate for a 4-year Car Loan")
cor(year,rate)

# "By hand", calculate the coefficient of x for the least-squares regression line

sxx<-sum((year-mean(year))^2)/length(year)
syy<-sum((rate-mean(rate))^2)/length(rate)
sxy<-sum((year-mean(year))*(rate-mean(rate)))/length(rate)
bhat1=sxy/sxx
bhat1

# Note that this slope is slightly shallow - the regression line will not cut "straight through"!
# "By hand", calculate the correlation coefficient
r=bhat1*(sxx/syy)^.5
r

# Now, moving on: a linear model
fit<-lm(rate~year)
attributes(fit)   # this lists attributes associated with the lm() object "fit"
fit$coefficients
fit$coefficients[2]
fit$coefficients[[2]]

# This is the syntax you need to use the coefficients in calculations.
# For instance, here are the residuals "by hand", side-by-side with those stored in "fit":
res<-rate-(fit$coefficients[[2]]*year+fit$coefficients[[1]])
cbind(res,fit$residuals)

# plot the residuals:
dev.set(3)
plot(year,res,type='p')

# yet another way:
residuals(fit)

# summary() with lm objects:
summary(fit)

# plotting residuals
plot(year,fit$residuals,type='p')

# Now, here are two ways to plot the regression line, the first "by hand"
x<-seq(0,5,by=1)
y<-fit$coefficients[[2]]*x+fit$coefficients[[1]]
plot(year,rate)
points(x,y,type='l') # the line of best fit

# Or, using abline(fit):
plot(year,rate)
abline(fit)








###############################
###    Confidence Intervals ###
###############################


# First: normal quantile plots to check the assumption of normality
y <- rnorm(200)
qqnorm(y)      


# Confidence intervals:
# Intervals constructed so as to have a given (estimated) probability of containing the true value of the parameter.
# For example, with a normal distribution "by hand",
# using qnorm to give the needed z-score, and assuming sigma is known:
confidence_level <- .95
x_bar <- 5 
sigma <- 2 
n <- 20
error1 <- qnorm(1-(1-confidenceLevel)/2) * sigma/(n^.5)
error1
normal_ci <- c(x_bar - error1, x_bar + error1)

# Confidence intervals with a t-distribution (sigma estimated with s)
error2 <- qt(1-(1-confidence_level)/2,df=n-1)*s/(n^.5)
left <- a - error2
right <- a + error2
t_ci <- c(left,right)
rbind(normal_ci,t_ci)


# Now, using an actual data set:
# calculate the 95% confidence interval
w1 <- read.csv(file='/Users/David/Documents/gitHub/References-and-Illustrations/otherFiles/w1.csv',head=TRUE)
attributes(w1)
summary(w1)

n <- length(w1$vals)
m <- mean(w1$vals)
s <- sd(w1$vals) # unbiased estimator of sigma

error <- qt(.975,df=n-1)*s/n^.5
left <- m - error
right <- m + error
CI <- cbind(left,right)
colnames(CI) <- c("LOWER","UPPER")
CI


# Suppose you need to calculate many confidence intervals simultaneously,
# where the parameter to be estimated is a mean difference.
# Put all the necessary info for one of each pair of samples
# group into one set of vectors m1 and the others into another set.
# Then calculate the (pooled) standard error of the means in each comparison.
# (Remember, var(x+y)=var(x-y)=var(x)+var(y), for independent x,y)
# Then calculate the error vector showing errors for all comparisons,
# Using the pmin command to find degrees of freedom for each comparison.

means1 <- c(10,12,30)
means2 <- c(10.5,13,28.5)
sd1 <- c(3,4,4.5)
sd2 <- c(2.5,5.3,3)
n1 <- c(300,210,420)
n2 <- c(230,340,400)

# pooled standard error, vectorized:
se_pooled <- sqrt(sd1^2/n1+sd2^2/n2)
se_pooled

# confidence intervals
errors <- qt(.975,df=pmin(n1,n2)-1)*se_pooled
lowers <- (means1 - means2) - errors
uppers <- (means1 - means2) + errors
mean_diffs <- means1 - means2
rbind(lowers,mean_diffs,uppers)


################################
###     Calculating p-values ###
################################

# a vector of 10 iid normal random variables
n <- 10
mean <- 5
sigma <- 2
x <- rnorm(n,mean,sigma)
stripchart(x)

# standardized z-score, assuming sigma known
x_bar <- mean(x)
z_score <- (x_bar-mean)/(sigma/(sqrt(n)))

# the two-tailed p-value associated with that z-score,
# that is, the probability of obtaining a z-score at least as extreme (in either direction)
p_value <- 2*(1-pnorm(abs(z_score),0,1))
cbind(z_score,p_value)



# Using the data set w1, test the hypotheses that mean = 0.7:
# That is, find the p-value associated with the sample mean x_bar
# on the assumption that the population mean = 0.7.

w1 <- read.csv(file="/Users/David/Documents/gitHub/References-and-Illustrations/otherFiles/w1.csv")
summary(w1)

n <- length(w1$vals)
x_bar <- mean(w1$vals)
s <- sd(w1$vals) 
se_mean <- s/(sqrt(n))
z <- (x_bar - .7)/se_mean


# Now, we need to use the t-distribution to calculate the p-value
# of this standardized score:
p_value <- 2*(1-pt(abs(z),df=n-1))
cbind(.7,x_bar,se_mean,z,p_value)  # sanity check


# Calculating p-values for several differences of means:
means1 <- c(10,12,30)
means2 <- c(10.5,13,28.5)
mean_diffs <- means1 - means2
sd1 <- c(3,4,4.5)
sd2 <- c(2.5,5.3,3)
n1 <- c(300,210,420)
n2 <- c(230,340,400)
se_mean_diffs <- sqrt(sd1^2/n1+sd2^2/n2) # an unbiased estimate

# now a vector of t-scores
t <- (mean_diffs)/se_mean_diffs
t

# and a vector of conservative degrees of freedom
df=pmin(n1,n2)-1
df

# now a vector of one-tailed p_values
p_values_1 <- pt(-abs(t),df=pmin(n1,n2)-1)
p_values_1

# alternatively, 
1-pt(abs(t),df=df)

## Or, two-tailed,
p_values_2 <- 2 * (pt(-abs(t),df=df))

# summarizing results:
cbind(mean_diffs,se_mean_diffs,t,df,p_values_2)




# t.test():
# As an alternative to doing everything by hand, use t.test
# Takes a vector and by default tests for mean=0:
x <- c(9.0,9.5,9.6,10.2,11.6)
t.test(x)

# You can use a different mean (mu) value to test against:
t.test(x,mu=10)

# One-sided,
t.test(x,mu=10,alternative='less')



## To test for a difference in means between two independent samples,
x <- c(9.0,9.5,9.6,10.2,11.6)
y <- c(9.9,8.7,9.8,10.5,8.9,8.3,9.8,9.0)
t.test(x,y)

# Now, checking the "by hand" method against t.test:
x <- c(9.0,9.5,9.6,10.2,11.6)
y <- c(9.9,8.7,9.8,10.5,8.9,8.3,9.8,9.0)

x_bar <- mean(x)
y_bar <- mean(y)
sd_x <- sd(x)
sd_y <- sd(y)
n_x <- length(x)
n_y <- length(y)

mean_diff <- x_bar - y_bar
se_mean_diff <- sqrt(sd_x^2/n_x + sd_y^2/n_y)
t <- mean_diff/se_mean_diff
df <- min(n_x,n_y) - 1

# now the two-tailed p-value:
p_value_2 <- 2 * pt(-abs(t),df=df)
cbind(x_bar,y_bar,mean_diff,se_mean_diff,t,df,p_value_2)

# alternatively, t.test():
ttest <- t.test(x,y)
ttest

# comparing results side-by-side:
# first, pull attributes out of t.test() object
attributes(t.test(x,y))
ttest$estimate["mean of x"]
ttest$statistic[["t"]]
ttest$parameter[["df"]]
ttest$p.value

by_hand <- rbind(x_bar
                 ,y_bar
                 ,mean_diff
                 ,t
                 ,df
                 ,p_value_2)

t_test <- rbind(ttest$estimate[["mean of x"]]
                ,ttest$estimate[["mean of y"]]
                ,ttest$estimate[["mean of x"]]-ttest$estimate[["mean of y"]]
                ,ttest$statistic[["t"]]
                ,ttest$parameter[["df"]]
                ,ttest$p.value)

cbind(by_hand,t_test) 
# The slight discrepancy in p-value owing to more conservative df estimate by hand.
# Substituting 6.78 for df in the by-hand calculation yields the expected result:
p_value_2_2 <- 2 * pt(-abs(t),df=6.78)
cbind(p_value_2_2,ttest$p.value)

# More:
help(t.test)

~~~~
