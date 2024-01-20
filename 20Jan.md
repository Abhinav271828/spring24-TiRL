---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 20 January, Saturday (Lecture 5)
author: Taught by Prof. Tejas Bodas
---

# Markov Chains (contd.)
## Markov Chains as Recursions
Consider the recursion $X_{n+1} = f(X_n, U_n)$, where $f : \mathcal{S} \times \{0, 1\} \to \mathcal{S}$ and $\{U_n\}_{n \geq 0}$ is an i.i.d sequence of RVs.  
Then the process $\{X_n\}_{n \geq 0}$ is a Markov chain with TPM $P_{ij} = \Pr(f(i, U) = j)$.  
Conversely every Markov chain can be represented by such a combination of $f$ and $U$.

The forward direction is trivial, following from the independence of $X_1, \dots, X_{i-1}$ and $f(X_i, U_i) = X_{i+1}$.

The reverse direction can be proved constructively using uniform RVs $U_i$ and the inverse simulation method on the $i^\text{th}$ row of the TPM. In other words, if $F_i$ denotes the CDF of the $i^\text{th}$ row of the TPM, then we let
$$f(i, U_n) = F_i^{-1}(U_n).$$

## Markov Reward Process
Consider a Markov chain $\{X_n\}_{n \geq 0}$ on $\mathcal{X}$, where $|\mathcal{X}| = M$.  
Suppose reward $r(X_t)$ is obtained when in state $X_t$ at time $t$. Let $\beta \in (0, 1)$ be the discount factor for the rewards.  
We would like to characterize the cumulative expected discounted reward $V(x)$ conditioned on starting in state $x$. Clearly,
$$V(x) = \mathbb{E}_{[X_0 = x]} \left[\sum_{t=0}^\infty \beta^t r(X_t)\right].$$

Note that $V : \mathcal{X} \to \mathbb{R}$, and so we can consider it a vector $V \in \mathbb{R}^{M}$.

$V(x)$ can be derived as the unique solution to
$$V(x) = \beta(PV)(x) + r(x),$$
where $(PV)(x)$ denotes the $x^\text{th}$ row in $PV$.  
This can be proved as follows.
$$\begin{align*}
V(x) &= \mathbb{E}_x \left[\sum_{t=0}^\infty \beta^t r(X_t)\right] \\
&= r(x) + \beta \mathbb{E}_x\left[\sum_{t=1}^\infty\beta^{t-1}r(X_t)\right] \\
&= r(x) + \beta \mathbb{E}_x \left[\mathbb{E}_{X_1} \left[\sum_{t=1}^\infty \beta^{t-1}r(X_t) \mid X_{t-1}\right] \right] \\
&= r(x) + \beta\mathbb{E}_x \left[V(X_1)\right] \\
&= r(x) + \beta\sum_{x_1 \in \mathcal{X}} P_{xx_1}V(x_1) \\
&= r(x) + \beta(PV)(x).
\end{align*}$$

From this, we can derive a closed-form expression for $V$ as
$$V = r(I - \beta P)^{-1}.$$

This is possible when the state space is finite. In the case of an infinite state space, we use an iterative process on some random starting $V_0$ to find a stationary $V$.

## Deterministic Dynamic Programming
In a deterministic DP, the environment is governed by a deterministic plant equation. We need to find the maximum-reward path.

For example, consider the problem of finding the least-cost path from the root $R$ of a weighted binary DAG to any leaf.  
For any node $s$, let $V(s)$ denote (the cost of) the least-cost path to any leaf, starting from $s$.  
Let $f(s, a)$ denote the state reached by taking action $a$ at state $s$. The function $f$ is called the plant equation.  
Let $c(s, a)$ denote the edge cost incurred by taking the action $a$ from node $s$.  
Using these abstractions, we can write an expression for $V(R)$ in terms of the states reachable from $R$ in one step.
$$V(R) = \min_{a \in \{l, r\}} \left[c(R, a) + V(f(R, a))\right].$$
This is a Bellman equation, which can be solved recursively backwards.

However, this only gives us the *cost of* the least-cost path. Finding the actual path is analogous:
$$\pi^*(R) = \argmax_{a \in \{l, r\}} \left[c(R, a) + V(f(R, a))\right].$$

In general, in a dynamic programming problem,

* We consider a discrete set of timesteps $t = 0, \dots, T$.
* $\mathcal{S}$ denotes the state space and $\mathcal{A}$ the action space. These are usually countable.
* $s_t \in \mathcal{S}$ is the state of the system at time $t$. $s_0$ is the starting state.
* $r(s_t, a_t)$ is the reward at time $t$, taking action $a_t$ from state $s_t$.
* $r_T(s_T)$ is the reward for terminating in $s_T$ at time $T$.
* $c(s, a)$ is the cost formulation.
* $s_{t+1} = f(s_t, a_t)$ is the dererministic plant equation.
* A policy $\pi = \{\pi_t : t = 0, \dots, T-1\}$ specifies an action $\pi_t \in \mathcal{A}$ for each timestep $t$.
* The value of a policy $\pi$ is given by the cumulative reward
$$V^\pi(s_0) = r(s_0, \pi_0) + r(s_1, \pi_1) + \cdots + r_T(s_T).$$

Thus, the definition of a DP problem is the following optimization problem:
$$V(s_0) = \max_{\pi in \Pi} V^\pi(s_0)$$
such that $s_{t+1} = f(s_t, \pi_t)$ for $t = 0, \dots, T-1$, given an initial state $s_0$.

In general, we obtain an implicit or recursive equation for $V(s_0)$ in terms of smaller and smaller subproblems.