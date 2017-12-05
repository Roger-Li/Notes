Notes from the Udemy class "Bayesian Machine Learning with Python: A/B Testing" by Lazy Programmer Inc. A somewhat useful refreshment on basic probability, hypothesis testing, and Bayes.

******
**Table of Content**
[toc]

******
## Sec 1: Introduction
There is nothing to say here.

******
## Sec 2: Bayes Rule and Probability Review

### Manipulate Bayes Rule

We have the bayes rule as follows:
$$p(A|B)=\dfrac{p(B|A)P(A)}{P(B)}$$
If we don't have $p(B)$ directly, we can calculate it from the numerator, i.e.
$$P(B) = \int_A P(A,B) = \int_Ap(B|A)P(A)$$

We can think of the bottom term as a "normalization constant" so that the distribution sums up to 1
$$p(A|B)=\dfrac{p(B|A)P(A)}{\int_Ap(B|A)P(A)} = \dfrac{p(B|A)P(A)}{Z} \propto p(B|A)P(A)$$
as $Z$ is independent of $A$. This could be useful if we want to optimize over $A$, i.e.,
$$\text{argmax}_A P(A|B) = \text{argmax}_A P(B|A) P(A)$$

### CTR and Bernoulli distribution
Think of click through rate (CTR) as coin tosses, which has a [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution) for each click/don't click choice $Bern(p)$. Let $H= $click, $T=$ No click, their corresponding # be $N_H$ and $N_T$. The likelihood is
$$L(N_H, N_T) = p^{N_H}(1-p)^{N_T}$$
To estimate $p$, we maximize the log likelihod
$$
L = \log[L(N_H, N_T)]=N_H\log p + N_T\log(1-p)
$$
so $$\dfrac{\partial L}{\partial p} = \dfrac{N_H}{p} -\dfrac{N_T}{1-p} =0 \Longleftrightarrow p = \dfrac{N_H}{N_H+N_T}$$

### Central Limit Theorem
* If $X_i$'s are $iid$ random variables with $E[X_i] = \mu, Var[X_i]=\sigma^2<\infty$. Let $\bar{X} = \frac{1}{N}\sum_{i=1}^{N}X_i$, CLT states that $$\bar{X} \xrightarrow[]{d}N(\mu,\frac{\sigma^2}{N})$$

Note that in practice, we don't actually know $\sigma$, instead we could approximate it by
$$\hat{\sigma}=\sqrt{\frac{1}{N}\sum_{i=1}^{N}(X_i-\bar{X})^2}$$

### Confidence Intervals
#### Gaussian approximated CI
* Note that $\frac{\sqrt{N}}{\hat{\sigma}}(\bar{X}-\mu)\sim N(0,1)$ approximately (As we are using the impirical variance)

* Let $\hat{\mu} = \bar{X}, \hat{\sigma}=\sqrt{\frac{1}{N}\sum_{i=1}^{N}(X_i-\bar{X})^2}$. Given $X_1, ..., X_N$, we calculate the level -$\alpha$ (e.g., $.05$) C.I. of $\mu$ as
$$[\hat{\mu} + z_{\alpha/2}\frac{\hat{\sigma}}{\sqrt{N}}, \hat{\mu} + z_{1-\alpha/2}\frac{\hat{\sigma}}{\sqrt{N}}]$$Where $z_{\alpha/2} = \Phi^{-1}(\alpha/2)$ and $z_{1-\alpha/2}= \Phi^{-1}(1-\alpha/2)=-z_{\alpha/2}$

#### Non-approxiated CI with Student's $t$-distribution
* To find the non-approximated version, we need to use [$t$-distribution](https://en.wikipedia.org/wiki/Student%27s_t-distribution). Specifically, let $S^2=\frac{1}{N-1}\sum_{i=1}^{N}(X-\bar{X})^2$, the exact distribution of $\dfrac{\bar{X}-\mu}{S/\sqrt{N}}$ is a Student's $t$-distribution with $N-1$ degrees of freedom.
* Note the difference between $S^2$ and $\hat{\sigma}^2$

## Sec 3: Traditional A/B Testing
### Test the Mean Difference Between Two Groups
#### t Test
Assuming all data are $iid$ Gaussian, we have $X_1 = \{x_1^{(1)}, ..., x_1^{(N_1)}\}, X_2 = \{x_2^{(1)}, ..., x_2^{(N_2)}\}$, and we want to test whether or not group 1 is different than group 1 on average
* Null hypothesis $H_0: \mu_1 - \mu_2 \neq 0$
* The test statistic $t=\dfrac{(\bar{X_1}-\bar{X_2})-(\mu_1 - \mu_2)}{\sqrt{\frac{S_1^2}{N_1}+\frac{S_2^2}{N_2}}}$
* If the null hypothesis is true, $t\sim T_{N_1+N_2-2}$
* The C.I. for the difference in means $\mu_1 - \mu_2$ is given by $(\bar{X_1}-\bar{X_2}) \pm t^*\sqrt{\frac{S_1^2}{N_1}+\frac{S_2^2}{N_2}}$ where $t^*$ is the $1-\alpha/2$ (e.g., $0.975$) critical value.

#### Pooled t Procedure
Note that this is to assume that the two group has different variance, if we know that the two groups have the same variance (standard deviation), we use the [*pooled t procedure*](http://www.stat.yale.edu/Courses/1997-98/101/meancomp.htm) where we calculate the *pooled sample variance* $$S_p^2=\frac{(N_1-1)S_1^2+(N_2-1)S_2^2}{N_1+N_2-2}$$, and the test statistic becomes $$t=\dfrac{(\bar{X_1}-\bar{X_2})-(\mu_1 - \mu_2)}{S_p\sqrt{\frac{1}{N_1}+\frac{1}{N_2}}}$$

#### P-Value
* **Definition:** The probability of obtaining a result equal to or 'more extreme' than what was actually observed, when the null hypothesis is true.
* Since we use a significance level $\alpha$, we reject the null hypothesis if p-value $\leq \alpha$

#### Distribution-free Tests
* If we don't want to assume Gaussian or any distribution, we could apply non-parametric tests such as
    * [Kolmogorov-Smirnov test](https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test)
    * [Kruskal-Wallis test](https://en.wikipedia.org/wiki/Kruskal%E2%80%93Wallis_one-way_analysis_of_variance)
    * [mann-Whitney U test](https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U_test)
* Less assumptions $\rightarrow$ less power $\rightarrow$ need "more extreme" observed difference for statistically significant p-value.

### A/B Test for CTR (Chi-squared Test)
* [$\chi^2$ test](https://en.wikipedia.org/wiki/Chi-squared_test) can be used to attempt rejection of the null hypothesis that the data are independent.
* See the [wikipedia example](https://en.wikipedia.org/wiki/Chi-squared_test#Example_chi-squared_test_for_categorical_data)
* Example of A/B testing, would like to know if Advertisement A and B are statistically significantly different.

|   | Click           | No Click | Click + No Click|
|--- | --- | --- | --- |
| Advertisement A     | **36**       | 14 | 50 |
| Advertisement B     | 30       | 25 | 55 |
| Ad. A + Ad. B       | 66       | 39 | 105|
* Null hypothesis: click/no click is *independent* of advertisement A/B, i.e., no statistical difference between advertisement A/B.

* test statistic $\chi^2=\sum_i\frac{(observed_i-expected_i)^2}{expected_i}$
    * $i =$ (ad A, click)
    * $observed_i = 36$
    * $expected_i =$ (# of times ad A is shown)$\times p$(click) $=50\times \frac{66}{105} = 31.429$
* For $2\times 2$ table, shortcut $\chi^2=\frac{(ad-bc)^2(a+b+c+d)}{(a+b)(c+d)(a+c)(b+d)}$
* Degree of freedom $=$(# of rows $-1$)(# of columns $-1$)
* "Extream" is when $\chi^2(data)$ is large (far away from expected). $1-CDF(\chi^2(data))$ is the p-value. If p-value $< \alpha$, we reject the null hypothesis and claim that A and B are significantly different.

### Bonferroni Correction (A/B/C/D/... Testing)
* Post Hoc Testing
    * Parewise testing: confidence level changes from $1-\alpha \rightarrow 1-\alpha/m$ where $m={{N}\choose{2}}$
    *  One-vs-the-rest testing: $1-\alpha \rightarrow 1-\alpha/N$

### Statistical Power
* Power = sensitivity = $P$(reject $H_0|H_1$ is true)
* Some definitions and properties
    * Higher power decreases chances of FN ($=$ type 2 error)
    * $P$(FN) $=$ FN rate $=\beta$
    * Power $=1-\beta$
    * FP $+$ type 1 error


### A/B Testing Pitfalls
* Should not stop experiment early when it reaches significance
* Choosing Sample Size: general rule of thumb
$$N=16\frac{\sigma^2}{\delta^2}$$ where $\sigma^2$ is the variance of data, $\delta$ is the minimum difference you want to detect
* An example why A/B testing in Frequentist setting could be undesirable
New drug or landing page is working well, but you cannot stop the test to launch it early because it increases the chance of FPs. Remember, p-value is the probability that observations show difference even if they are actually the same (i.e. null hypothesis is true), and it can also pass below the threshold and go back up over time.

******

## Sec 4: Bayesian A/B testing
### Epsilon-Greedy algorithm
* one way to solve the exploration-exploitation dilemma, also used in reinforcement learning
* Choose a small number $\epsilon \in (0,1)$ and let it be the probability of exploration, i.e. choose at random. With probability of $1-\epsilon$ we exploit , i.e., choose the current-best arm ([multi-armed bandit](https://en.wikipedia.org/wiki/Multi-armed_bandit)).
* Note that the rate of exploration and exploitation are fixed. In the long run, the probability of choosing the best arm is $1-\epsilon/2$. In the case of CTR, if reward is $1$ for click and $0$ for no click, the loss is $\epsilon N/2$, which is $O(N)$.

### Upper Confidence Bound (UCB1) Algorithm
#### [Chernoff-Hoeffding's inequality](https://en.wikipedia.org/wiki/Hoeffding%27s_inequality)
Let $X_1,...,X_N$ be independent r.v.s in the range $[0,1]$ with $E[X_i]=\mu$, then for $\epsilon >0$, we have $$p(\hat{\mu }\geq \mu + \epsilon)\leq \exp(-2\epsilon^2N)$$ The opposite is also true, so
$$p(|\hat{\mu}-\mu|\leq \epsilon)>1-2\exp(-2\epsilon^2N)$$
#### UCB1 algorithm and properties
* For each arm $j$, choose with probability $$\epsilon_j=\sqrt{\frac{2\ln N}{N_j}}$$ Where $N=$ # of total games played so far, $N_j=$ total # times arm $j$ is played.
* For each pick, choose $$j^*=\text{argmax}(\hat{\mu}_j + \sqrt{\frac{2\ln N}{N_j}})$$
* First term obviously represents exploitation. The second term depends on both $N$ and $N_j$, if $N_j$ is low, we are not so confident about the estimate of $j$, thus explore it more.
* Since $\frac{\ln N}{N}\rightarrow 0$, we are approaching the true CTR estimate asymptotically.
* In the long run, the loss of UCB1 is $O(\ln N)$, which is much less than the Epsilon-Greedy algorithm.

#### Regret (expected loss)
The regret of a policy is defined as $$\mu^*N-\sum_{j=1}^{K}\mu_j\cdot E[T_j(N)]$$ where $\mu_j$ is the expected reward of arm $j$, $\mu^*$ is the expected reward of the optimal arm, $T_j$ is the # of times arm $j$ was played.

It represented the *expected loss* after $N$ plays due to the fact that the policy does not always play the optimal arm.

### Conjugate priors for CTR estimates ([*Beta* distribution](https://en.wikipedia.org/wiki/Beta_distribution))
* The likelihood for CTR is Bernoulli
$$P(X_1, ... X_N|\theta) = \prod_{i=1}^N \theta^{X_i}(1-\theta)^{1-X_i}$$
* The conjugate prior for Bernoulli is Beta $\theta \sim Beta(\alpha, \beta)=\frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}\theta^{\alpha-1}(1-\theta)^{\beta-1}$
* $E(\theta) = \frac{\alpha}{\alpha + \beta}$, the same as we get by MLE
* $Var(\theta) = \frac{\alpha \beta}{(\alpha\beta)^2(\alpha+\beta+1)}$, note that the following posterior updates indicate that the more data we collect, the bigger $\alpha$ and $\beta$ gets, and the smaller the variance gets.
* The posterior
$$P(\theta | X_1, ... X_N)\propto\theta^{\alpha-1+\sum_{i=1}^NX_i}(1-\theta)^{\beta-1+\sum_{i=1}^N(1-X_i)}\sim Beta(\alpha+\sum_{i=1}^NX_i, \beta+N-\sum_{i=1}^NX_i)$$
* In terms of CTR problem, the posterior is $Beta(\alpha+\text{\# of clicks}, \beta + \text{\# of no clicks})$
* Use $Beta(\alpha=1, \beta=1)$ as prior because it is equivalent to $U(0,1)$

### Bayesian A/B Testing with Thompson Sampling
* [Thompson Sampling](https://en.wikipedia.org/wiki/Thompson_sampling) "is a heuristic for choosing actions that addresses the exploration-exploitation dilemma in the multi-armed bandit problem. It consists in choosing the action that maximizes the expected reward with respect to a randomly drawn belief."
* Thompson sampling idea: pick action $j$ with probability that increases with the probability (under posterior distributions) that $j$ is optimal
```
while True:
    Draw a random sample from all bandits' current Beta(alpha_j, beta_j)
    j* = bandit that gives us maximum sample
    x = play bandit j*
    alpha_j* = alpha_j* + x
    beta_j* = beta_j* + 1 - x
```
* See [Python implementation](https://github.com/lazyprogrammer/machine_learning_examples/blob/master/ab_testing/bayesian_bandit.py) by lazyprogrammer.
* Bayesian method automatically converges to the maximum CTR, as demoed [here](https://github.com/lazyprogrammer/machine_learning_examples/blob/master/ab_testing/convergence.py). Intuitively, Thompson Sampling converges at exploiting the arm with the highest reward.

### Finding a Threshold Without P-Values
* Note that in the Frequentist setting, $1\%$ p-value for $H_0: \mu_1 > \mu_2$ does not mean $p(\mu_1 > \mu_2) = 99\%$, which could be counter-intuitive in practice.
* For two independent *Beta* distribution, $p(\mu_1,\mu_2) = p(\mu_1)p(\mu_2)$, thus $p(\mu_1>\mu_2)$ can be calculated as the area under $p(\mu_1,\mu_2)$ where $\mu_1>\mu_2)$, i.e.
$$p(\mu_1>\mu_2)=\sum_{i=0}^{\alpha_2-1}\dfrac{B(\alpha_1+i, \beta_1+\beta_2)}{(\beta_2+i)B(1+i, \beta_2)B(\alpha_1, \beta_1)}$$ where $B(\alpha, \beta)=\frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)}$. Note that the computation is $O(\alpha_2)$, not necessarily cheap.
* Another option is to define a loss function $$L=\max(\mu_2 - \mu_1,0)$$ and stop when $$E_{\mu_1,\mu_2}(L) < \text{threshold}$$
Use the previously computed $p(\mu_1>\mu_2)\equiv H(\alpha_1, \beta_1, \alpha_2, \beta_2)$, we have
$$E_{\mu_1,\mu_2}(L) = \frac{B(\alpha_1+1, \beta_1)}{B(\alpha_1, \beta_1)}H(\alpha_1+1, \beta_1, \alpha_2, \beta_2)-\frac{B(\alpha_2+1, \beta_2)}{B(\alpha_2, \beta_2)}H(\alpha_1, \beta_1, \alpha_2+1, \beta_2)$$

### Confidence Interval Approximation vs. Beta Posterior for CTR Estimation
* As shown in the [demo](https://github.com/lazyprogrammer/machine_learning_examples/blob/master/ab_testing/ci_comparison.py), the Gaussian approximated C.I. converges to the beta posterior distribution after collecting many samples, which is a demonstration of [CLT](https://en.wikipedia.org/wiki/Central_limit_theorem).

******
## Sec 5: More on Conjugate Priors
### Role a Dice
#### [Categorical distribution](https://en.wikipedia.org/wiki/Categorical_distribution)
#### Likelihood
$$p(X_1,...,X_N|\Theta)=\prod_{n=1}^N\prod_{k=1}^K\theta_k^{I(X_n=k)}$$
#### [Dirichlet prior](https://en.wikipedia.org/wiki/Dirichlet_distribution)
It works like a multivariate version of *Beta* distribution.
$$Dirichlet(\vec{a})=\frac{1}{B(\vec{a})}\prod_{k=1}^K\theta_k^{\alpha_k-1}$$ $$\vec{a}=(\alpha_1,...,\alpha_K)$$ $$B(\vec{a})=\dfrac{\prod_{k=1}^K\Gamma(\alpha_k)}{\Gamma(\sum_{k=1}^K\alpha_k)}$$
#### Posterior
$$p(\Theta|\vec{X})\propto\prod_{k=1}^K\theta_k^{\alpha_k-1+\sum_{n=1}^NI(X_n==k)}$$ i.e.,
$$p(\Theta|\vec{X})=Dirichlet(\vec{\alpha}_{k+1})$$ with
$$\alpha_{k+1}=\alpha_k+\sum_{n=1}^NI(X_n==k)=\alpha_k+\text{\# times }k\text{ appeared}$$

### Gaussian Likelihood with Known Variance
#### Gaussian Prior
With fixed variance/precision, the prior of the mean is also a Gaussian
$$X|\mu \sim N(\mu, \tau^{-1}), \quad  \mu\sim N(m_0,\lambda_0^{-1}) $$ Note that $\tau, \lambda$ are precisions (inverse of variance) here.
#### Posterior
$$p(\mu|X)\propto N(m=\frac{m_0\lambda_0+\tau\sum_{n}x_n}{\lambda_0+\tau N},\lambda^{-1}=(\lambda_0+\tau N)^{-1})$$ Note that $m\sim \frac{\sum X}{N}$ when $N$ is large, which converges to the sample mean.

******
## Resources
### UCB1 Algorithm
* [The Upper Confidence Bound Algorithm](http://banditalgs.com/2016/09/18/the-upper-confidence-bound-algorithm/) By [Tor Lattimore](http://banditalgs.com/author/tor/)
* [(Slides) The UCB Algorithm](https://webdocs.cs.ualberta.ca/~games/go/seminar/notes/2007/slides_ucb.pdf) By  Markus Enzenberger, on UCB1, UCB2, $\epsilon-$Greedy, UCB1-NORMAL, etc.
* [(Slides) Bandit Algorithms Continued: UCB1](https://www.cs.bham.ac.uk/internal/courses/robotics/lectures/ucb1.pdf) By Noel Welsh
### Thompson Sampling
* [(slides) CS294 - Thompson sampling](https://www.stat.berkeley.edu/~bartlett/courses/2014fall-cs294stat260/lectures/thompson-notes.pdf) By Peter Bartlett
* Russo et. al., [A Tutorial on Thompson Sampling](https://arxiv.org/pdf/1707.02038.pdf)
### Conjugate Priors
* [Table of conjugate distributions](https://en.wikipedia.org/wiki/Conjugate_prior#Table_of_conjugate_distributions)
