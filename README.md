# Multi comparisons-for-hypotheses-testing
#### Implement Multiple comparisons for hypotheses in R  :
1)[Bonferroni correction](https://en.wikipedia.org/wiki/Bonferroni_correction) </br>

2)[Holm method](https://en.wikipedia.org/wiki/Holm%E2%80%93Bonferroni_method#:~:text=In%20statistics%2C%20the%20Holm%E2%80%93Bonferroni,powerful%20than%20the%20Bonferroni%20correction.) </br>
3)Simes method </br>
4)Using the criterion Controlling [Family-wise Error Rate](https://en.wikipedia.org/wiki/False_discovery_rate) </br>
5)Simultaneous confidence intervals using Bonferroni correction </br>


## Multiple comparisons for hypotheses testing

Sometimes, we would like to test our hypothesis on multiple coordinates, where each coordinate has its own meaning. For example we can look at
$$H_0:\mu=\mu_0\in\mathbb{R}^m$$
as $m$ test, and our $H_0$ would be
$$H_0:\cap_{i=1}^m \mu_{i}=\mu_{0,i}$$
Where $\mu_i$ is the $i^{th}$ coordinate of the vector $\mu$, this hypothesis is also called the global null.
Using our significance level $\alpha$ to test each hypothesis may lead to problems, because even though we have $\le\alpha$ doing a type I error in a single hypothesis, the chances could be much higher when we use the same $\alpha$ for all our partial hypotheses.
For example, if we'll take $\alpha=0.05,m=10$ and our hypotheses are independent coordinates (and identically distributes, e.g. each one consists of flipping a fair coin several times) then by comparing each coordinate to $\alpha$ we have a chance of
$$1-(1-0.05)^{10}\approx 0.4016$$
to do a type I error, which is way too big, so a different approach is needed.

As learned in class there are several procedures that were designed to address the multiple comparisons challenge. We will describe and implement the following (while mentioning and proving the simplest results):

+ Bonferroni correction
+ Holm method
+ Simes method

Using the criterion FWER.

#### Bonferroni correction:
The procedure is as follows:
Given $m$ hypotheses and significance level $\alpha$, test each one with significance level of $\frac{\alpha}{m}$.
It is straight forward to show that the probability of falsely rejecting the global null is less than $\alpha$ :

```math
\mathbb{P}_{H_0}(\cup_{j=1}^m p_j\le\frac{\alpha}{m})\le\sum_{j=1}^m \mathbb{P}_{H_0}(p_j\le\frac{\alpha}{m})\overset{p_j\overset{H_0}{\sim}U[0,1]}{=}\sum_{j=1}^m \frac{\alpha}{m}=\alpha
```
#### Holm procedure:
The algorithm goes as follows:

+ Sort the p-values $p_{(1)}\le\cdots\le p_{(m)}$
+ Denote the hypotheses accordingly $H_{0,(1)},\cdots,H_{0,(m)}$
+ For $i$ in $1$ to $m$:
     If $p_{(i)}\le\frac{\alpha}{m+1-i}$ reject $H_{0,(i)}$ and continue to the next iteration, otherwise:
     Reject $H_{0,(i)},\cdots,H_{0,(m)}$ and break the loop.

#### Simes procedure:
The algorithm goes as follows:

+ Sort the p-values $p_{(1)}\le\cdots\le p_{(m)}$
+ Denote $T_m=\min_{j}(p_{(j)}\frac{m}{j})$
+ Reject the global null if $T_m\le\alpha$




## Simultaneous confidence intervals using Bonferroni correction

A t-test confidence interval is of the form:
```math
CI^\alpha=[\bar{x}-t_{n-1,1-\frac{\alpha}{2}}\cdot\frac{S}{\sqrt{n}},\bar{x}+t_{n-1,1-\frac{\alpha}{2}}\cdot\frac{S}{\sqrt{n}}]
```
The algorithm goes as follows:

+ Sort the p-values $p_{(1)}\le\cdots\le p_{(m)}$
+ Denote $T_m=\min_{j}(p_{(j)}\frac{m}{j})$
+ Reject the global null if $T_m\le\alpha$
  
