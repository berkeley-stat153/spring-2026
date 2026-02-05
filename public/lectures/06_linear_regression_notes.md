---
title: "Lecture 6 Notes - Linear Regression Continued"
description: "Stat153/248 Time Series - Lecture 6 Notes"
author:
  - name: "Liberty Hamilton"
exports:
  - format: pdf
    template: /Users/liberty/Documents/Berkeley/Teaching/plain_latex
    output: ./Lec06_Notes.pdf
  - format: tex
    output: ./Lec06_Notes.tex
---

* **Reading**: Chapter 2 – Shumway and Stoffer

# Simple linear regression

Last time we spoke about regression models where we have two parameters, $\beta_0$ and $\beta_1$ that we use to estimate the relationship between our response variable $y$ and our covariate $x$:

$$y = \beta_0 + \beta_1 x + \epsilon$$

For example, $y$ is height of an adult and $x$ is the height of their parent, or $y$ is the price of chicken and $x$ is time.

## Revisiting OLS 

We also talked about ordinary least squares (OLS), which is where we want to solve:

$$Q = \sum_{i=1}^n (y_i - \hat{y}_i)^2 = \sum_{i=1}^n (y_i - \beta_0 - \beta_1 x_i)^2$$

We can do this by differentiating Q with respect to each parameter and setting it equal to zero:

First, for $\beta_0$:

$$
\begin{aligned}
\frac{\delta Q}{\delta \beta_0} &= \sum_{i=1}^n 2(y_i - \beta_0 - \beta_1 x_i)(-1) \\
&= \sum_{i=1}^n 2(\beta_0 + \beta_1 x_i - y_i)= 0\\
&= 2\sum_{i=1}^n \beta_0 + 2 \sum_{i=1}^n \beta_1 x_i - 2\sum_{i=1}^n y_i\\
&= 2n \beta_0 + 2n \beta_1 \bar{x} - 2n \bar{y} 
\end{aligned}
$$

$$\therefore \beta_0 = \bar{y} - \beta_1 \bar{x}$$


And for $\beta_1$:

$$
\begin{aligned}
Q &= \sum_{i=1}^n (y_i - \beta_0 - \beta_1 x_i)^2\\
&= \sum_{i=1}^n (y_i - \bar{y} + \beta_1 \bar{x} - \beta_1 x_i)^2\\
\frac{\delta Q}{\delta \beta_1} &= \sum_{i=1}^n 2(y_i - \bar{y} + \beta_1 \bar{x} - \beta_1 x_i)(\bar{x}-x_i)\\
&= 2 \sum_{i=1}^n \left[((y_i - \bar{y}) + \beta_1(\bar{x}-x_i) )(\bar{x}-x_i)\right]\\
&= -2\sum_{i=1}^n (y_i-\bar{y})(x_i-\bar{x}) + 2\beta_1\sum_{i=1}^n (x_i - \bar{x})^2
\end{aligned}
$$

Then it follows that:

$$
\begin{aligned}
\beta_1 &= \frac{\sum_{i=1}^n (y_i-\bar{y})(x_i-\bar{x})}{\sum_{i=1}^n (x_i-\bar{x})^2}
&= \frac{\operatorname{Cov}(x,y)}{\operatorname{Var}(x)}
\end{aligned}
$$

Notice now how our solution for the slope, $\beta_1$, is related to the covariance of $x$ and $y$ and the variance of $x$! 

Another note here is that to calculate $\bar{x}$ and $\bar{y}$, we often must use the sample means (e.g. across all time points), because we do not have multiple samples for each time point. 

## Maximum likelihood estimation (MLE)

We can also calculate these terms using MLE, In this case, we normally assume that the errors are normally distributed i.i.d, e.g.:

$$ \epsilon_1, \dots, \epsilon_n \overset{i.i.d}\sim N(0,\sigma^2)$$

With this assumption, we can rewrite our linear regression model as:

$y_i \overset{\text{independent}}\sim N(\beta_0 + \beta_1 x_i, \sigma^2)$

The likelihood is expressed as:

$f_{y_1,\dots, y_n | \beta_0,\beta_1,\sigma}(y_1,\dots,y_n) = \displaystyle\prod_{i=1}^n \frac{1}{\sqrt{2\pi}\sigma} \exp \left({-\frac{(y_i-\beta_0-\beta_1 x_1)^2}{2\sigma^2} }\right)$

To maximize the likelihood, we can maximize the log-likelihood, which is easier to deal with:

$$
\begin{aligned}
\text{log-likelihood} &= \displaystyle\sum_{i=1}^n \log \frac{1}{\sqrt{2\pi}\sigma} -\displaystyle\sum_{i=1}^n\frac{(y_i-\beta_0-\beta_1 x_1)^2}{2\sigma^2}\\
&= -\frac{n}{2}\log(2\pi) - n\log\sigma - \frac{1}{2\sigma^2}\displaystyle\sum_{i=1}^n (y_i-\beta_0-\beta_1 x_1)^2
\end{aligned}
$$

Since we know the sum of squared error $Q = \sum_{i=1}^n (y_i - \beta_0 - \beta_1 x_i)^2 = S(\beta_0,\beta_1)$, we can take the derivative with respect to unknown parameters $\beta_0, \beta_1$, and $\sigma$:

$$
\begin{aligned}
\frac{\delta}{\delta \beta_0} \text{log-likelihood} &= -\frac{1}{2\sigma^2}\frac{\delta}{\delta\beta_0}S(\beta_0, \beta_1) = 0 \implies \frac{\delta }{\delta \beta_0} S(\beta_0, \beta_1)= 0 \\
\frac{\delta}{\delta \beta_1} \text{log-likelihood} &= -\frac{1}{2\sigma^2}\frac{\delta}{\delta\beta_1}S(\beta_0, \beta_1) = 0 \implies \frac{\delta }{\delta \beta_1} S(\beta_0, \beta_1)= 0\\
\frac{\delta}{\delta \sigma} \text{log-likelihood} &= -\frac{n}{\sigma}+\frac{S(\beta_0, \beta_1)}{\sigma^3}=0 \implies \sigma = \sqrt{\frac{S(\beta_0, \beta_1)}{n}}
\end{aligned}
$$

These first two equations are the same as what we derived before. However, we now have a new term in which we get the MLE estimate for $\sigma$ from the third equation (with $\beta_0$ and $\beta_1$ replaced by $\hat{\beta}_0$ and $\hat{\beta}_1$, respectively):

$$\hat{\sigma}_{\text{MLE}} = \sqrt{\frac{S(\hat{\beta}_0,\hat{\beta}_1)}{n}}$$

From this, we can get $\hat{\sigma}^2_{\text{MLE}}=\frac{1}{n}S(\hat{\beta}_0,\hat{\beta}_1)$.

But as it turns out, this is a biased estimator (unlike those for $\beta_0$ and $\beta_1$, which are not! We often instead use a corrected, unbiased estimator of $\hat{\sigma}^2$ where we divide by $n-p$, where $p$ is the number of parameters estimated (here, $p=2$, one for $\beta_0$ and one for $\beta_1$). That is:

$$\hat{\sigma}^2_{\text{unbiased}}=\frac{S(\hat{\beta}_0,\hat{\beta}_1)}{n-2}$$

Let's look at a simulation to show how this happens!

Next time, we will look at how this extends to multiple linear regression. Later this bias will also become important when we think about regularization.

# A note on assumptions

* What assumptions have we made in the example regressions we have fit so far? **None!** In fact, we don't need to assume the true relationship between $y$ and $x$ is linear to perform linear regression.
* Sometimes it is useful to fit sample coefficients $\beta_0$ and $\beta_1$ and use them to make predictions, even if this model isn't fully correct or the "best" model.
* Often we may use linear models as they are convenient to interpret, not necessarily because they are the best or most correct model. 
* If we do want to perform inference, however, we require that many more assumptions are satisfied.
