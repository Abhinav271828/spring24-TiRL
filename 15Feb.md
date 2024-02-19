---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 15 February, Thursday (Lecture 11)
author: Taught by Prof. Tejas Bodas
---

# Reinforcement Learning
Consider an infinite horizon discounted MDP, where the transition model $P(s' \mid s, a)$ and/or rewards $r(s, a, s')$ are unknown to us.

## Model-Based RL
Model-based RL is based on estimating $P(s' \mid s, a)$ for all values of $s, a, s'$. One type of model-based strategies rely on exploration initially, and then exploit the model that has been built from the information obtained in the first phase.  
A naive way to do this is to fix a randomized policy that explores all combinations uniformly.  
The problem with such approaches is that the time it takes to learn $\hat{P}$ may incur a lot of regret. Furthermore, empirical samples are never completely robust.

Another approach of model-based RL, certainty equivalence, is to treat the current estimate $\hat{P}$ as the ground truth and employ the optimal policy according to the current estimate $\hat{\pi}^*$.  
The problem with this methods is that we have no guarantee of the correctness of our current estimates; also, the process may converge to a local optima due to inefficient exploration.

We will not study model-based RL in this course.

## Model-Free RL
The idea that model-free RL is based on is learning $Q^f(s, a)$ directly for all $(s, a)$ pairs. Some examples of these methods are Monte-Carlo methods, TD learning, Q-learning, and actor-critic methods. Q-learning is based on value iteration, while actor-critic methods are based policy iteration.

Some methods also directly search the policy space (*e.g.* encoded as a neural network).

## Framework and Notation
The task we consider may be infinite horizon, or episodic (finite horizon) in nature. In episodic tasks, the length of the episode may be random.

The agent may be directly interacting with an environment or a simulator, or collecting actual rewards and employing policies real-time (online learning). On the other hand, the agent may be a passive entity with data about $(S_t, A_t, R_t, S_{t+1})$ (offline learning).

# Model-Free Strategies
## Policy Evaluation
We know that the value of a policy $\pi$ in an infinite horizon discounted MDP setting is given by
$$\begin{align*}
V^\pi(s) &= \mathbb{E}\left[\sum_{t=0}^\infty \alpha^t r(s_t, \pi(s_t) \mid s_0 = s)\right] \\
&= r(s, \pi(s)) + \alpha\sum_j P(j \mid s, \pi(s)) V^\pi(j).
\end{align*}$$
For an episodic problem, the summation goes up to time $T$ instead of infinity.

### Naive Policy Evaluation
Our goal here is to estimate $V^\pi(s)$ for the policy $\pi$.

In the episodic case, we start in state $s_0$, run the task until the end, and then calculate the reward as
$$G_0 = \sum_t \alpha^tr_t.$$
We repeat this $k$ times and estimate
$$\hat{V}_k(s) = \frac{\sum_{i=0}^k G_0^i}{k}.$$
In incremental form,
$$\hat{V}_k(s) = \hat{V}_{k-1}(s) + \frac1k(G_0^k - \hat{V}_{k-1}(s)).$$

A similar process can be followed to estimate $Q^\pi(s, a)$.

To avoid the discount factor $\frac1k$ vanishing, we sometimes use a different function $\alpha_k$ as the discount.