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

### 3.5 The Evidence Approximation
#### 3.5.1 Evaluation of the evidence function
* The evidence function $p(\mathbf{t}|\alpha,\beta)$ (3.7.7) is evaluated using previous derived equations on conditional Gaussian, prior/likelihood of the linear models, and finally came as (3.86) in pp.167.
* Note how part of the evidence function $\mathbb{E(\mathbf{m}}_N)$ (3.82) is related to the cost function of ridge regression.
* *Figure* 3.14 shows how the model evidence versus the order $M$ plots could indicate the best model, similar to how we use plot of training/testing error errors (*Figure 1.5*, p.8) to choose the best/simple model.
