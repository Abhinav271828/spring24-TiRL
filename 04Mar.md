---
title: Topics in Reinforcement Learning
subtitle: |
          | Spring 2024, IIIT Hyderabad
          | 04 March, Monday (Lecture 14)
author: Taught by Prof. Tejas Bodas
---

# Continuous (Dynamical) Systems: State Space
The state of a system is the minimum information required to describe it. For a Markov system, we need to be able to determine future states (or a probability distribution thereover) given the current state and the current action.

In the most general sense (in the continuous case), we have a state *vector* $X \in \mathbb{R}^n$ and an input *vector* $U \in \mathbb{R}^n$, and then we can define (for a Markovian system) the dynamics in terms of some function $f$ of the current state and input.
$$\frac{dX}{dt} = \dot{X} = f(X, U).$$

Examples of this include RLC circuits and spring-mass-damping systems.

# Linear Systems
**NB: This is not restricted to dynamical systems.**

A system is called linear if it satisfies superposition and homogeneity.

A system obeys superposition if, for any inputs $u_1, u_2$ that have outputs $y_1(t), y_2(t)$ respectively, the input $u_1 + u_2$ will have the output $y_1(t) + y_2(t)$. For example, a system defined by
$$\frac{dy}{dt} + ay = bu$$
obeys superposition.