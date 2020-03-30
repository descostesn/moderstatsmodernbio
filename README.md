# Modern Statistics for Modern Biology

Notes and supporting materials for the book Modern Statistics for Modern Biology that can be found [here](https://www.huber.embl.de/msmb/).


## Chapter 1: Generative models for Discrete Data

### 1.2 A real example

Introduction to Poisson distribution: https://www.youtube.com/watch?v=cPOChr_kuQs

*"A **discrete** distribution that desribes the number of events occuring in a **fixed time interval** or **region of opporunity**.*

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
