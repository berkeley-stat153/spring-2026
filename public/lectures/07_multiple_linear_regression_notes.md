---
title: "Lecture 7 Notes - Multiple Linear Regression "
description: "Stat153/248 Time Series - Lecture 7 Notes"
author:
  - name: "Liberty Hamilton"
exports:
  - format: pdf
    template: /Users/liberty/Documents/Berkeley/Teaching/plain_latex
    output: ./Lec07_Notes.pdf
  - format: tex
    output: ./Lec07_Notes.tex
---

* **Reading**: Chapter 2 – Shumway and Stoffer

# Today

## Simple and multiple linear regression

Last time we spoke about regression models where we have just two parameters, $\beta_0$ and $\beta_1$:

$$y = \beta_0 + \beta_1 x + \epsilon$$

For example, $y$ is height of an adult and $x$ is the height of their parent, or $y$ is the price of chicken and $x$ is time.

However, often we have multiple independent series that may be contributing to $y$. We can then express this through *Multiple Linear Regression*

$$y_i = \beta_0 + \beta_1 x_{i_1} + \beta_2 x_{i_2} + \cdots + \beta_n x_{i_p} + w_i$$

Here we will still be estimating $\beta_0, \dots, \beta_n$. 

We can do this by rewriting the equation above as:

$$y_i = x_i^\intercal\beta,$$ 

for $i=1,\dots,n$. To add an intercept, we can redefine each vector $x_i$ so that it has a 1 prepended to it:

$x=(1,x_1, x_2, \dots, x_p)$

Now, the first entry of $\beta$ will be the intercept, while the rest are the coefficients for each $x_j$.

We now have the least squares problem:

$$\underset{{\beta \in \mathbb{R}^p}}{\min} \displaystyle\sum_{i=1}^n (y_i - x_i^\intercal \beta)^2$$

We can then set the derivatives equal to zero to obtain:

$$\hat{\beta} = \left( \displaystyle\sum_{i=1}^n x_i x_i^T \right)^{-1} \displaystyle\sum_{i=1}^n x_i y_i$$

## Matrix notation
We can also write this equation in a more general matrix form:

$$\underset{n\times 1}{y} = \underset{n \times p}{X} \underset{p \times 1}{\mathbb{\beta}}$$

$$
  y = \begin{bmatrix} 
    y_1 \\ y_2 \\ \vdots \\ y_n 
  \end{bmatrix}, \quad 
  X = \begin{bmatrix} 
    x_{11} & x_{12} & \cdots & x_{1p} \\
    x_{21} & x_{22} & \cdots & x_{2p} \\
    \vdots & & & \\
    x_{n1} & x_{n2} & \cdots & x_{np} 
    \end{bmatrix}, \quad
  \beta = \begin{bmatrix}
    \beta_1 \\ \beta_2 \\ \vdots \\ \beta_p 
    \end{bmatrix}
$$

Here $n$ might represent the number of time points and $p$ is the number of parameters. We can then write our least squares problem as:

$$\underset{{\beta \in \mathbb{R}^p}}{\min} || y - X\beta||^2_2 $$

Recall that the $\ell_2$ norm $|| \cdot ||_2$ of a vector $a \in \mathbb{R}^d$ is defined as $||a||^2_2 = \sum_{i=1}^d a_i^2$. Then, we can solve to get an estimate of $\hat{\beta}$:

$$\underset{p \times 1}{\hat{\beta}} = \underset{p \times p}{(X^\intercal X)}^{-1}\underset{p \times n}{X^\intercal} \underset{n \times 1}{y}$$

An important note here is that we assume that the columns of $X$ (sometimes called our *features*) are *linearly independent*. This can only happen for $p\leq n$ - where we have no more features than samples. Otherwise, $X \intercal X $ will not have an inverse, but we will be able to deal with this using regularization (which we will cover later).

## Finger tapping demo

Next we will try collecting some data in class and fitting a multiple linear regression model to the data. We will collect data based on a finger tapping task, which is sometimes used as a clinical diagnostic tool to assess fine motor speed, coordination, and brain function. For these tests, typically the experimenter assesses the number of taps that a person makes over a given period of time. As you might imagine, we can also look at whether the number of taps is fairly steady over time, or whether the person is showing some fatigue (slowing down their taps over time). 

We will use the [following website](https://stat153.berkeley.edu/spring-2026/lectures/07_finger-tap.html) to test your finger taps, then analyze the resulting data. 

We will also chat about what external factors might influence the data and which might make the most sense to add to our model predicting the data.

