---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 18 March, Monday (Lecture 18)
author: Taught by Prof. Harikumar Kandath
---

# Dynamic Programming (contd.)
## Solution
The dynamics of a discretized continuous system are given by
$$X(k+1) = AX(k) + BU(k).$$

Cost to be minimized
$$J = \frac12 X^T(N)HX(N) + \frac12\sum_{k=0}^{k=N-1}\left[X^T(k)Q(k)X(k) + U^T(k)R(k)U(k)\right].$$
Here $H, Q, R$ are parameters that we use to weight the importance of the various terms.

We can differentiate and show that $J^*_{N-1}(X(N-1))$ is achieved by
$$\begin{align*}
U^*(N-1) &= -[R(N-1) + B^TP(0)B]^{-1} B^T P(0) AX(N-1) \\
&= F(N-1)X(N-1).
\end{align*}$$

In general, we have the relations
$$F(N-k) = -[R(N-k) + B^TP(k-1)B]B^TP(k-1)A$$
$$U^*(N-k) = F(N-k)X(N-k),$$
which give us
$$J^*_{N-k}(X(N-k)) = \frac12X^T(N-k)P(k)X(N-k)$$
$$\begin{align*}
P(k) &= [A+BF(N-k)]^TP(k-1)[A+BF(N-k)] \\
&+ F^T(N-k)R(N-k)F(N-k) \\
&+ Q(N-k)
\end{align*}$$

In terms of implementation, we compute $F(N-k)$ and $P(k)$ in a backward pass $k = 0, \dots, 10$. Then we can compute $U(k)$ and $J(X(k))$ using these values.