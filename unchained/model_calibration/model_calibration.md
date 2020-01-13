# Model calibration

Model calibration is used to align the output of a classification model to probability prediction, e.g., instances with .05 model score has approximately 5% of chance of being positive. Summaries from the two articles listed in references are as follows:

- In practice, model calibration should be conducted on out-of-sample set to avoid overfitting.
- There are two major algorithms for model calibration
  - [Platt/sigmoid scaling](https://en.wikipedia.org/wiki/Platt_scaling) is equivalent of training another logistic regression model on the (raw score, label) pairs, i.e., estimate $A$ and $B$ in the following
  $$p(y=1|x) = \dfrac{1}{1 + \exp{[Af(x)+ B]}}$$
  - Fit an [isotonic regression](https://scikit-learn.org/stable/modules/isotonic.html#isotonic) model on $f^{(i)}(x), y^{(i)}$ pairs
- [Reliability curve](https://scikit-learn.org/stable/auto_examples/calibration/plot_compare_calibration.html) is often used to measure how well the probabilistic predictions of a classifier is calibrated.
  - Note that instead of decile/ventile the whole sample, we divide the sample into pre-set score bucket, i.e., $[0.0-0.1],...,[0.9-1.0]$ and plot the mean `dep_var` with count/frequency at the bottom (see an [scikit-learn example](https://scikit-learn.org/stable/auto_examples/calibration/plot_compare_calibration.html)).
- In practice, the larger the calibration set, the better the results. Platt scaling works better when the calibration set is small. When the set is large, isotonic regression is often preferred as it is non-parametric (easy to fit) and works better.
- If the negative class is downsampled with probability $w$ before training, the raw model scores need to be calibrated by the following formula:
$$q = \dfrac{p}{p+(1-p)/w}$$



# References
- [知乎 - 模型校准calibration](https://zhuanlan.zhihu.com/p/101766505) by [霖霖霖](https://www.zhihu.com/people/lin-lin-lin-zhu-75/activities)
- [scikit-learn - 1.16. Probability calibration](https://scikit-learn.org/stable/modules/calibration.html)