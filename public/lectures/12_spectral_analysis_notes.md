---
title: "Lecture 12 Notes - Power Spectral Analysis"
description: "Stat153/248 Time Series - Lecture 12 Notes"
author:
  - name: "Liberty Hamilton"
exports:
  - format: pdf
    template: /Users/liberty/Documents/Berkeley/Teaching/plain_latex
    output: ./Lec12_Notes.pdf
  - format: tex
    output: ./Lec12_Notes.tex
---

* **Reading**: Ch 4 - Shumway and Stoffer

# Periodic signals

We spoke previously about periodic signals 

$$y(t) := \beta_0 + R \cos (2\pi f t + \phi)$$

which can also be re-parameterized as 

$$y(t) = \beta_0 + U_1 \cos 2\pi ft + U_2 \sin 2\pi ft$$

where $U_1=A\cos\phi$ and $U_2=-R\sin\phi$, and $U_1$ and $U_2$ are taken to be normally distributed random variables. The amplitude is $A=\sqrt{U_1^2+U_2^2}$ and the phase is $\phi=\arctan (U_2/U_1)$. If we assume that $U_1$ and $U_2$ are uncorrelated random variables with mean 0 and variance $\sigma^2$, then we also can show that $$y(t)$$ is stationary because $\mathbb{E}(y_t)=0$ and $\lambda = 2\pi f$:

$$
\begin{aligned}
\gamma_y(t,s) &= \text{cov}(y_t, y_s)\\
&= \text{cov} (U_1 \cos(\lambda t) + U_2 \sin (\lambda t), U_1 \cos(\lambda s) + U_2 \sin (\lambda s))\\
&= \text{cov} (U_1 \cos(\lambda t),U_1 \cos(\lambda s)) + \text{cov} (U_1 \cos(\lambda t),U_1 \sin(\lambda s)) + \\
& \quad \text{cov} (U_1 \sin(\lambda t),U_1 \cos(\lambda s)) +\\
& \quad \text{cov} (U_1 \sin(\lambda t),U_1 \sin(\lambda s))\\
&= \sigma^2 \cos (\lambda t) \cos (\lambda s) + 0 + 0 + \sigma^2 \sin (\lambda t) \sin(\lambda s)\\
&= \sigma^2 [\cos (\lambda t) \cos (\lambda s) + \sin (\lambda t) \sin (\lambda s)]\\
&= \sigma^2 \cos (\lambda(t-s))
\end{aligned}
$$

Because this quantity depends only on the time lag $t-s$. We can create a generalization of this signal that allows mixtures of periodic signals with multiple freqauencies and amplitudes:

$$y_t = \sum_{k=1}^q [U_{k1}\cos(2\pi f_k t) + U_{k2}\sin(2\pi f_k t)]$$

where $U_{k1},U_{k2}$ for $k=1,2,\dots,q$ are uncorrelated zero-mean random variables with variances $\sigma^2_k$ and $f_k$ are distinct frequencies. 

Let's look at some examples of these mixtures of frequencies in the accompanying jupyter notebook.

# Periodogram

Now let's say we wanted to estimate the component frequencies of a signal like this, where we didn't know what the underlying components were. One way to do this is using the *periodogram*.

For any time series, we can write:

$$y_t = a_0 + \sum_{j=1}^{\lfloor n/2 \rfloor} [a_j \cos(2\pi t j/n) + b_j \sin(2\pi t j/n)]$$

for $t=1,\dots,n$ and coefficients $a_j$ and $b_j$. $\lfloor \rfloor$ is the greatest integer function (also called the "floor"), which rounds numbers down to the nearest integer. If $n$ is even, we have $a_{n/2} \cos(2\pi t \frac{1}{2})=a_{n/2}(-1)^t$ and $b_{n/2}=0$. 

Here the values of $j$ correspond to frequencies indices. Each $j$ represents a different frequency component in the decomposition. $j/n$ is the frequency in cycles per sample. As $j$ goes from 1 to $\lfloor n/2 \rfloor$, we sweep up through all distinguishable frequencies from the slowest oscillation up to the Nyquist frequency. For example, let's say we have $n=100$ data points:

* $j=1$ means the wave completes exactly 1 full cycle over the n samples, which is the slowest possible oscillation that fits in your data window.
* $j = 2$ completes exactly 2 full cycles, $j = 3$ completes 3, and so on.
* $j = 50 (= n/2)$ completes 50 cycles, the fastest oscillation you can resolve, alternating up-down-up-down every sample.

We can now use regression to get the coefficients:

$a_j = \frac{2}{n}\sum_{t=1}^n x_t \cos(2\pi t j/n)$ and $b_j = \frac{2}{n}\sum_{t=1}^n x_t \sin(2\pi t j/n)$

Here $a_j$ and $b_j$ represent how much of a particular frequency is present in our signal, with $a_j$ and $b_j$ together controlling the amplitude and phase at frequency $j$. These are free parameters and set independently, but jointly contribute to $A$ and $\phi$.

From this, we can then define the *scaled periodogram*:

$$P(j/n) = a_j^2+b_j^2$$

for $j/n \neq 0, 1/2$. The scaled periodogram is the sample variance at each frequency component and is an estimate of $\sigma_j^2$ corresponding to a sinusoid at frequency $f_j = j/n$. These frequencies are called the *Fourier frequencies*. Large values of $P(j/n)$ indicate which frequencies dominate the series, small values may represent noise.

Next time we will relate this to the Discrete Fourier Transform (DFT) of a signal.