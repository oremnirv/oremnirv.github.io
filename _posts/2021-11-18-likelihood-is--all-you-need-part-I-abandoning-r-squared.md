


---
layout: post
comments: true
title:  "Likelihood is all you need part I: abandoning R sqaured"
excerpt: "A reason to stop using R squared and replacing it with the likelihood"
date:   2011-11-29 
---


1. $R^2$ *or the log-likelihood?*


**Info:** This post may be of interest to scholars and practitioners who have already used/heard about the $R^2$ metric and are familiar with maximum likelihood estimation.

$R^2$ **is useful when your model outputs fixed uncertainty. But log-likelihood is broader —- it can also deal with outputs with non fixed uncertainty!**

Let’s take a concrete example — this will help us show that log-likelihood is a broader measure than $R^2$. Imagine we have the following test set (data points we have hidden away to assess our model):

![image-title-here](/figures/post_r_squared/a5.jpg)
**Figure 1.0**

which represent the factitious temperatures (°C) measured over five consecutive days in London in November 2002. $m+5$ is the size of our full dataset, $m$ being the size of the training set and $m+1,... m+5$ the indices of the test set. For conciseness, we introduce some mathematical notation: take the recorded temperature values at date $t = m+1...m+5$ to be denoted by $y_t \in R$; e.g, $y_{m+3} = 5.7$ is the temperature recorded on 14-11-02. Our prediction task is shown in figure [1.1](#_page1_x110.55_y92.49), where we are given the pairs $(t = 1,y_1),...(t = m,y_m)$ as training data and we are asked to predict the question marks for $t = [m+1,..., m+5]$.

![image-title-here](/figures/post_r_squared/a1.jpg)
**Figure 1.1**

We have also constructed a model to predict those temperature values in the test set. Let’s denote the predicted temperature values as $y_t \in R$. Constructing a table with the math notation and inserting the predicted values we have:

![image-title-here](/figures/post_r_squared/a0.jpg)
**Figure 1.2**



and we can use the values in the table and just plug them in the formula for $R^2$:

$$
 1 - \dfrac{\dfrac{1}{N} \sum_{t} (y_{t} - \hat{y_{t}})^{2}}{\dfrac{1}{N} \sum_{t} (y_{t} - \bar{y})^{2}} 
$$


where $\bar{y}$ the average of all the recorded yt values used for training our model. In figure [1.2](#_page2_x110.55_y176.62) we can see the denominator terms in the $R^2$, and in figure [1.3](#_page3_x110.55_y34.85) both the denominator and numerator terms.

![](/figures/post_r_squared/a2.jpg)
**Figure 1.3**

Let’s explore the terms in the formula for $R^2$ through a probabilistic lens. Then, we can readily see what is $R^2$ useful for and what it is not. The numerator of the second term in the formula, namely $\sum_{t} \dfrac{1}{N} (y_{t} - \hat{y_{t}})^{2}$, is the mean squared error (MSE). The MSE can be viewed as a recipe to assess errors, in which the square operation is placed to avoid errors canceling each other; or, it can be viewed probabilisticly as the result of minimising the negative log-likelihood of the variance term in the following model (call it M1):

$y_t \sim \operatorname{Normal}(f(t), σ^2)$

where we chose to model the mean temperature values $f(t)$ based only on
time $t$. This result is not specific to our model, but to any model with a normally distributed random variable with a fixed variance. So, for these models we can write that MSE = $\(\min\limits_{\sigma^{2}}\) - Likelihood(y_1, \dots, y_m|M1)$ (hereafter Likelihood will be denoted with $\ell$). To show that the MSE is equal to the negative log-likelihood of the data, we provide a full derivation, starting from the definition of the log-likelihood.

![](/figures/post_r_squared/a4.jpg)
**Figure 1.4**

In the same manner we can think of the denominator of the second term representing the model (call it M2)

$y_t \sim  \operatorname{Normal}(\bar{y}, ρ^2)$

![](/figures/post_r_squared/a3.jpg)
**Figure 1.5**

and hence calculating $R^2$ is the reciprocal value to the maximum likelihood ratio between the fixed variances (in our case σ and ρ) of normally distributed random variables. In other words

$$R^{2} = 1 - \dfrac{\min\limits_{\sigma^{2}} -\ell(y_1, \dots, y_m|M1)}{\min\limits_{\rho^{2}} -\ell(y_1, \dots, y_m|M2)}$$


, and if you model your problem with a fixed variance, then $R^2$ is just another way to communicate the likelihood ratio. However, if you use a model with non fixed variance, for example $y_t \sim  \operatorname{Normal}(f(t), σ(t)^2)$, evaluating it with $R^2$ will completely ignore the fact that you have a non fixed variance. An example of such a model is Gaussian Process. Instead, **you can just use the likelihood of your model, rather than just plugging the values into the $R^2$ formula**.


