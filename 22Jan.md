---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 22 January, Monday (Lecture 6)
author: Taught by Prof. Tejas Bodas
---

# Markov Chains
## Deterministic Dynamic Programming (contd.)
We would like a way to evaluate $V^\pi(s_0)$ for arbitrary $\pi$ and $s_0$. For this, we decompose the problems into some sub-problems.

We define

* a *sub-policy* $\bm\pi_t$ as $(\pi_t, \dots, \pi_{T-1})$.
* the value of the sub-policy $V_t^{\bm\pi_t}(s_t)$ as $\sum_{u=t}^{T-1} r(s_u, \bm\pi_u) + r_T(s_T)$. We have the special cases $V_T^{\bm\pi_T}(s) = r_T(s)$ and $V_0^\pi(s) = V^\pi(s)$. We can write a *policy evaluation equation*
$$V_t^{\bm\pi_t}(s_t) = r(s_t, \bm\pi_t) + V_{t+1}^{\bm\pi_{t+1}}(f(s_t, \pi_t)).$$
* the optimum reward of a state $V_t(s_0) = \max_{\pi_t}V_t^{\pi_t}(s_0)$. The Bellman Optimality equation allows us to evaluate this as follows:
$$V_t(s) = \max_{a \in \mathcal{A}} \left[r(s, a) + V_{t+1}(f(s, a))\right].$$

To derive the optimal policy from this theorem, we use argmax instead of max and record the action at each step.
$$\pi_t^* = \argmax_{a \in \mathcal{A}} \left[r(s, a) + V_{t+1}(f(s, a))\right].$$

The Principle of Optimality states that the subsolutions of an optimal solution to a problem are optimal solutions to subproblems of the problem.

We can generalize this formulation by allowing 

## Stochastic Dynamic Programming
For stochastic DP problems (also known as Markov decision problems or MDPs), we use the same notations and concepts as in deterministic ones except that the plant equation is stochastic:
$$S_{t+1} = f_t(S_t, A_t, W_t),$$
where $W_t$ is i.i.d noise. This implies that the state transitions are Markovian (given an action for each state):
$$P(S_{t+1} = s' \mid S_t = s, A_t = a) = P(s' \mid s, a)$$

### Types of Policies
We have seen that a policy $\pi = (\pi_t : t = 0, \dots, T-1)$ specifies actions $\pi_t \in \mathcal{A}$ for any time $t$.

If $\pi : t \to \mathcal{A}$, then it is *state-independent* and *deterministic*.

If $\pi : t \to \mathcal{P}(\mathcal{A})$, then it is a state-independent *randomized* policy.

If $\pi : \mathcal{H}_t \to \mathcal{P}(\mathcal{A})$ (where $\mathcal{H}_t = (S_{1:t}, A_{1:t})$ is the history), then it is *history-dependent*, randomized policy.

If $\pi : S_t \times t \to \mathcal{A}$, then it is a Markovian, nonstationary, deterministic policy.

If $\pi : S_t \to \mathcal{A}$, it is Markovian, stationary and deterministic.

### Evaluating a Policy
Policies are evaluated in the same way as for deterministic DP problems, with an additional expectation.
$$V^\pi(s_0) = \mathbb{E}_{s_0} \left[r(s_0, \pi_0) + \cdots + r_{T-1}(s_{T-1}, \pi_{T-1}) + r_T(s_T)\right]$$
$\mathbb{E}_{s_0}$ denotes the expectation over all possibilities conditioned on starting at $s_0$. We will hereinafter assume this in all statements related to stochastic DP problems.

It can be proved that the best history-dependent strategy does not improve upon the best Markovian strategy.
$$V(s_0) = \sup_{\pi \in \Pi^\text{HR}} V^\pi(s_0) = \sup_{\pi \in \Pi^\text{MD}}V^\pi(s_0).$$