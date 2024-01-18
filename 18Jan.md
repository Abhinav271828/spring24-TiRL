---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 18 January, Thuday (Lecture 4)
author: Taught by Prof. Tejas Bodas
---

$\newcommand{\comm}[2]{#1 \leftrightarrow #2}$

# Markov Chains
## Discrete-Time Markov Chains
A discrete time stochastic process $\{X_n, n \in \mathbb{Z}_+\}$, is a collection of random variables. These may not be independent or identically distributed.

A stochastic process is a Markov chain if for any $n_1 < \cdots n_k < n$, we have
$$P(X_n = x_n \mid X_{n_1} = x_1, \dots, X_{n_k} = x_k) = P(X_n = x_n \mid X_{n_k} = x_k).$$
In other words, the future states only depend on the present state and not on any of the past state. This is called the Markov property.

To define a Markov chain on state space $\mathcal{S}$, therefore, we need two parameters: a probability transition matrix
$$P \in [0, 1]^{|\mathcal{S}| \times |\mathcal{S}|},$$
where $P_{ij} = P(X_{t+1} = s_j \mid X_t = s_i)$, and an initial distribution
$$\bar{\mu} = (\mu_1, \dots, \mu_{|\mathcal{S}|}),$$
where $\mu_i = P(X_0 = s_i)$.

## Chapman-Kolmogorov Equations for DTMC
Consider a Markov coin with TPM $P \in [0, 1]^{2 \times 2}$.

If we need to find $P(X_2 = 1 \mid X_0 = 1)$, we simply need to sum over all values of $X_1$. Thus we have
$$\begin{align*}
P(X_2 = 1 \mid X_0 = 1) &= P(X_2 = 1 \mid X_1 = 1) P(X_1 = 1 \mid X_0 = 1) \\
&+ P(X_2 = 1 \mid X_1 = -1) P(X_1 = -1 \mid X_0 = 1) \\
&= p_{1, 1}^2 + p_{-1, 1}p_{1, -1}.
\end{align*}$$

This is in fact an entry in the two-step TPM, $P^{(2)}$. It is easy to verify that the entries in this matrix are the same as those in $P^2$.  
The Chapman-Kolmogorov equations are a generalization of this:
$$P^{(n+l)} = P^{(n)}P^{(l)}.$$

## Classification of States
For a Markov process with state space $\mathcal{S}$ and TPM $P$, we say that state $j$ is accessible from state $i$ (denoted by $i \to j$) if $P^n_{ij} > 0$ for some $n$.  
If $i \to j$ and $j \to i$, then we say that $i$ and $j$ communicate (denoted $\comm{i}{j}$).  
If $\comm{i}{j}$ for all $i, j \in \mathcal{S}$, then the Markov chain is said to be *irreducible*.

We define $F_{ij}$ as the probability of *ever* returning to $j$ after starting from $i$. A state $i$ is *recurrent* if $F_{ii} = 1$. A state which is not recurrent is called *transient*, *i.e.*, $F_{ii} < 1$.  
Note that if $\comm{i}{j}$ and $i$ is recurrent, then $j$ is recurrent.

## Limiting Probabilities
The limiting probability matrix is the limit of the TPM $P$ as time progresses, *i.e.*, $\lim_{n \to \infty} P^{(n)}$. We note that
$$\lim_{n \to \infty} P^{(n)}_{ij} = \left[\lim_{n \to \infty} P^n\right]_{ij}.$$

For irreducible Markov chains, if the limit of $P^{(n)}$ exists, all rows become identical. This row, denoted $\pi = (\pi_1, \dots, \pi_{|\mathcal{S}|})$, is called the *limiting distribution*. In other words,
$$\pi_j = \lim_{n \to \infty} P^{(n)}_{ij}$$
for any $i$.

We denote by $\pi$ the stationary distribution of a Markov chain, *i.e.*, the distribution that does not change after transitions. We note that $\pi$ satisfies the equation
$$\pi = \pi P.$$
$\pi P$ represents the PMF of $X_1$ once $X_0$ is drawn from $\pi$.  
This means that $\pi$ is the eigenvector of $P$ that corresponds to the eigenvalue 1.

Note that the limiting distribution may not exist, but the stationary distribution always does. In fact, whenever the limiting distribution exists, it is the same as the stationary distribution.
