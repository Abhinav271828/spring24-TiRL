---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 07 March, Thursday (Lecture 15)
author: Taught by Prof. Tejas Bodas
---

# Properties of Systems
## LTIV Response in Absence of Input
Consider the case where $U(t) = 0$. Then we can write the response as
$$X(t) = c_1X_1e^{\lambda_1}t + \cdots + c_nX_ne^{\lambda_n}t,$$
where $\lambda_i$ and $X_i$ are corresponding eigenvalues and eigenvectors of $A$. This can be proved by differentiating and substituting $\lambda_iX_i$ with $AX_i$ in each term.

The constraints $c_i$ can be obtained from the initial condition $X(0)$.

Note, therefore, that in such a system, the response is completely determined by the eigenvalues of $A$. Therefore controlling the eigenvalues is sufficient to control the system's response.

## Stability
There are two types of stable systems: BIBO-stable and asymptotically stable.

BIBO (bounded input-bounded output) stability implies  that $||X(t)|| < M$, where $M < \infty$ as $t \to \infty$.

Asymptotically stable systems satisfy $||X(t)|| \to 0$ as $t \to \infty$.

## Controllability (contd.)
All eigenvalues of the system matrix can be modified using state feedback control if the system is controllable, *i.e.*, setting
$$U(t) = KX(t)$$
gives us
$$\dot{X}(t) = AX(t) + BU(t) = (A+BK)X(t),$$
where we can choose $K$ such that $A+BK$ has the desired eigenvalues.

Controllability is important as an unstable system can be stabilised via state-feedback control.

# Discrete-Time Systems
Here, we consider continuous-time systems, but examine them at discrete time intervals of a fixed duration $\delta t$. This interval is called the sampling time.

Here we have
$$\begin{align*}
X(t+\delta t) &= e^{A(t+\delta t)}X(0) + e^{A(t+\delta t)} \int_0^{t+\delta t} e^{-A\tau}BU(\tau)d\tau \\
&= e^{A \delta t}\left[e^{At}X(0) + e^{At}\int_0^te^{-A\tau}BU(\tau)d\tau + e^{At}\int_t^{t+\delta t}e^{-A\tau}BU(\tau)d\tau\right] \\
&= e^{A\delta t}\left[X(t) + e^{At}\left(\int_t^{t+\delta t} e^{-A\tau}Bd\tau\right)U(t)\right] \\
&= e^{A\delta t}X(t) +  e^{A\delta t}e^{At}\left(\int_t^{t+\delta t} e^{-A\tau}Bd\tau\right)U(t) \\
&= e^{A\delta t}X(t) + e^{A\delta t}BU(t).
\end{align*}$$

We write this as
$$X(k+1) = A_dX(k) + B_dU(k),$$
where $A_d, B_d$ are determined by the above relation.

# Principle of Optimality
An optimal trajectory has the property that, given initial conditions and control inputs over some initial period, the remaining control inputs are optimal.

Essentially, any subsequence of controls in an optimal trajectory must be optimal for the state transition corresponding to that subsequence.