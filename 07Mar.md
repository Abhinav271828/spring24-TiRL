---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 07 March, Thursday (Lecture 15)
author: Taught by Prof. Tejas Bodas
---

## Time Invariance
A system is said to be time-invariant if the outputs shift along with the inputs. This property is also called time invariance.

Mathematically, if the input $u_1(t)$ gives the response $y_1(t)$, then the input $u_1(t-\tau)$ will give the response $y_1(t-\tau)$.

## LTIV Systems
### Convolution Theorem
Let $g(t)$ be the impulse response of an LTIV system, *i.e.*, $X(t) = g(t)$ when $U(t) = \delta(t)$. Then, by homogeneity,
$$\delta(t-\tau)U(\tau) \mapsto g(t-\tau)U(\tau).$$
We can integrate w.r.t $\tau$ over $[0, t]$. This gives us
$$U(t) \mapsto \int_0^t g(t-\tau)U(\tau)d\tau = (g * U)(t).$$

### General LTIV Response
Consider a general LTIV system
$$\dot{X} = AX + BU,$$
where $A \in \mathbb{R}^{n \times n}, X \in \mathbb{R}^n, B \in \mathbb{R}^{n \times m}, U \in \mathbb{R}^m$.

Then we have
$$X(t) = e^{At}X(0) + e^{At} \int_0^t e^{-A\tau}BU(\tau)d\tau.$$
This directly follows from the fact that the impulse response is $g(t) = e^{At}B$.

## Controllability
A system is said to be controllable iff it is possible, by means of the input, to transfer the system from any initial state $X(0)$ to any other state $X(T_f)$ in a finite time $T_f$. If the controllability matrix
$$\text{CM} = [A^0BA^1B \cdots A^{n-1}B]$$
is nonsingular, the system is controllable.

This is because we have that
$$e^{A(t-\tau)}B = B + AB(t-\tau) + \frac{A^2B(t-\tau)^2}{2!} + \cdots + \frac{A^{n-1}B(t-\tau)^{n-1}}{(n-1)!} + \text{higher order terms}.$$
and so each matrix $A^iB$ must be nonsingular.