---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 11 January, Thuday (Lecture 3)
author: Taught by Prof. Tejas Bodas
---

# Probability Theory
* Joint continuous random variables
* Conditioning on random variables
    - Conditional expectation as a random variable: Law of Iterated Expectation
    $$E_Y[E[X \mid Y = y]] = E[X].$$

## Sampling from Continuous Random Variables
Suppose that we have a cdf $F$, and we want to sample a continuous RV $X$ according to the distribution defined by $F$.  
To do this, we use a uniform RV $U$ over $[0, 1]$; we define
$$X = F^{-1}(U).$$

To prove that this definition of $X$ satisfies our need, let $F_X$ be the cdf of $X$. Then
$$\begin{align*}
F_X(x) &= P[X \leq x] \\
&= P[F^{-1}(U) \leq x] \\
&= P[U \leq F(x)] \text{ \{by monotonicity and invertibility of } F\} \\
&= F(x) \text{ \{by definition of } U\},
\end{align*}$$
as required.

## Convergence of Random Variables
Consider a sequence of random variables $\{X_n\}_\mathbb{N}$, and a sample $\omega \in \Omega$. Then, we can study the nature of the sequence $\{X_n(\omega)\}_\mathbb{N}$ (recall that an RV is a function from $\Omega$).

Now, we wish to study the convergence of this sequence. There are many definitions (modes) of the concept of convergence:

**Pointwise** or **Sure convergence.** $\{X_n\}_\mathbb{N}$ converges to $X$ pointwise if
$$\forall \omega \in \Omega, \lim_{n \to \infty} X_n(\omega) = X(\omega).$$

**Almost sure convergence.** $\{X_n\}_\mathbb{N}$ almost surely converges to $X$ if
$$P[\omega \in \Omega : \lim_{n \to \infty} X_n(\omega) = X(\omega)] = 1.$$
For example, let $\Omega = [0, 1]$ and $X_n(\omega) = \omega^n$, with $X(\omega) = 0$.

The strong law of large numbers is an example of almost sure convergence, *i.e.*, if $X_i$ are i.i.d., then $S_n = \frac1n\sum_{i=1}^n X_i$ tends to $\mu$ almost surely.

**Convergence in probability.**
$$\varepsilon > 0 : \lim_{n \to \infty} P[|X_n - X| > \varepsilon] = 0$$

**Mean-square convergence.**
$$\lim_{n \to \infty} E\left[(X_n - X)^2\right] = 0.$$

**Convergence in distribution.**
$$\lim_{n \to \infty} F_{X_n}(x) = F_X(x).$$

## Interchanging Limits and Expectation
If $X_n \to X$ almost surely, then when does
$$E\left[\lim_{n \to \infty} X_n\right] = \lim_{n \to \infty} E[X_n]$$
hold?

For a counterexample, consider $X_n = n \cdot \mathbb{1}_{\{U < \frac1n\}}$, where $U \sim \mathcal{U}(0, 1)$. Then
$$\lim_{n \to \infty} E[X_n] = 1,$$
but
$$E\left[\lim_{n \to \infty} X_n\right] = E[0] = 0,$$
since for almost all values of $U$, $X_n = 0$ for all $n$ above some $N_0$.

The Monotone Convergence Theorem states that, if $X_n$ is an increasing sequence of nonnegative RVs, *i.e.*,
$$\forall n \in \mathbb{N}, \omega \in \Omega : 0 \leq X_n(\omega) \leq X_{n+1}(\omega),$$
then $X = \lim_{n \to \infty} X_n$ exists and $E[X_n] \uparrow E[X]$ as $n \to \infty$.  
A corollary of this is that if $Y_i \geq 0$, then
$$E\left[\sum_{i=1}^\infty Y_i \right] = \sum_{i=1}^\infty E[Y_i].$$