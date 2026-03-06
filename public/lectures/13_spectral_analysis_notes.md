---
title: "Lecture 13 Notes - Power Spectral Analysis"
description: "Stat153/248 Time Series - Lecture 12 Notes"
author:
  - name: "Liberty Hamilton"
exports:
  - format: pdf
    template: /Users/liberty/Documents/Berkeley/Teaching/plain_latex
    output: ./Lec13_Notes.pdf
  - format: tex
    output: ./Lec13_Notes.tex
---

* **Reading**: Ch 4 - Shumway and Stoffer

# Discrete Fourier Transform

We ended last lecture talking about the *periodogram*, which was defined as:

$$y_t = a_0 + \sum_{j=1}^{\lfloor n/2 \rfloor} [a_j \cos(2\pi t j/n) + b_j \sin(2\pi t j/n)]$$

with $a_j = \frac{2}{n}\sum_{t=1}^n x_t \cos(2\pi t j/n)$ and $b_j = \frac{2}{n}\sum_{t=1}^n x_t \sin(2\pi t j/n)$.

Today we will relate this to the *Discrete Fourier Transform*, which is defined as:

$$d(j/n) = \frac{1}{\sqrt{n}}\sum_{t=1}^n x_t e^{-i 2\pi tj/n}$$

Recalling Euler's formula $e^{-i\theta} = \cos \theta - i \sin \theta$ .

$$ d(j/n) = \frac{1}{\sqrt{n}}\left (\sum_{t=1}^n x_t \cos(2\pi t j/n) - i \sum_{t=1}^n x_t \sin(2\pi t j/n)\right)$$

for $j=0,1,\dots, n-1$ where the frequencies $j/n$ are the Fourier frequencies. We can then compute the squared modulus of the transform, which represents the total strength of a frequency component regardless of the phase. For example, two signals could have very different mixes of sines and cosines for a frequency $j$, but if their moduli are the same, they have the same power at that frequency.

$$|d(j/n)|^2 = \frac{1}{n}\left(\sum_{t=1}^{n} x_t \cos(2\pi t j/n)\right)^2 + \frac{1}{n}\left(\sum_{t=1}^n x_t \sin (2\pi t j/n)\right)^2$$ 

the scaled periodogram is then 

$$P(j/n) = \frac{4}{n} |d(j/n)|^2$$

Note also that $P(j/n) = P(1-j/n)$ for $j=0,1,\dots,n-1$ so we only have to calculate up to $1/2$.

As we saw, the periodogram is a raw, noisy estimate computed from sample data. However, we'd also like to have a way to estimate the true underlying spectral structure, so we need learn about the theoretical quantity that the periodogram is estimating. This is the *spectral density*.

# Spectral Density

Consider a periodic stationary process with fixed frequency $\omega_0 \in (0, 1/2)$:

$$x_t = U_1 \cos(2\pi\omega_0 t) + U_2 \sin(2\pi\omega_0 t)$$

with $U_1$ and $U_2$ as uncorrelated zero-mean random variables with equal variance $\sigma^2$. The frequency $\omega_0$ tells us what fraction of a complete cycle the process completes between consecutive time points. Equivalently, the process requires $1/\omega_0$ time periods to complete one full cycle. We can then calculate the covariance function as:

$$
\begin{aligned}
\gamma(h) &=\sigma^2 \cos(2\pi \omega_0 h) = \frac{\sigma^2}{2}e^{-2\pi i \omega_0 h} + \frac{\sigma^2}{2}e^{2\pi i \omega_0 h}\\
&= \int_{-\frac{1}{2}}^{\frac{1}{2}} e^{2\pi i \omega h} dF(\omega)\\
\end{aligned}
$$

where $F(\omega)$ is the *spectral distribution function* defined by:

$$F(\omega) = \begin{cases}
0 & \omega < -\omega_0\\
\sigma^2/2 & -\omega_0 \leq \omega < \omega_0\\
\sigma^2 & \omega \geq \omega_0 
\end{cases}$$

For details of this integration, your book has a longer explanation and proof in Section C.4.1.

An important property of this spectral distribution function is that if ${x_t}$ is stationary with autocovariance $\gamma(h) = \text{cov}(x_{t+h}, x_t)$, then there exists a unique monotonically increasing function $F(\omega)$ such that the relationship described above in (7) applies.

We can think of $F$ here as being analogous to a cumulative distribition function (CDF), so the integral above is analogous to an expectation defined with respect to the distribution governing $F$. The total mass is $\gamma(0)$ rather than 1. In the case where the autocovariance function is absolutely summable, we have $dF(\omega) = f(\omega)d\omega$, where we call $f(\omega)$ the *spectral density*.

that is, we rewrite this as:

$$
\begin{aligned}
\gamma(h) &= \int_{-\frac{1}{2}}^{\frac{1}{2}} e^{2\pi i \omega h} f(\omega)d\omega\\
\end{aligned}
$$

This is then the inverse transform of the *spectral density function*:

$$f(\omega) = \sum_{h=-\infty}^{h=\infty} \gamma(h)e^{-2\pi i \omega h}\quad -1/2 \leq \omega \leq 1/2$$

This is very cool because the spectral density is then the analog of the probability density function. The fact that $\gamma(h)$ is nonnegative ensures that we always have positive values for each frequency: $f(\omega) \geq 0$ for all $\omega$. We also have $f(\omega)=f(-\omega)$, so typically we only plot $f(\omega)$ for values less than 1/2. For $h=0$ we also have:

$$\gamma(0) = \text{var}(x_t) = \int_{-\frac{1}{2}}^{\frac{1}{2}} f(\omega)d\omega$$

This quantity expresses the total variance as the integrated spectral density over all frequencies.

# Autocovariance vs. spectral distribution functions

These contain similar information but expressed in different ways!

* Autocovariance function expresses information in terms of lags
* Spectral distribution expresses information in terms of cycles / frequencies

Some problems are easier in the time domain (lagged information), whereas some are easier in the spectral domain (if they contain periodic information). 

# Theoretical spectra of different processes

We have discussed a number of types of processes in this class, so let's look at their theoretical power spectra.

![Examples of spectra of white noise, moving average, and second-order autoregressive process](./images/13_spectra.png)

## White noise

Recall that a white noise process consists of uncorrelated random variables $w_t$ with variance $\sigma_w^2$. The autocovariance of a white noise process is $\gamma_w(h)=\sigma_w^2$ for $h=0$ and $0$ otherwise. Thus it follows that the spectral density function is:

$$f(\omega) = \sum_{h=-\infty}^{h=\infty} \gamma(h)e^{-2\pi i \omega h} = \sigma_w^2$$

for $-1/2 \leq \omega \leq 1/2$. This means that white noise contains equal power at all frequencies. In fact, we spoke before about how the term "white noise" is related to the analogy to white light, which contains all colors (frequencies). In fact, we can think of spectral analysis like separating our signals like a prism into different colors (spectra).

### Linear filtering

An important tool in time series analysis is the concept of linear filtering, where we may want to amplify or attentuate different frequencies in our signal. In general, a linear filter uses a set of specified coefficients $a_j$ for $j=0,\pm 1, \pm2, \dots$, to transform an input series $x_t$ into an output $y_t$:

$$y_t = \sum_{j=-\infty}^{\infty} a_j x_{t-j}, \quad \sum_{j=-\infty}^{\infty} |a_j| < \infty$$

Sometimes this is also written as a convolution: $y_t = (a * x)_t$

Here the frequency behavior is hidden inside of the pattern of the weights $a_j$ - we can't immediately see what these are doing because they're time domain weights. However, we can look at the Fourier transform of these weights (the frequency response function):

$$A(\omega) = \sum_{j=-\infty}^{\infty} a_j e^{-2\pi i \omega j}$$

After applying a filter like this, we can connect this to the output spectrum for our new signal $y_t$, and we have:

$$f_y(\omega) = |A(\omega)|^2 f_x(\omega)$$

For a proof, you can look at Chapter 4 of Shumway and Stoffer (Property 4.3). 

One thing that's very nice about this is that convolution in the time domain becomes multiplication in the frequency domain, which makes this representation much more convenient in many time series problems.

Note that this also looks like the property of variances where if $Y=aX$, then $\text{var}(Y)=a^2\text{var}(X)$ assuming $\text{var}(X)$ exists.

## Moving Average

For the moving average, let's consider a causal moving average (remember, this is the version where we only consider values from the past in our moving average):

$$x_t = w_t + \theta w_{t-1}$$

The autocovariance function for this is:

$$
\gamma(h) = \begin{cases}
(1+\theta^2)\sigma^2 & h=0\\
\theta\sigma^2 & |h|=1\\
0 & |h| > 1
\end{cases}
$$

The spectral density is therefore: 

$$
\begin{aligned}
f(\omega) &= (1+\theta^2)\sigma^2 + \theta\sigma^2(e^{-2\pi i \omega}+e^{2\pi i \omega}) \\
&= \sigma^2(1+\theta^2+2\theta\cos(2\pi\omega))
\end{aligned}
$$

in the second line, we used $\cos(\theta) = (e^{i\theta}+e^{-i\theta})/2$ from Euler's formula $e^{i\theta}=\cos(\theta)+i\sin(\theta)$.

What results here is that the MA process has a spectral density that decays from zero, with larger $\theta$ corresponding to a steeper decay from $\omega=0$ to $\omega=1/2$. 

Next week we will talk about how to deal with noisiness in the DFT and periodogram through a few different techniques, and we will extend our conversation to time-frequency analysis, where we care about how frequency coefficients change over time.
