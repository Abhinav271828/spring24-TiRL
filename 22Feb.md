---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 22 February, Thursday (Lecture 13)
author: Taught by Prof. Tejas Bodas
---

# Model-Free Strategies
## Policy Evaluation (contd.)
### Temporal Difference Prediction
We have seen the update equation
$$V(S_t) \leftarrow V(S_t) + \beta[G_t - V(S_t)].$$
The update cannot be made without having rolled out the complete episode, regardless of the strategy.

Instead of this, we would like to approximate the true gain $G_t$ with $R_{t+1} + \alpha V(S_{t+1})$.
$$\text{TD}(0) : V(S_t) \leftarrow V(S_t) + \beta[R_{t+1}+\alpha V(S_{t+1})-V(S_t)].$$

The $R_{t+1}+\alpha V(S_{t+1})-V(S_t)$ term is called the TD error. This method converges to $V^\pi$ and is typically faster than the MC methods.

This can be generalized by approximating $G_t$ with higher-order terms, *e.g.*
$$G_t(n) = R_{t+1} + \alpha R_{t+2} + \alpha^2 R_{t+3} + \dots + \alpha^{n+1}V(S_{t+n+1}).$$
This gives us the $\text{TD}(n)$ update procedure, generalized from above.

Further, we can combine all these updates using a geometric distribution. $\text{TD}(\lambda)$ uses
$$G_t(\lambda) = (1-\lambda)\sum_{n=1}^\infty \lambda^{n-1}G_t(n).$$
This procedure also converges.

## Policy Improvement (contd.)
### SARSA (On-Policy TD Control)
It is straightforward to write an analogous update rule for $Q(s,a)$:
$$Q(S_t, A_t) \leftarrow Q(S_t, A_t) + \beta[R_{t+1}+\alpha Q(S_{t+1},A_{t+1}) - Q(S_t, A_t)].$$
Now we can apply techniques like exploring starts and $\varepsilon$-greedy policies.

However, here, at each step we update the policy as well as the $Q$ estimate.

This can be seen as a stochastic version of Euler's method, which we know to converge. From this, it can be shown that this update process leads to a value of $Q(S_t, A_t)$ such that
$$\mathbb{E}[R + \alpha Q(S', A')] = Q(S, A).$$

$Q$-learning is similar to this, but independent of the policy – it may be described as "off-policy TD control."