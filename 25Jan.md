---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 25 January, Thursday (Lecture 7)
author: Taught by Prof. Tejas Bodas
---

# Markov Chains
## Stochastic Dynamic Programming
### Evaluating a Policy (contd.)
We have seen that for a finite-horizon stochastic DP problem, we can evaluate a policy as
$$V^\pi(s_0) = \mathbb{E}_{s_0} \left[r(s_0, \pi_0) + \cdots + r_{T-1}(s_{T-1}, \pi_{T-1}) + r_T(s_T)\right].$$

We can also discount future rewards usign a discount factor $\alpha$:
$$V_\alpha^\pi(s_0) = \mathbb{E}_{s_0} \left[\alpha^0r(s_0, \pi_0) + \cdots + \alpha^{T-1}r_{T-1}(s_{T-1}, \pi_{T-1}) + \alpha^Tr_T(s_T)\right].$$

### Subproblems
As in the deterministic case, we define
$$V_t^{\pi_t}(s_t) = \mathbb{E}\left[\sum_{u=t}^{T-1} r(S_u, \pi_u) + r_T(S_T)\right],$$
which leads to the special cases $V_0^\pi(s_0) = V^\pi(s_0)$ and $V_T^{\pi_T}(s_T) = r(S_T)$.

We use the subproblem formulation to recursively evaluate $V^\pi(s_0)$.
$$V_t^{\pi_t}(s_t) = r(s_t, \pi_t) + \mathbb{E}_{s, \pi_t} \left[V_{t+1}^{\pi_{t+1}}(s)\right],$$
where the second term is
$$\mathbb{E}_{s, \pi_t} \left[V_{t+1}^{\pi_{t+1}}(s)\right] = \sum_j P(j \mid s, \pi_t) \cdot V_{t+1}^{\pi_{t+1}}(j).$$

### Bellman Optimality Equations
We define $V_t(s_t) = \max_{\pi_t} V_t^{\pi_t}(s_t)$.
From this we can derive the Bellman optimality equation
$$V_t(s) = \max_{a \in \mathcal{A}} \{r(s, a) + \mathbb{E}_{s,a}[V_{t+1}(S')]\}.$$

### Q-Function Formulation
Suppose that we are in state $s$ at time $t$, and we take action $a$. The best possible cumulative reward starting from this premise is denoted $Q_t(s, a)$. It is easy to see that
$$Q_t(s, a) = r_t(s, a) + \mathbb{E}_{s, a}[V_{t+1}(f_t(s, a, W_t))].$$

The relation between $V_t$ and $Q_t$ is
$$V_t(s) = \max_{a \in \mathcal{A}} Q_t(s, a).$$