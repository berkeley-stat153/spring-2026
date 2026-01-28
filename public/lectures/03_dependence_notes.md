---
title: "Lecture 3 Notes - Characteristics of Time Series"
description: "Stat153/248 Time Series - Lecture 3 Notes"
author:
  - name: "Liberty Hamilton"
exports:
  - format: pdf
    template: /Users/liberty/Documents/Berkeley/Teaching/plain_latex
    output: ./Lec03_Notes.pdf
  - format: tex
    output: ./Lec03_Notes.tex
---

* **Reading**: Chapter 1.3-1.7 – Shumway and Stoffer

# Review / basic concepts

* Stochastic process
* White noise
* Moving average
* Autoregression
* Random walk (w/ and w/o drift)
* Signals in noise

# This week - Measures of dependence

* Mean function
* Autocovariance
* Autocorrelation
* Cross-covariance
* Cross-correlation
* Stationarity
* Multidimensional time series
* Random number generation

# Mean and variance

Last week and in your lab, we saw the concept of how mean and variance differ for a white noise process, a random walk, and a random walk with drift. The *mean* and *variance functions* of a time series are useful descriptors because they helps us determine something about the drift and the spread of the data that we should expect over time.

The *mean function* is defined as $\mu_{xt} = \mathbb{E}(x_t)$

The *variance function* is $\sigma^2_t = \operatorname{Var}(x_t) = \mathbb{E}[(x_t-\mu_t)^2]$.

Let's revisit some examples from before:

## White noise
For a white noise time series, $\mu_{wt} = \mathbb{E}(w_t)=0$ for all $t$. $\operatorname{Var}(w_t) = 1$ (for a Gaussian white noise series).

## Moving average

What if we apply a 3-point moving average? Although this induces some correlation structure, it actually does not change the mean function at all:

$\mu_{vt} = \mathbb{E}(v_t) = \frac{1}{3}[\mathbb{E}(w_{t-1})+\mathbb{E}(w_{t})+\mathbb{E}(w_{t+1})] = 0$

## Random walk with drift

For the random walk with drift, the mean function is just the line $\mu_{xt} = \delta t + \displaystyle\sum_{j=1}^t E(w_j) = \delta t$.

## Signal plus noise

And for a signal plus noise (as an example, we'll use this sinusoid):

$$
\begin{aligned}
\mu_{xt} = \mathbb{E}(x_t) &= \mathbb{E} [A \cos(2\pi \omega t + \phi) + w_t] \\ 
&= \mathbb{E} [A \cos(2\pi \omega t + \phi)] + \mathbb{E}[w_t] \\
&= \mathbb{E} [A \cos(2\pi \omega t + \phi)]
\end{aligned}
$$

# Autocovariance

What if we instead want to know something about the dependence between two points $s$ and $t$ within the same time series. We'll call this *autocovariance*, $\gamma$. 

$\gamma_x(s,t) = \operatorname{cov}(x_s,x_t) = \mathbb{E}[(x_s-\mu_s)(x_t-\mu_t)]$

When $s=t$, we have:

$\gamma_x(t,t) = \mathbb{E}[(x_t-\mu_t)^2] = \operatorname{Var}(x_t)$

For white noise, there should be no dependence between differing time points. By definition, $\mathbb{E}(w_t)=0$ and:


$$
\gamma_w(s,t) = \operatorname{cov}(w_s,w_t) = 
\begin{cases}
\sigma^2_w, & \text{if } s=t, \\
0, & s \neq t
\end{cases}
$$

## Covariance of linear combinations

If we have random variables $U=\displaystyle\sum_{j=1}^m a_j X_j$ and $V=\displaystyle\sum_{k=1}^r b_k Y_k$ that are linear combinations of (finite variance) random variables ${X_j}$ and ${Y_k}$, then the covariance of these is:

$\operatorname{cov}(U,V) = \displaystyle\sum_{j=1}^m \displaystyle\sum_{k=1}^r a_j b_k \operatorname{cov}(X_j, Y_k)$

Also, $\operatorname{var}(U) = \operatorname{cov}(U,U)$.

So how can we use this? Now we can try this for the moving average example. 

$$
\begin{aligned}
\gamma_v(s,t) = \operatorname{cov}(v_s, v_t) &= \operatorname{cov}(\tfrac{1}{3}(w_{s-1}+w_s+w_{s+1}), \tfrac{1}{3}(w_{t-1}+w_t+w_{t+1})) \\
&= \tfrac{1}{9} \operatorname{cov}(w_{s-1}+w_s+w_{s+1}, w_{t-1}+w_t+w_{t+1})
\end{aligned}
$$

When $s=t$, we have:
$$
\begin{aligned}
\gamma_v(t,t) &= \operatorname{cov}(v_t, v_t) \\
&= \operatorname{cov}(\tfrac{1}{3}(w_{t-1}+w_t+w_{t+1}), \tfrac{1}{3}(w_{t-1}+w_t+w_{t+1})) \\
&= \tfrac{1}{9} (\operatorname{cov}(w_{t-1},w_{t-1}) + \operatorname{cov}(w_{t},w_{t}) + \operatorname{cov}(w_{t+1},w_{t+1}) \\
&\quad + 2\operatorname{cov}(w_{t-1}, w_t)) + 2\operatorname{cov}(w_{t}, w_{t+1})) + 2\operatorname{cov}(w_{t-1}, w_{t+1}))\\
&= \tfrac{1}{9} (\sigma_w^2 + \sigma_w^2 + \sigma_w^2 + 0 +0 +0)\\
&= \tfrac{3}{9} \sigma_w^2 \\
&= \tfrac{1}{3} \sigma_w^2
\end{aligned}
$$

When $s=t+1$, we have:
$$
\begin{aligned}
\gamma_v(t+1,t) &= \operatorname{cov}(\tfrac{1}{3}(w_{t}+w_{t+1}+w_{t+2}), \tfrac{1}{3}(w_{t-1}+w_t+w_{t+1})) \\
&= \tfrac{1}{9}(\operatorname{cov}(w_t,w_t)\\
	&\quad +\operatorname{cov}(w_{t+1},w_{t+1}))\\
	&\quad +\operatorname{cov}(w_{t},w_{t-1})\\
	&\quad +\operatorname{cov}(w_{t},w_{t+1})\\
	&\quad +\operatorname{cov}(w_{t+1},w_{t-1})\\
	&\quad +\operatorname{cov}(w_{t+1},w_{t})\\
	&\quad +\operatorname{cov}(w_{t+2},w_{t-1})\\
	&\quad +\operatorname{cov}(w_{t+2},w_{t})\\
	&\quad +\operatorname{cov}(w_{t+2},w_{t+1}))\\
&= \tfrac{1}{9}(\operatorname{cov}(w_t,w_t)+\operatorname{cov}(w_{t+1},w_{t+1}))\\
&= \tfrac{2}{9}\sigma^2_w
\end{aligned}
$$

If we then follow this for more lags, we get:

$$
\gamma_v(s,t) = 
\begin{cases}
\tfrac{3}{9}\sigma^2_w, & \text{if } s=t, \\
\tfrac{2}{9}\sigma^2_w, & \text{if } |s-t|=1, \\
\tfrac{1}{9}\sigma^2_w, & \text{if } |s-t|=2, \\
0, & \text{if } |s-t|>2
\end{cases}
$$

Why is this interesting? The autocovariance depends only on the *lag* between $s$ and $t$ and not on the absolute location of these time points. We'll come back to this when we talk about *stationarity*.

