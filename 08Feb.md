---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 08 February, Thursday (Lecture 10)
author: Taught by Prof. Tejas Bodas
---

# Markov Chains
## Contraction Mappings
Let $B(\mathcal{S})$ be the set of bounded functions on $\mathcal{S}$, *i.e.*,
$$B(\mathcal{S}) = \{U : \mathcal{S} \to \mathbb{R} \mid \forall i \in \mathcal{S}, U(i) < \infty\}.$$

For any function $u \in B(\mathcal{S})$, let
$$||u|| = \sup_{i \geq 0} |u(i)|.$$

A mapping $T : B(\mathcal{S}) \to B(\mathcal{S})$ is a contraction mapping if
$$||Tu - Tv|| \leq \beta||u - v||,$$
for some $\beta < 1$.

Let $T^nu_0 = u_n$. Then, for a contraction mapping $T$, we have
$$||u_n - u_{n-1}|| \leq \beta^n||u_1 - u_0||.$$
From this it follows that there exists a unique function $g \in B(\mathcal{S})$ such that $\lim_{n \to \infty} T^n u = g$ for all $u \in B(\mathcal{S})$.

We note that $T^f$, defined as
$$(T^fu)(i) = r(i, f(i)) + \alpha\sum_j P(j \mid i, f(i))u(j),$$
is a contraction mapping with factor $\alpha$. This can be proved as follows:
$$\begin{align*}
||T^fV_1 - T^fV_2|| &= ||r(i, f(i)) + \alpha\sum_j P(j \mid i, f(i))V_1(j) \\
&- r(i, f(i)) - \alpha\sum_j P(j \mid i, f(i))V_2(j)|| \\
&= \alpha||\sum_jP(j \mid i, f(i)) (V_1(j)-V_2(j))|| \\
&\leq \alpha\sum_jP(j \mid i, f(i))\cdot||V_1(j)-V_2(j)|| \\
&= \alpha||V_1 - V_2||.
\end{align*}$$

### Optimality Operator
We define the mapping $T_\alpha : B(\mathcal{S}) \to B(\mathcal{S})$ as
$$(T_\alpha u)(i) = \max_a \left\{r(i, a) + \alpha\sum_j P_{ij}(a)u(j)\right\}.$$
This is a contraction mapping, with fixpoint $V_\alpha$.

We can find this fixpoint via value iteration, choosing an $\epsilon$ and stopping iteration when the difference between successive values is less than this value.

Consider the value $||V_n - V_\alpha||$. Can we bound this value?  
One bound is
$$||V_n - V_\alpha|| \leq \alpha^n||V_0 - V_\alpha||.$$
We can also say that
$$||V_n - V_{n+1}|| \leq \alpha^n||V_0 - V_1||.$$

To derive a useful bound, consider the following:
$$\begin{align}
||V_n - V_\alpha|| &= ||(V_n - V_{n+1}) + \cdots + (V_{n+l} - V_\alpha)|| \\
&\leq \alpha^n||V_1 - V_0|| (1 + \alpha + \cdots + \alpha^{l-1}) + ||V_{n+l} - V_\alpha|| \\
&= \frac{\alpha^n}{1-\alpha}||V_1 - V_0|| \text{ [in the limit]}.
\end{align}$$
We can also show that
$$||V_n - V_\alpha|| \leq \frac\alpha{1-\alpha}||V_n- V_{n-1}||.$$

## Value Iteration
### Gaus-Seidel or In-Place Value Iteration
This algorithm simply replaces values of states in-place, instead of using separate vectors $V_n$ and $V_{n+1}$. Then the iteration simply updates
$$V(s) = \max_a\left\{r(s,a) + \alpha\sum_j P(j \mid s, a) V(j) \right\}.$$

This can be seen as a map with the transformation
$$V_{n+1}(s_i) = \max_a\left\{r(s_i, a) + \alpha\left[\sum_{j < i} P(s_j \mid s_i, a) V_{n+1}(s_j) + \sum_{j \geq i} P(s_j \mid s_i, a) V_n(s_j)\right]\right\}$$

### Asynchronous VI
If $\mathcal{S}$ is large, iterating over it can be prohibitively expensive. Thus we prioritize some states by visiting them more often.

## Modified Policy Iteration
PI requires fewer iterations, but VI takes less time per iteration.

We can combine these two algorithms. We don'tof evaluate $V^{\pi_n}$ for an intermediate policy $\pi_n$ fully; instead we do the policy evaluation  step for some $m_n$ steps and then use the resulting vector $V_n$ to get $V_{n+1}$ using VI.