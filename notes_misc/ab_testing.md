Notes from the Udemy class "Bayesian Machine Learning with Python: A/B Testing" by Lazy Programmer Inc. A somewhat useful refreshment on basic probability, hypothesis testing, and Bayes.

******
**Table of Content**
[toc]

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
