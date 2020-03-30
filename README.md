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

**Tip:** 'd' gives a density, 'p' gives a distribution (at least, more than if lower.tail is false, lower than is true).


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
 
 # 
 
 # 
 ``` 