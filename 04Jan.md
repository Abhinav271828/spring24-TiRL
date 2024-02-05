---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 04 January, Thursday (Lecture 1)
author: Taught by Prof. Tejas Bodas
---

# Introduction
## Perspectives of RL
### Markovian Decision Process
RL is essentially an MDP where the transitions are unknown.

An MDP is a Markov chain controlled by actions to maximise some reward.

A Markov chain is a collection of dependent random variables, or a stochastic process – the future state can depend only on the present state and not on any of the past states.

### Sequential Decision Problem
RL constitutes an interaction between an agent and the environment. The actions taken by the agent influence the environment, which in turn changes the reward obtained by the agent.

In effect, RL is the study of learning to perform tasks by trial and error while interacting with the environment.

This perspective of RL leads to a state-action-reward-state-action (SARSA) model.

The agent has to select a sequence of actions to maximise the total reward under environment uncertainty. This involves balancing immediate and future gains.

## Key Ingredients
An RL algorithm consists of

* A model for the environment
    * A transition model
    * A reward model
* A policy for the agent
* A value function for the policy and/or states

### Transition Model
This component of the model represents the dynamics of the environment. The most general model for state transitions, a history-based model, is usually denoted
$$S_{t+1} = f(\mathcal{H}_t, W_t),$$
where
$$\mathcal{H}_t = \{S_1, A_1, \dots, S_t, A_t\}$$
is the history, and $W_t$ represents a potential source of randomness.

A category of transition models that eliminate the dependence on history, Markovian transition models, are usually denoted
$$S_{t+1} = f(S_t, A_t, W_t),$$
where $W_t$ is i.i.d noise.

In this case, the Markov property holds:
$$P[S_{t+1} = s' \mid S_t, A_t, S_{t-1}, \dots, A_1] = P[S_{t+1} = s' \mid S_t, A_t].$$

Note that this $f$ is unknown.

### Reward Model
Typically, we assume that the reward $R_t$ at time $t$ can be written as
$$R_{t+1} = g(S_t, A_t),$$
*i.e.*, the reward depends on the current state and action. However, it is also common to have a model of the form
$$R_{t+1} = g(S_t, A_t, S_{t+1}).$$

The *reward hypothesis* states that the goal in RL is to maximise the expected total rewards under model uncertainty.

### Policy
A policy $\pi$ represents a strategy for choosing an action at each timestep. They may be history-based, Markovian, randomised, stationary, etc.

The optimal policy is denoted $\pi^*$; it offers the highest expected total reward.  
When the transition model is known (as in the case of MDPs) the optimal policies turn out to be Markovian, deterministic or stationary. However, in RL, the optimal policy is unknown, since the model is unknown.

### Value Function
When following a policy, we might need to know the quality, or "value," of the states being visited. The value function, denoted $V^\pi(s)$, quantifies the *expected total (discounted) reward* from policy $\pi$ starting from state $s$. A discount is applied to future timesteps to weigh future rewards less than equal rewards in the present.  
The optimal expected total discounted reward, which is equivalent to $V^{\pi^*}(s)$, is usually denoted $V(s)$.

Another important quantity is the state action value function $Q^\pi(s, a)$, which quantifies the expected total discounted reward from policy $\pi$ *after* taking action $a$ from state $s$.

The main objective in MDP is to obtain expressions for the optimal value function
$$V(s) = \max_{\pi \in \Pi} V^\pi(s)$$
and optimal policy.
$$\pi^* = \argmax_{\pi \in \Pi} V^\pi(s).$$

Under model uncertainty, the objective in RL is to quickly learn $Q(s, a)$. This is called $Q$-learning.

## Classification of RL Problems
* **Value function-based algorithms.** These algorithms focus on learning $Q(s, a)$ quickly, *e.g.*, value iteration.
* **Policy-based algorithms.** These algorithms focus on learning $\pi^*$ quickly, *e.g.*, policy iteration.
* Some algorithms can do both of the above, *e.g.*, actor-critic algorithms.
* **Model-based algorithms.** These algorithms try to learn the environment (or the model, $f$) first. All other types of algorithms are *model-free*.