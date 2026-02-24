---
title: "Lecture 10 Notes - Regularization"
description: "Stat153/248 Time Series - Lecture 10 Notes"
author:
  - name: "Liberty Hamilton"
exports:
  - format: pdf
    template: /Users/liberty/Documents/Berkeley/Teaching/plain_latex
    output: ./Lec10_Notes.pdf
  - format: tex
    output: ./Lec10_Notes.tex
---

* **Reading**: Chapter 6 (and 6.2) - An Introduction to Statistical Learning

# Improving upon the linear model

For multiple linear regression we have seen $Y = \beta_0 + \beta_1 X_1 + \dots + \beta_p X_p + \epsilon$. If the true relationship between the response and predictors is approximately linear, then the least squares estimates will have low bias. In addition, if $n >> p$ (we have many more time points than parameters), then the least squares estimates will tend to have low variance and should perform well on test observations. 

If $n$ is not much larger than $p$, we can have a lot of variability in the least squares fit, which will result in overfitting. This generally means that we will not be able to predict future observations well. 

If $p > n$, then there are more coefficients $\beta_j$ to estimate than there are observations from which to estimate them. This means we cannot and should not fit multiple linear regression using least squares. We will have to take into consideration other methods for high dimensional data. In this case, there is no longer a unique least squares estimate, and there are infinitely many solutions. These least squares solutions can give zero error on the training data, but will be very poorly performing on new data due to high variance. Again, this is overfitting.

# What is overfitting?

Overfitting is seen when we fit a model that learns to predict our data perfectly, but does not generalize to new data. It follows the *noise* too closely. As model flexibility (and number of parameters) increases, we can pick up patterns in the data by random chance rather than by some true relationship.

We can look at an example in the notebook that accompanies this lecture. 

# Cross-validation

How can we avoid overfitting? One way is to seek models that minimize the MSE on *held out data*, that is, data that was not used to train our model. 

How do we choose this in practice? For cross validation in general, we might choose to use something like *k-fold cross validation*, which is where we split the data into *k* chunks, train on $k-1$ of those sets, and compute test error based on the last fold. However, in time series there are a few things to consider:

* Often we want to train on past data to predict future data (so randomly permuting time doesn't make sense)
* We also need to preserve the autocorrelation structure in time series data, so we should not randomly choose some percentage of observations. We want to chunk the data in a way that maintains the temporal order in the fitting.

In the past, cross-validation was not used as it was computationally prohibitive to test many possible training/test splits of the data. Nowadays this is not an issue, and cross-validation can be a very clean way to test model performance without requiring: 

* normally distributed errors
* homoscedasticity
* correct model specification
* known numbers of parameters

It works for any model without you needing to know anything about the error distribution. This is why this is more popularly used now as compared to parametric approaches such as AIC and BIC (which assume Gaussian errors).

# Alternative fitting procedures

So what do we do in these cases where we have potentially many parameters and few observations, but we want an accurate and interpretable model? We can constrain or *shrink* the coefficients to reduce the variance of our estimates at the cost of slightly increasing bias. This also can allow for improved model interpretability - by forcing some coefficients to be very small or to zero, we can more easily interpret our model by removing irrelevant covariates.

We will discuss these next time when we talk about *ridge regression* and *LASSO regression*, also called *L2* and *L1* regularization, respectively.

