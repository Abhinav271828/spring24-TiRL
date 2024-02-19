---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 19 February, Monday (Lecture 12)
author: Taught by Prof. Tejas Bodas
---

# Model-Free Strategies
## Policy Evaluation (contd.)
### First-Visit Policy Evaluation
We have seen the naive policy evaluation equation
$$\hat{V}_k(s) = \hat{V}_{k-1}(s) + \alpha_k(G_0^k - \hat{V}_{k-1}(s)),$$
where $G_0^k$ represents the total measured reward from the $k^\text{th}$ run (episode) of the task, following policy $\pi$ (omitted).  
$Q^\pi(s,a)$ can be estimated similarly.

This is extremely inefficient; we can optimize it by using all intermediate summations as datapoints for the intermediate states, since the process is Markovian.  
Here, $G_t$ represents the reward in an episode from time $t$ (the episode superscript $k$ is omitted); we have so far only considered $t = 0$. Thus the idea is to use $G_t$ to update $\hat{V}(s_t)$, if $s_t$ has not been visited already.

In summary, having generated an episode $\{(S_i, A_i, R_i)\}_{i \in [0, T-1]} \cup \{(S_T, R_T)\}$, we loop from $T-1$ to 0. At each point, we update the average of $\hat{V}(S_t)$ with $R_t$ if $S_t \notin \{S_0, \dots, S_{t-1}\}$.

### Every-Visit Policy Evaluation
We may also not check the condition $S_t \notin \{S_0, \dots, S_{t-1}\}$. This introduces a bias – certain rewards may be counted several times within a single state's average.

To combat this, we use an exploring-start mechanism, where we ensure that all $(s, a)$ pairs have a nonzero probability of being the starting combination.

However, under certain conditions, this algorithm also converges to $V^\pi(s)$.

### Estimates for State-Action Pairs
We can use the same procedures to estimate $Q^\pi(s,a)$. However, since the state-action space is much wider, we may not visit many such pairs.

Furthermore, $(s, \pi(s))$ will be visited most frequently and $(s, a \neq \pi(s))$ will be updated only in the starting case.

## Policy Improvement
### Monte Carlo Control with Exploring Starts
As we have seen, we can update the policy after evaluating it by taking
$$\pi'(s) = \argmax_{a \in \mathcal{A}} \left[r(s,a) + \alpha\sum P(s' \mid s, a) V^\pi(s')\right].$$

However, in a model-free setting, we have no access to the tpm $P$. This equation is therefore not applicable. Thus we substitute it with an estimable quantity:
$$\pi'(s) = \argmax_{a \in \mathcal{A}} \hat{Q}^\pi(s, a).$$

Thus we can use a policy iteration-like framework, where we start with an arbitrary $\pi_0$, find $\hat{Q}^{\pi_0}$, update the policy to $\pi_1$ (greedy w.r.t $\hat{Q}^{\pi_0}$), and so on. Ideally, $\pi$ converges to $\pi^*$ and $Q_\alpha(s, a)$.

We can also interleave the policy update step with the $\hat{Q}$ update step.

This converges, apparently (ICLR 2023).

### MC Control without Exploring Starts
If we do not use exploring starts, we need to ensure coverage of the state-action space during the episode itself. We achieve this by using *soft policies*, *i.e.*, policies for which $\pi(a \mid s) > 0$ for all $a \in \mathcal{A}$ and $s \in \mathcal{S}$.

Furthermore, we use $\varepsilon$-greedy updates rather than simple greedy ones. In this case, we let
$$\pi'(s) = \begin{cases}
\argmax_{a \in \mathcal{A}} Q^\pi(s, a) & \text{with probability } 1 - \varepsilon; \\
\text{random } a \in \mathcal{A} & \text{with probability } \varepsilon.
\end{cases}$$