---
title: "Notes on A/B Testing (Udacity)"
author: Yuanzhe Li
date: 2020-02
output: pdf_document
linkcolor: blue
---

Notes on the [A/B Testing (Udacity)](https://classroom.udacity.com/courses/ud257) course.

- [Lesson 1: Overview of A/B Testing](#lesson-1-overview-of-ab-testing)
    - [1.15 Calculating confidence interval (CTR example)](#115-calculating-confidence-interval-ctr-example)
    - [1.17 Null and Alternative Hypothesis, *Two-tailed vs. One-tailed tests*](#117-null-and-alternative-hypothesis-two-tailed-vs-one-tailed-tests)
    - [1.19 Pooled Standard Error](#119-pooled-standard-error)
    - [1.21 - 24. Sample Size and Power](#121---24-sample-size-and-power)
    - [1.25 Pooled Example](#125-pooled-example)
    - [1.26 Confidence Interval Case Breakdown](#126-confidence-interval-case-breakdown)
- [Lesson 2: Policy and Ethics for Experiments](#lesson-2-policy-and-ethics-for-experiments)
    - [2.1 - 2.7. Four Principles](#21---27-four-principles)
    - [2.8 Accessing Data Sensitivity](#28-accessing-data-sensitivity)
    - [2.10 Summary of Principles](#210-summary-of-principles)
- [Lesson 3: Choosing and Characterizing Metrics](#lesson-3-choosing-and-characterizing-metrics)
- [Lesson 4: Designing an Experiment](#lesson-4-designing-an-experiment)
- [Lesson 5: Analyzing Results](#lesson-5-analyzing-results)
- [Lesson 6: Final Project](#lesson-6-final-project)
- [Reference](#reference)

# Lesson 1: Overview of A/B Testing


### 1.15 Calculating confidence interval (CTR example)
- Let's say we have $\hat{p} = \dfrac{X}{N} = \dfrac{100}{1000}=0.1$ where $X =$ # of users who clicked, and $N=$ # of users. 
- A *Rule of Thumb* for normality is to check $N\cdot\hat{p} > 5$ (might as well check $N\cdot(1-\hat{p}) > 5$), otherwise use $t$-distribution instead of $z$-distribution.
- The *margen of error* $m=z_{\alpha/2}\cdot SE = z_{\alpha/2} \cdot \sqrt{\frac{\hat{p}(1-\hat{p})}{N}}$, notice that here $SE = \sqrt{\frac{p(1-p)}{N}}$ instead of $\sqrt{np(1-p)}$ for binomial distribution, since we use the fraction or proportion of successes instead of the total number of sucesses.
- For $\alpha=5\%$, we have $m=z_{0.025}\cdot \sqrt{\frac{0.1*0.9}{1000}} = 0.019$, and the final 95\% CI is $[0.081, 0.119]$.


### 1.17 Null and Alternative Hypothesis, *Two-tailed vs. One-tailed tests*

The null hypothesis and alternative hypothesis proposed here correspond to a two-tailed test, which allows you to distinguish between three cases:
- A statistically significant positive result
- A statistically significant negative result
- No statistically significant difference.

Sometimes when people run A/B tests, they will use a one-tailed test, which only allows you to distinguish between two cases:
- A statistically significant positive result
- No statistically significant result

Which one you should use depends on what action you will take based on the results. 

If you're going to launch the experiment for a statistically significant positive change, and otherwise not, then you don't need to distinguish between a negative result and no result, so a one-tailed test is good enough. If you want to learn the direction of the difference, then a two-tailed test is necessary.

### 1.19 Pooled Standard Error 

- We have $X_{cont}, X_{exp}, N_{cont}, N_{exp}$, and 
- Pooled sample mean $\hat{p}_{pool} = \dfrac{X_{cont} + X_{exp}}{ N_{cont} + N_{exp}}$
- Pooled sample standard error $SE_{pool} = \sqrt{\hat{p}_{pool}(1-\hat{p}_{pool})(\frac{1}{N_{cont}}+\frac{1}{N_{exp}})}$
- Test statistic $\hat{d} = \hat{p}_{exp} - \hat{p}_{cont}$
- Null hypothesis $H_0: d = 0$, under which $\hat{d}\sim \mathcal{N}(0, SE_{pool})$
- For 95% confidence level ($z_{1-0.05/2} = 1.96$), if $\hat{d} > 1.96\times SE_{pool}$ or  $\hat{d} < - 1.96\times SE_{pool}$, reject the null.


### 1.21 - 24. Sample Size and Power
- Two types of error
  - $\alpha = P$(reject null | null True)
  - $\beta = P$(not reject null | null False)
  - So if sample is small, we have low $\alpha$ and high $\beta$, i.e., harder to identify the alternative when a difference exists. On the other hand, if sample is large, $\alpha$ is the same, but $\beta$ is much lower, as shown below.
    - Sample size = 1000
    
    ![small_sample](images/1_22_ctr_example_small_sample.png)
    - Sample size = 5000
    
    ![large_sample](images/1_22_ctr_example_large_sample.png) 

  - $1-\beta$ is called *sensitivity* and often choose to be $> 80\%$


- Note on power
  - Statistical textbooks often define power as the sensitivity. However, conversationally power often means the probability that your test draws the correct conclusions, which depends on both $\alpha$ and $\beta$.
- Required sample size to achieve certain statistical power can be calculated using [online calculator](https://www.evanmiller.org/ab-testing/sample-size.html), in which you need to specify $\alpha$, $\beta$, baseline conversion rate (null), and minimum detectable effect (alternative).
- Examples of factors that affect the required sample size are as follows:

    ![how many page views varies](images/1_24_factors_affecting_sample_size.png)

### 1.25 Pooled Example
An pooled example is shown below, notice how the $d_{min}$ works (need the lower bond of the $1-\alpha$ level CI $> d_{min} = 0.02$)

![pooled example](images/1_25_pooled_example.png)

### 1.26 Confidence Interval Case Breakdown
Shown below is the how we should consider the decision under varying CI and $d_{min}$ cases

![CI breakdown](images/1_26_CI_breakdown.png)


# Lesson 2: Policy and Ethics for Experiments

### 2.1 - 2.7. Four Principles
[IRB](https://en.wikipedia.org/wiki/Institutional_review_board)'s four main principles to consider when conducting experimentats are:
- **Risk**: *what risk is the participant undertaking?*. The main threshold is whether the risk exceeds that of *“minimal risk”*. Minimal risk is defined as the probability and magnitude of harm that a participant would encounter in normal daily life. 
- **Benefit**: *what benefits might result from the study?*
- **Choice/Alternatives**: *what other choices do participants have?*
- **Privacy/Data Sensitivity**: *what data is being collected, and what is the expectation of privacy and confidentiality?* 
  - How sensitive is the data?
  - What is the re-identification risk of individuals from the data?

### 2.8 Accessing Data Sensitivity
An example of data sensitivity assessment is shown below

![accessing data sensitivity](images/2_8_data_sensitivity.png)

### 2.10 Summary of Principles
- It's a grey area whether internet studies should be subject to IRB review or not and whether informed consent is required.
- Most studies face the bigger question about data collection with regards to identifiability, privacy, and confidentiality / security.
  - Are participants facing more than minimal risk?
  - Do participants understand what data is being gathered?
  - Is that data identifiable?
  - How is the data handled?








# Lesson 3: Choosing and Characterizing Metrics

# Lesson 4: Designing an Experiment

# Lesson 5: Analyzing Results

# Lesson 6: Final Project

# Reference
- [Evan's Awesome A/B Tools](https://www.evanmiller.org/ab-testing/)