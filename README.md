# Modern Statistics for Modern Biology

Notes and supporting materials for the book Modern Statistics for Modern Biology that can be found [here](https://www.huber.embl.de/msmb/).


## Chapter 1: Generative models for Discrete Data

### 1.2 A real example (Poisson distribution)

Introduction to Poisson distribution: [https://www.youtube.com/watch?v=cPOChr_kuQs](https://www.youtube.com/watch?v=cPOChr_kuQs)

*"A **discrete** distribution that desribes the number of events occuring in a **fixed time interval** or **region of opporunity**."*

Assumptions:

1) **The rate at which events occur is constant**
2) The occurence of one event does not affect the occurence of a subsequent event (i,e events are **independents**)

Answers to the questions in the video in R:

```
# P(10 click-through sales in the first day)
dpois(q = 10, lambda = 12)

# p(At least 10 click-through sales on the first day), P(X >= 10). We have to use 9 here to reflect the "equal to")
# Note: lower.tail, if true probabilities are P[X <= x] otherwise P[X > x]
ppois(q = 9, lambda = 12, lower.tail = FALSE)

# p(More than one click-through sale in the first hour)
# If there is an average of 12 sales per day (lambda = 12), the average sale per hour is lambda = 12/24 = 0.5
ppois(q = 1, lambda = 0.5, lower.tail = FALSE)
```

**Tip:** 'd' gives a density (equal to), 'p' gives a distribution (at least, more than if lower.tail is false, lower than if true). A way to think about lower.tail = FALSE is to think that we want the 'upper tail'.


### 1.3.1 Bernoulli trials/1.3.2 Binomial success counts

**Note:** A Bernoulli distribution is a special case of binomial distribution. Specifically, when n=1 the binomial distribution becomes Bernoulli distribution.

Introduction to Binomial distribution: [https://www.youtube.com/watch?v=e04_wUoscBU&list=PLTNMv857s9WVzutwxaMb0YZKW7hoveGLS](https://www.youtube.com/watch?v=e04_wUoscBU&list=PLTNMv857s9WVzutwxaMb0YZKW7hoveGLS)

*"Number of successes in n trials."*

Assumptions:

1) Two potential outcomes per **trial**
2) The probability of **success** (p) is the same in all trials
3) The number of trials (n) is **fixed**
4) Each trial is **independent**

 Answers to the questions in the video in R:
 
 ```
 # p(All 10 men are color blind)
 0.08^10 or dbinom(x = 10, size = 10, prob = 0.08)
 
 # p(No men are color blind)
 0.92^10 or dbinom(x = 0, size = 10, prob = 0.08)
 
 # p(2 men are color blind)
 dbinom(x = 2, size = 10, prob = 0.08)
 
 # p(at least two men are color blind) or P(X >= 2). We set q=1 to reflect the equal to
 pbinom(q = 1, size = 10, prob = 0.08, lower.tail = FALSE) 
 
 # The expected number of colour blind men, the variance and standard deviation
 n=10
 p=0.08
 q=1-p
 sum(n*p)
 var=n*p*q
 sd = sqrt(var)
 ```
 
 ### 1.3.3 Poisson distributions
 
 **Note**: When the probability of success p is small and the number of trials n large, the binomial distribution B(n,p) can be faithfully approximated by a simpler distribution, the Poisson distribution with rate parameter 
λ=np.

Question 1.5

It is equal if rounding to 2 digits i.e. it is approximately equal

```
dbinom(x=0:12, size=10^4, prob=5*10^-4)
dpois(x=0:12, lambda=10^4*5*10^-4)
all.equal(round(dbinom(x=0:12, size=10^4, prob=5*10^-4),2), round(dpois(x=0:12, lambda=10^4*5*10^-4),2))
``` 









---------------------------

Exercises

## ------------------------------------------------------------------------
dbinom(2, size = 10, prob = 0.3)
pbinom(2, size = 10, prob = 0.3)
sum(sapply(0:2, dbinom, size = 10, prob = 0.3))

## ----poismax, echo = FALSE-----------------------------------------------
poismax = function(lambda, n, m) {
  epsilon = 1 - ppois(m - 1, lambda)
  1 - exp( -n * epsilon)
}

## ----posimaxout----------------------------------------------------------
poismax(lambda = 0.5, n = 100, m = 7)
poismax(lambda = mean(e100), n = 100, m = 7)

## if (!requireNamespace("BiocManager", quietly = TRUE))
##     install.packages("BiocManager")
## BiocManager::install(c("Biostrings", "BSgenome.Celegans.UCSC.ce2"))

## ----BSgenome.Celegans.UCSC.ce2, message=FALSE, warning=FALSE------------
library("BSgenome.Celegans.UCSC.ce2")
Celegans
seqnames(Celegans)
Celegans$chrM
class(Celegans$chrM)
length(Celegans$chrM)

## ----Biostr--------------------------------------------------------------
library("Biostrings")
lfM = letterFrequency(Celegans$chrM, letters=c("A", "C", "G", "T"))
lfM
sum(lfM)
lfM / sum(lfM)

## ----rf------------------------------------------------------------------
t(rmultinom(1, length(Celegans$chrM), p = rep(1/4, 4)))

## ----lengthM-------------------------------------------------------------
length(Celegans$chrM) / 4

## ----oestat--------------------------------------------------------------
oestat = function(o, e) {
  sum((e-o)^2 / e)
}
oe = oestat(o = lfM, e = length(Celegans$chrM) / 4)
oe

## ----oesim---------------------------------------------------------------
B = 10000
n = length(Celegans$chrM)
expected = rep(n / 4, 4)
oenull = replicate(B,
  oestat(e = expected, o = rmultinom(1, n, p = rep(1/4, 4))))

## ----resundernull, echo=FALSE--------------------------------------------
hist(oenull, breaks = 100, col = "skyblue", main="")

## ----assert, echo = FALSE------------------------------------------------
stopifnot( oe/10 > max(oenull) )
