---
title: "Lecture 22 Notes - State Space Models Part 2"
description: "Stat153/248 Time Series - Lecture 22 Notes"
author:
  - name: "Liberty Hamilton"
exports:
  - format: pdf
    template: /Users/liberty/Documents/Berkeley/Teaching/plain_latex
    output: ./Lec22_Notes.pdf
  - format: tex
    output: ./Lec22_Notes.tex
---

* **Reading**: Ch 6.2 - Shumway and Stoffer

# Continuation of State Space Models 

# Examples

To remind you, we talked a bit about some examples last time of where state space models might be useful:

* Estimating global warming/global temperatures $x_t$ from land and sea temperature measurements $y_t$
* Brain computer interfaces (BCI) - we want to infer intended cursor velocity ($x_t$) from brain measurements from 100 electrodes in motor cortex $y_t$
* Health - we want to estimate true blood glucose level $x_t$ from $y_t$ intermittent measurements from a continuous glucose monitor with some sensor drift.
* Finance - we want to measure volatility $x_t$ of the S&P500 on day $t$, we observe $y_t$ daily log returns

# Advantages of state space models

What are the advantages of state space models as opposed to other models we've discussed in this class?

1. They deal well with missing data and don't require every time point to be observed. You can still get estimates of the latent state for those missing time points.
2. We can separate process noise from measurement noise. In ARIMA models, we have just one noise term (innovations/shocks). For real scientific applications, we may have noisy sensors where the measurement reading's error is distinct from underlying noise in the true signal.
3. We can have time-varying parameters (unlike fixed $\beta$ in a regression model).

# Filtering, smoothing, and forecasting

For state space models, we want to estimate our underlying unobserved signal $x_t$ given the data $y_{1:s} = {y_1, \dots, y_s}$ to time $s$. In practice, the steps for the state space models include:

1. Filtering, where we estimate $x_t$ using measurements up through time $t$ (here $s$=$t$)
2. Prediction/forecasting, where we have $s<t$ and want to estimate new data
3. Smoothing, where $s>t$. This allows us to estimate $x_t$ using the entire dataset, including observations after $t$. This can be used to better estimate missing values.

We can write out the unobserved signal $x_t$ using the convention:

$x_t^s = E(x_t | y_{1:s})$ 

which again, is $x_t$ given $y$ from time 1 to time $s$, which can be $=t$ (filtering), $<t$ (prediction), or $>t$ (smoothing).

We also have the Prediction error covariance (using the notation from Shumway and Stoffer and the original Kalman 1960 paper):

$P_{t1,t2}^s = E{(x_{t1}-x_{t1}^s)(x_{t2}-x_{t2}^s)^\prime}$

from this, we will look at the Kalman Filter, which gives filtering and forecasting equations (Kalman 1960). As it turns out, modifications of this algorithm were most recently used in the Artemis-II mission to send a crewed flight into lunar orbit and back to Earth. The original Apollo 8 mission and subsequent missions to the moon all used improvements to this original algorithm, in the latest case using 4 navigation Extended Kalman Filters (EKFs) as part of the navigation system. From a [technical report](https://ntrs.nasa.gov/api/citations/20230000548/downloads/Orion_EKF_AAS_2023.pdf) related to Artemis-1:

> The Artemis Program is NASA’s campaign to explore the Moon and beyond. Artemis-1, the uncrewed exoLEO test flight of the Orion spacecraft, was completed in 2022. There are four navigation Extended Kalman Filters (EKFs) that are part of the Orion navigation system. The Atmospheric Extended Kalman Filter (ATMEKF) estimates the vehicle position, velocity, and attitude (referred to as the vehicle state) during the ascent and entry phases of flight. Once Orion is outside of Earth’s atmosphere, the Earth Orbit Extended Kalman Filter (EOEKF) and Cislunar Extended Kalman Filter (CLEKF) estimate the translational states, depending on the phase of flight, while the Attitude Extended Kalman Filter (ATTEKF) estimates the rotational state of the vehicle. The Kalman filters propagate the vehicle state forward in time using a combination of dynamics models and the output data from the Inertial Measurement Unit (IMU). The filters update the vehicle states and associated uncertainties, in the form of the covariance matrix, using pseudorange measurements from GPS (in ATMEKF/EOEKF), optical navigation measurements of the Earth or Moon (in CLEKF), and star tracker measurements (in ATTEKF). Simultaneously, the Kalman filters estimate error sources in the sensors, which are included in the state vectors as Exponentially Correlated Random Variables (ECRVs). 

One note here - the Kalman Filter we'll show an example for is for a linear system, but the EKF allows us to extend this idea into systems with nonlinear dynamics.

# The Kalman Filter

For the state space model with:

*(Hidden) State equation*: $$x_t = \Phi x_{t-1} + \Upsilon u_t + w_t$$ and

*Observation equation*: $$y_t = A_t x_t+\Gamma u_t + v_t$$

with initial conditions $x_0^0=\mu_0$ and $P_0^0 = \sigma_0$ for $t=1,\dots,n$:

$$
\begin{aligned}
x_t^{t-1} &= \Phi x_{t-1}^{t-1} + \Upsilon u_t,\\
P_t^{t-1} &= \Phi P_{t-1}^{t-1}\Phi^\prime + Q,
\end{aligned}
$$

* $x_t^{t-1}$ is the predicted state mean, which is your best guess for the state at $t$ based on $t-1$, applying the dynamics ($\Phi$)
* $P_t^{t-1}$ is the predicted state covariance. Uncertainty is growing during prediction as propagated through $\Phi$ and new process noise $Q$

with

$$
\begin{aligned}
x_t^t &= x_t^{t-1} + K_t(y_t-A_t x_t^{t-1}-\Gamma u_t),\\
P_t^t &= [I-K_t A_t]P_t^{t-1},
\end{aligned}
$$

* $x_t^t$ is the filter state mean, where we correct our prediction by a fraction $K_t$ of the innovation, which is how much today's measurement disagrees with what you predicted
* $P_t^t$ shows how uncertainty shrinks after an update, where the factor $[I-K_t A_t]$ quantifies how much the measurement reduced your uncertainty. When $K_t A_t = I$, we have full reduction, and no reduction when $K_t=0$. 

where

$$K_t=P_t^{t-1} A_t^\prime [A_tP_t^{t-1}A_t^\prime + R]^{-1}$$

is called the Kalman gain. The Kalman gain weighs how much we should trust the measurement vs. the prediction. Noisy measurements (with big $R$) will shrink this gain, while uncertain predictions with big $P_t^{t-1}$

# Running the Kalman Filter

Given a model $(\Phi, A, Q,R)$ and initial conditions $(\mu_0, \sigma_0)$, the filter moves forward through the data one time step at a time.

At each $t$ we do two things:

1. Prediction: propagate the previous estimate forward using the dynamics $x_{t-1}^{t-1} \rightarrow x_t^{t-1}$  (Uncertainty grows)
2. Update: Correct the prediction using new measurements $y_t$ through the Kalman gain. (Uncertainty shrinks)

# The Kalman Smoother

The Kalman Smoother (in the accompanying notebook this is implemented using the Rauch-Tung-Striebel/RTS algorithm, published in 1965). This can be used *post hoc* to refine state estimates retrospectively, using all data (including future data)! 

In practice, this runs using a forward pass (the Kalman filter), followed by a backward pass, which modifies earlier estimates based on the filter's stored outputs.

# When to use the filter vs. smoother

It's helpful to use the Kalman filter for real-time estimates where you can't use future data because it doesn't exist yet. Computationally, the Kalman filter is also very cheap, because you only need data from the previous time step for your estimates. 

On the other hand, the Kalman smoother is helpful for post hoc analyses where getting the best possible estimate at each time point is your goal. For example, in the bone marrow transplant data, if we are trying to look at platelet, WBC, and hematocrit data after the fact to make some claims about patient outcomes, we may want to use the smoother to get better estimates.

# Examples

Next, we'll show an example using the Kalman filter and smoother for estimating 2D trajectory data in the accompanying Lecture22 notebook.