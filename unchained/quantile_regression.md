# Quantile Regression (Pinball Loss Function)

## Background

In Prof. Hyndman's [podcast episode](https://robjhyndman.com/seminars/curious-quant-podcast/) for the [Curious Quant](https://podcasts.apple.com/us/podcast/the-curious-quant/id1481550488), he talked about loss functions and evaluation methods for forecasting models. This note follows up on the this topic and provides key concepts a list of references for further reading.

## Outline
According to [https://en.wikipedia.org/wiki/Quantile_regression](https://en.wikipedia.org/wiki/Quantile_regression),

> Whereas the method of least squares estimates the *conditional mean* of the response variable across values of the predictor variables, quantile regression estimates the *conditional median (or other quantiles)* of the response variable.

Quantile regression employs alternative measures of central tendency and statistical dispersion of the response variable, hence provides a more comprehensive view of relationships between variables.

## Mathematical formulation

### Quantiles

Let $\tau$ by the quantile (of a random variable $Y$) of interest, and define the Pinball loss function

$$\rho_\tau(y)=y(\tau-\mathbb{I}_{y<0})$$

Then the quantile can be found be minimizing the expected loss of $Y-\mu$ w.r.t. $\mu$:

$$\min_{\mu} \mathbb{E}[\rho_\tau(Y-\mu)]=\min_\mu\{(\tau-1)\int_{-\infty}^{\mu}(y-\mu)dF_Y(y) + \tau\int_u^{\infty}(y-\mu)dF_Y(y)\}$$

By setting the derivative of the expected loss function to $0$ and letting $q_\tau$ be the solution, we find

$$F_Y(q_\tau)=\tau$$

### Quantile regression

Suppose the $\tau$-th conditional quantile function is $Q_{Y|X}=X\beta_\tau$. Given the distribution function of $Y$. $\beta_\tau$ can be obtained by solving 

$$\beta_\tau = \argmin_{\beta\in\mathbb{R}^k} \mathbb{E}[\rho_\tau(Y-X\beta)]$$

The minimization problem can be reformulated as a linear program

$$\min_{\beta, \mu^+, \mu^- \in \mathbb{R}^k\times\mathbb{R}_+^{2n}}\{ \tau 1_n'\mu^+ +(1-\tau)1_n'\mu^- | X\beta+\mu^+ - \mu^- = Y \}$$

where

$$\mu_j^+=\max(\mu_j, 0), \quad \mu_j^-=\min(\mu_j, 0)$$

## Extra notes
- How to evaluate forecasting models that take the form of quantile regression?
  - Inferential statistics apply to quantile regression. Under the null hypothesis that $\hat{\tau}=\tau$, whether or not a given future realization fulfills $y_i<\hat{y}_i$ follows $\text{Bern}(\tau)$, and a batch of $y_i$'s follows $\text{Bin}(n, \tau)$
  - Check [this thread](https://stats.stackexchange.com/questions/213050/scoring-quantile-regressor) of [stats.stackexchange](https://stats.stackexchange.com/) for further discussion of employing Value-at-Risk alike metrics to measure the quality of the model in the context of time-series and forecasting.
- How to conduct cross-validation for time-series problems?
  - Evaluation on a rolling forecasting origin
  - Averaging n-step-ahead forecasts
  - Check [Cross-validation for time series](https://robjhyndman.com/hyndsight/tscv/) for details.


## References
- Koenker, Roger, and Kevin F. Hallock. 2001. "Quantile Regression." Journal of Economic Perspectives, 15 (4): 143-156.
- [Wikipedia/Quantile regression](https://en.wikipedia.org/wiki/Quantile_regression)
- [STAT 593 Quantile regression slides](http://josephsalmon.eu/enseignement/UW/STAT593/QuantileRegression.pdf) by Prof. Salmon
- [stats.stackexchange/scoring-quantile-regressor](https://stats.stackexchange.com/questions/213050/scoring-quantile-regressor)
- Prof. Hyndman's posts
  - [Cross-validation for time series](https://robjhyndman.com/hyndsight/tscv/)
  - [A note on the validity of cross-validation for evaluating autoregressive time series prediction](https://robjhyndman.com/publications/cv-time-series/)

