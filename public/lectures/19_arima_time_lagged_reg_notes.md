---
title: "Lecture 19 Notes - ARIMA Models"
description: "Stat153/248 Time Series - Lecture 19 Notes"
author:
  - name: "Liberty Hamilton"
exports:
  - format: pdf
    template: /Users/liberty/Documents/Berkeley/Teaching/plain_latex
    output: ./Lec19_Notes.pdf
  - format: tex
    output: ./Lec19_Notes.tex
---

* **Reading**: Ch 3 - Shumway and Stoffer

# ARIMA models

Let's end our discussion of ARIMA models by finally putting everything together and adding the "I" term - this is for Autoregressive Integrated Moving Average.

When do you use ARIMA models? Up until now, we have discussed fitting models only to *stationarity* data, with many various ways of checking whether our data meet these assumptions. However, many real datasets are nonstationary, and thus we need to do some transformations to make our models work. 

In earlier lectures where we discussed the random walk, $x_t = x_{t-1} + w_t$, we also discussed that we can difference the signal, and that $\nabla x_t = w_t$ is stationary. We could also have a process consisting of a trend (nonstationary) and a zero-mean stationary component, for example:

$x_t = \mu_t + y_t$ where $\mu_t = \beta_0 + \beta_1 t$ and $y_t$ is stationary. If we difference this process, we get

$$\nabla x_t = x_t - x_{t-1} = \beta_1 + y_t - y_{t-1} = \beta_1 + \nabla y_t$$

which is indeed stationary. 

We can also apply this differencing more than once! If we have instead a kth order polynomial, $\mu_t = \sum_{j=0}^k \beta_j t^j$, then the differenced series $\nabla^k x_t$. For example, if we have

$$\mu_t = \mu_{t-1}+v_t \quad  and \quad v_t = v_{t-1}+e_t$$

where $e_t$ is stationary. $\mu$ is a random walk, but so is $v$! Differencing once,

$$\nabla x_t = \nabla \mu_t + \nabla y_t = v_t + \nabla y_t,$$

since $\nabla \mu_t = \mu_t - \mu_{t-1} = v_t$ from the first recursion.

Because $\nabla y_t$ is stationary, the (non)stationarity of $\nabla x_t$ is governed entirely by $v_t$. Unrolling the recursion $v_t = v_{t-1} + e_t$ from an initial value $v_0$,

$$v_t = v_0 + \sum_{s=1}^{t} e_s$$.

This still depends on $t$, so it is nonstationary (it's still a random walk). So we can difference twice:

$$\nabla^2 x_t = \nabla v_t + \nabla^2 y_t = e_t + \nabla^2 y_t,$$

and both terms are stationary, so $\nabla^2 x_t$ is stationary. This reflects the fact that $\mu_t$ here is effectively $I(2)$: a stochastic trend built from two nested random walks requires two differences to reduce to stationarity.

**Definition**: *A process is said to be ARIMA(p,d,q) if:*

$$\nabla^d x_t = (1-B)^d x_t$$

*is ARMA(p,q). In general we can also write:*

$$\phi(B)(1-B)^d x_t = \theta(B) w_t$$

*If $E(\nabla^d x_t) = \mu$, we can write the model as:*

$$\phi(B)(1-B)^d x_t = \delta + \theta(B)w_t$$,

*where $\delta = \mu(1-\phi_1 - \cdots - \phi_p)$.*

So how much differencing should we do? We should be careful! It's very rare that differencing order $d>1$ is necessary. Over-differencing can also add correlation where there was none. For example, if $x_t = x_{t-1} + w_t$ is a random walk, differencing twice leads to a non-invertible moving average $\nabla^2 x_t = w_t - w_{t-1}$.

Remember also that when we use differencing before forecasting, we are forecasting the difference series, and not our original series, so we will have to integrate our differenced data to get the original values back. We actually did this last time ourselves, but here we'll show a few more examples. 
