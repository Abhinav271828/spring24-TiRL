---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 05 February, Monday (Lecture 9)
author: Taught by Prof. Tejas Bodas
---

# Recap
We know that
$$V_\alpha(s) = \max_a \left\{r(s, a) + \alpha\sum_{s'} P(s' \mid s, a) V_\alpha(s')\right\}, s \geq 0.$$
To find this $V_\alpha$, we have two types of methods – *policy iteration* and *value iteration*.

We evaluate a policy as
$$V^f(s) = r(s, f(s)) + \alpha\mathbb{E}_{s, f} [V^f(S')].$$

We define a map from $\mathbb{R}^M$ to $\mathbb{R}^M$:
$$(T_fu)(s) = r(s, f(s)) + \alpha\sum_{s'}P_{ss'}(f(s))u(s').$$
This simulates taking one step (in expectation) according to policy $f$, with terminal reward $u$. Clearly, $V^f$ is the fixpoint of this transformation.  
This describes the underlying Markov reward process under policy $f$.

# Markov Chains
## Policy Evaluation (contd.?)
### Constructing the Optimal Policy
Given $V_\alpha$, how can we derive the optimal policy $f_\alpha$? We can identify the best action for each state by maximizing, given the value function $V_\alpha$. In other words, $f_\alpha(i)$ is such that
$$r(i, f_\alpha(i)) + \alpha\sum_j P_{ij}[f_\alpha(i)]V_\alpha(j) = \max_a \left\{r(i,a) + \alpha\sum_j P_{ij}(a)V_\alpha(j)\right\}.$$

### Improving a Policy
Given a policy $f$, we denote by $V^{f*}$ the stationary policy that maximizes the reward according to $V^f$. In other words, we define $f^*$ to be such that
$$r(i, f^*(i)) + \alpha \sum_j P_{ij}[f^*(i)]V^f(j) = \max_a \left\{r(i, a) + \alpha\sum_j P_{ij}(a)V^f(j)\right\}.$$
It can be shown then that $V^{f*}(i) \geq V^f(i)$ for all $i$, by showing that $(T_{f*}^n V_f)(i) \geq V_f(i)$ for all $i$, and noting that $\lim_{n \to \infty} T_{f*}^n V_f = V^{f*}$.

Thus, we have the policy evaluation algorithm. We start with an arbitrary policy $\pi_0$, whose optimal value we find as $V_{\pi_0}$. Then we can improve it to a policy $\pi_1$ and repeat this.

### Convergence
If $f$ is not $\alpha$-optimal, it can be improved; in other words, if $f$ cannot be improved for at least one state, then it is already $\alpha$-optimal.

To prove this, suppose $f$ is not $\alpha$-optimal and cannot be improved. Then,
$$\begin{align*}
\max_a \left\{r(i,a) + \alpha\sum_j P_{ij}(a)V^f(j)\right\} &= r(i, f^*(i)) + \alpha\sum_j P_{ij}[f^*(i)]V^f(j) \\
&= r(i, f(i)) + \alpha\sum_j[f(i)]V^f(j) \\
&= V^f(i) = V_\alpha(i).
\end{align*}$$