---
title: Reinforcement Learning
subtitle: An Introduction
authors: |
         | Richard S. Sutton
         | Andrew G. Barto
numbersections: true
---

\newcommand{\mean}[2]{\mathbb{E}_{#1}\left[#2\right]}

**Part I: Tabular Solution Methods**

* Multi-Armed Bandits
    - Action-Value Methods
        - Greedy vs. $\varepsilon$-greedy Methods
    - Estimation
        - Incremental Estimation
        - Recency-Weighted Estimation
        - Initial Value Bias
    - Selection
        - UCB Action Selection
    - Gradient Bandit Algorithms
    - Associative Search
* Finite MDPs
    - Concepts
        - Agent-Environment Interface
        - Goals and Rewards
        - Returns and Episodes
        - Policies and Value Functions
    - Bellman Equations: $v_\pi; q_\pi; v_*; q_*$

# Multi-Armed Bandits
RL uses training information that provides evaluative rather than instructive feedback – this creates the need for exploration.  
We consider a nonassociative setting – the agent needs to learn to act in just one situation.

## A $k$-armed Bandit Problem
A $k$-armed bandit is named by analogy to a slot machine with $k$ levers, each of which corresponds to an action which yields a different reward.

Each action has an expected reward, called its *value*, defined as
$$q_*(a) = \mean{}{R_t \mid A_t = a}.$$
Our estimate of this value at time $t$ is denoted $Q_t(a)$.

## Action-Value Methods
Action-value methods are those that estimate the values of actions and use these estimates to select actions.

One natural way to estimate $Q_t(a)$ is by averaging the reward:
$$Q_t(a) = \frac{\sum_{i=1; A_i = a}^{t-1}R_i}{\sum_{i=1; A_i = a}^{t-1} 1}.$$

The simplest action selection rule is to greedy select the action with the highest estimated value:
$$A_t = \operatorname*{argmax}_a Q_t(a).$$
We may also use $\varepsilon$-greedy methods, which select a non-greedy action with probability $\varepsilon$, to ensure that all actions are sampled and we converge to $q_*(a)$.

The following subsections treat either the estimation method (Estim.) or the selection method (Selec.).

## The 10-armed Testbed
Greedy methods usually don't select the optimal action or achieve optimal reward.

A higher value of $\varepsilon$ means that the initial stages see high returns, but lower values overtake these in the long run.

The advantage of $\varepsilon$-greedy methods comes from the variance in the reward, and is (roughly speaking) proportional to it.

## (Estim.) Incremental Implementation (Efficient Estimation)
We have seen that the naive way to estimate reward is to average it. Assuming a single action,
$$Q_n = \frac{R_1 + \cdots + R_{n-1}}{n-1}.$$

This is highly memory- and computation-intensive. An incremental update process is more efficient:
$$Q_{n+1} = Q_n + \frac1n[R_n - Q_n].$$

This is a specific case of a general form of update rule which we will encounter repeatedly:
$$\text{newEstimate} \leftarrow \text{oldEstimate} + \text{stepSize}[\text{target} - \text{oldEstimate}].$$

## (Estim.) Tracking a Nonstationary Problem
In a nonstationary problem (where the reward probabilities change over time), it makes sense to give more weight to recent rewards. This is achieved by using a constant step-size $\alpha$ in place of $\frac1n$. This results in the estimate being an exponential recency-weighted average of the form
$$Q_{n+1} = (1-\alpha)^n Q_1 + \sum_{i=1}^n \alpha(1-\alpha)^{n-1} R_i.$$

Necessary conditions on the $\alpha_n$ sequence for convergence are that its sum should diverge and the sum of the squares of the elements should converge – this is satisfied for the sample-average case of $\alpha_n = \frac1n$ but not in the constant case.  
In the latter, therefore, the estimates do not converge but fluctuate according to the most recent rewards (which is not necessarily undesirable).

## (Estim.) Optimistic Initial Values
There is always a bias introduced by the initial estimate $Q_1$. This disappears in the sample-average case but not in the recency-weighted case. Thus these values are parameters that the user needs to select.

Using high initial values encourages exploitation (temporarily) as all actions yield "low" rewards initially. This is effective on stationary problems, but there are better ways to encourage exploration in general.

## (Selec.) Upper-Confidence-Bound Action Selection
We have seen greedy and $\varepsilon$-greedy action selection methods.

We can select actions non-greedily according to their potential for being optimal. One effective way of this is the decision given by
$$A_t = \operatorname*{argmax}_a \left[Q_t(a) + c\sqrt{\frac{\ln t}{N_t(a)}}\right].$$
The parameter $c$ controls the degree of exploration. The sqrt term is a measure of the variance in the estimate of $a$'s value (it decreases the more we pick $a$); thus the objective function is an "upper bound" on the possible true value of $a$.

## (Estim. + Selec.) Gradient Bandit Algorithms
Rather than directly selecting actions, we may assign them *preferences* $H_t(a)$ and select them according to a probability distribution
$$\pi_t(a) = \operatorname{softmax}(H_t)(a).$$

We can apply stochastic gradient ascent to this process by comparing the obtained reward with the current average reward.
\begin{align*}
H_{t+1}(A_t) &= H_t(A_t) + \alpha(R_t - \bar{R}_t)(1-\pi_t(A_t)) \\
H_{t+1}(a) &= H_t(a) - \alpha(R_t - \bar{R}_t)pi_t(a) \text{ for all } a \neq A_t.
\end{align*}

## Associative Search
Associative search tasks involve learning a mapping from situations (states) to actions, rather than actions alone. These differ from the full reinforcement learning in that actions have no effect on the next state, but only on the reward.

Such problems are considered in the next chapter.

# Finite Markov Decision Processes
Finite MDPs involve evaluative feedback, but in an associative setting. Here, actions influence immediate rewards as well as subsequent states, and therefore involve delayed reward and the tradeoff of this with immediate reward.

Here, the estimanda are $q_*(s, a)$ (the value of an action in a state) and $v_*(s)$ (the value of a state given optimal action selection).

## The Agent-Environment Interface
We assume a discrete-time setting, where at each timestep, the agent can observe the state $S_t \in \mathcal{S}$, select an action $A_t \in \mathcal{A}(s)$, and in the next timestep receive a reward $R_{t+1} \in \mathcal{R} \subset \mathbb{R}$ and observe the next state $S_{t+1}$.

The dynamics of an MDP are determined by the probability
$$p(s', r \mid s, a) = \Pr\{S_t = s', R_t = r \mid S_{t-1} = s, A_{t-1} = a\}.$$

This matrix can be marginalized to find various other quantities associated with the process, like
$$p(s' \mid s, a) = \Pr\{S_t = s' \mid S_{t-1} = s, A_{t-1} = a\}$$
$$r(s, a) = \mean{}{R_t \mid S_{t-1} = s, A_{t-1} = a}$$
$$r(s, a, s') = \mean{}{R_t \mid S_{t-1} = s, A_{t-1} = a, S_t = s'}$$

## Goals and Rewards
The reward hypothesis, a distinct feature of RL, is a formalization of the notion of "purpose" for the agent – the agent maximizes the expected value of a cumulative sum of a received signal.

## Returns and Episodes
The expected return is some function of the reward sequence, in the simplest case a direct sum of the rewards
$$G_t = R_{t+1} + \cdots + R_T.$$
This is most naturally applicable in cases where the interaction can be delineated into episodes, which are repeated with resets in between – these are called episodic tasks.

Continuing tasks, on the other hand, may go on without limit; here, in order to ensure convergence of $G_t$ we discount the future rewards:
$$G_t = \sum_{k=0}^\infty \gamma^k R_{t+k+1}.$$

## Policies and Value Functions
We have seen how value functions are defined for states and state-action pairs.  
A *policy* is a mapping from states to probability distributions over actions.

The value function of a state $s$ under a policy $\pi$ is
$$v_\pi(s) = \mean{\pi}{G_t \mid S_t = s};$$
correspondingly for a state-action pair $(s, a)$, we have
$$q_\pi(s, a) = \mean{\pi}{G_t \mid S_t = s, A_t = a}.$$

We call estimation methods for these functions that rely on repeated sampling *Monte Carlo methods*.

These value functions satisfy recursive relationships; for example,
$$v_\pi(s) = \sum_{a \in \mathcal{A}(s)} \pi(a \mid s) \sum_{s', r} p(s', r \mid s, a) [r + \gamma v_\pi(s')].$$
This is called the Bellman equation for $v_\pi$; $v_\pi$ is the unique solution to this equation.

**Exercise 3.17.**
$$q_\pi(s, a) = \sum_{s', r} p(s', r \mid s, a) \left[r + \gamma \sum_{a' \in \mathcal{A}(s')}\pi(a' \mid s')q_\pi(s', a')\right]$$

**Exercise 3.18.**
$$v_\pi(s) = \sum_{a \in \mathcal{A}(s)} \pi(a \mid s) q_\pi(s, a).$$

**Exercise 3.19.**
$$q_\pi(s, a) = \sum_{s', r} p(s', r \mid s, a)[r + v_\pi(s')].$$

## Optimal Policies and Optimal Value Functions
Finite MDPs admit a precise definition for optimal policies. We say that $\pi$ is at least as good as $\pi'$ if $v_\pi(s) > v_{\pi'}(s)$ for all $s \in \mathcal{S}$; the root(s) of this partial ordering is (are) then the optimal policy(ies) $\pi_*$.  
They all share the state-value function $v_*$, which satisfies
$$v_*(s) = \max_\pi v_\pi(s),$$
and the same action-value function, which satisfies
$$q_*(s, a) = \max_\pi q_\pi(s, a).$$

The relationship between these two quantities is
$$q_*(s, a) = \mean{}{R_{t+1} + \gamma v_*(S_{t+1}) \mid S_t = s, A_t = a}.$$

They also satisfy recursive Bellman optimality equations
\begin{align*}
v_*(s) &= \max_a \sum_{s', r} p(s', r \mid s, a)[r + \gamma v_*(s)] \\
q_*(s, a) &= \sum_{s', r} p(s', r \mid s, a)[r + \gamma\max_{a'}q_*(s', a')].
\end{align*}

Once the optimal value functions are obtained, it is easy to see that in a finite MDP, any policy that assigns nonzero probabilities only to the maximum-value actions (*i.e.*, which is greedy w.r.t $v_*$) is an optimal policy.

Note that such a procedure is rarely applicable in real-life scenarios, because of three core assumptions:

* we accurately know the dynamics of the environment;
* we can feasibly solve the Bellman equations symbolically;
* the system is Markovian.