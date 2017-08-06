## Chapter 3 Linear models for regression

### 3.3 Bayesian Linear Regression
#### 3.3.2 Predictive distribution
* (pp.158) "If we used localized basis functions such as Gaussians, then in regions away from the basis function centres, the contribution from the second term in the predictive variance (3.59) will go to zero, leaving only the noise contribution $\beta^{-1}$. Thus, the model becomes very confident in its predictions when extrapolating outside the region occupied by the basis functions, which is generally an undesirable behaviour. This problem can be avoided by adopting an alternative Bayesian approach to regression known as a Gaussian process."

#### 3.3.3 Equivalent kernel
**!** Not sure I understand the essense of this section.

### 3.4 Bayesian Model Comparison (pp. 161 - 165)
* training set $D$, model $M_i$, wish to evaluate the posterior $$p(M_i|D) \propto p(M_i)\cdot p(D|M_i)$$ where the last term is called *model evidence*
* Model could be selected by comparing *Bayes factor*, which is the ratio of the model evicence.
* (3.73) The expected Bayes factor is related to the *KL divergence*
* "In practice, it will be wise to keep aside an independent test set of data on which to evaluate the overall performance of the final system."
