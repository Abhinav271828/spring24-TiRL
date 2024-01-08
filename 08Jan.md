---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 08 January, Monday (Lecture 2)
author: Taught by Prof. Tejas Bodas
---

# Probability Theory
## $\sigma$-Algebras as Domains for Probability
We can define a probability space in terms of measure theory as $(\Omega, \mathcal{F}, \mathbb{P})$. Here, $\mathcal{F}$ is a collection of subsets of the sample space $\Omega$, all of which must satisfy
$$\emptyset \in \mathcal{F}$$
$$A \in \mathcal{F} \implies A^c \in \mathcal{F}$$
$$A_1, A_2, \dots \in \mathcal{F} \implies \bigcup_{n=1}^\infty A_n \in \mathcal{F}$$

This $\sigma$-algebra encodes desirable intuitive properties of probability measures, like closure under the formation of complements (property 2), countable unions (property 3), and countable intersections (provable from properties 2 and 3).

When $\Omega$ is countable or finite, we let $\mathcal{F} = 2^\Omega.$

The measure $\mathcal{P}$ is a function $\mathcal{F} \to [0, 1]$ that satisfies
$$\mathbb{P}(\emptyset) = 0.$$
$$\mathbb{P}(\Omega) = 1.$$
$$\mathbb{P}\left(\bigcup_{i=1}^\infty A_i\right) = \sum_{i=1}^\infty \mathbb{P}(A_i) \text{ for disjoint } A_i.$$

## Conditional Probability
The conditional probability of an event $B$ given an event $A$ is
$$P(B \mid A) = \frac{P(A \cap B)}{P(A)}$$
for $P(A) > 0$.

## Independence and Mutual Exclusivity
Two events $A$ and $B$ are independent iff $P(A \mid B) = P(A)$ and $P(B \mid A) = P(B)$.

Two events $A$ and $B$ are mutually exclusive iff $P(A \cap B) = 0$.

## Random Variable
Random variables allow us to work with a simpler space $\Omega'$ in a convenient way.  
If $\Omega'$ is countable, then we have a discrete random variable; else, if it is uncountable, we have a continuous random variable.

Discrete RVs have probability mass functions (PMFs) as distributions, denoted $p_X$, while continuous RVs have probability density functions (PDFs) as distributions, denoted $f_X$.

## Expectation, Moments, and Variance
The expected value of a random variable is defined as
$$E[X] = \sum_{\Omega'} x\cdot p_X(x)$$
for discrete variables, and
$$E[X] = \int_{\Omega'} x \cdot f_X(x) dx.$$
The $n^\text{th}$ moment of a random variable $X$ is defined as $E[X^n]$.

THe variance of a random variable $X$ is defined as
$$\text{Var}(X) = E\left[\left(X - E[X]\right)^2\right] = E[X^2] - E[X]^2.$$

Examples of discrete RVs:

* Bernoulli distribution, which has range $\Omega' = \{0, 1\}$, where 1 occurrs with probability $p$.
* Binomial distribution, which has range $\Omega' = \{0, \dots, n\}$, where $k$ occurs with probability ${n \choose k}p^k(1-p)^{n-k}$.

Examples of continuous RVs:

* Uniform distribution
* Exponential distribution
* Gaussian distribution

# Multiple Random Variables
A joint probability distribution is defined as $p_{XY}(x,y) = \mathbb{P}\{X = x \wedge Y = y\}.$ The *marginal* PMFs are defined as
$$p_X(x) = \sum_y p_{XY}(x, y)$$
and
$$p_Y(y) = \sum_x p_{XY}(x, y).$$
The corresponding property holds applied to $f_{XY}(x, y)$, with integrals instead of sums.

Two RVs are independent iff $p_{XY} = p_X(x)p_Y(y)$.