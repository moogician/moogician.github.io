---
layout: post
title: Basic Inequality
date: 2022-06-16 20:00:00
description: basic tools in making approximations
tags: probability analysis
categories: classic-tools
---

**Theorem 1** (*Markov's Inequality*) 
\begin{equation}
    X \ge 0 \wedge \mathbb{E}[X] < + \infty \Rightarrow \forall k>0.\prob{}{X \ge k} \le \frac{\mathbb{E}[X]}{k}
\end{equation}

**Theorem 2** (*Chebyshev's Inequality*)
\begin{equation}
    \mathbb{E}[X],\dots \mathbb{E}[X^j]\text{ all exists} \Rightarrow \prob{}{| X - \mathbb{E}[X]| \ge k} \le \min\limits{i \in [j]} \frac{\mathbb{E}[|X-\mathbb{E}[x]|^i]}{k^i}
\end{equation}

**Theorem 3** (*Chernoff Bound*) Let $$X_1,\dots,X_n$$ be $$n$$ random variables satisfying $$X_i \in [0,1]$$ and $$\mathbb{E}[X_i] = p_i,\forall i \in [n]$$. Suppose $$\bar{X} = \frac1n\sum_{i=1}^n X_i, p = \frac1n\sum_{i=1}^n p_i$$, for all $$\epsilon > 0$$ we have
\begin{equation}
    \prob{}{\bar{X}-p \ge \epsilon} \le \exp(-nD_{KL}(B(p + \epsilon) || B(p)))
\end{equation}

**Theorem 4** (*Hoeffding's Inequality*) Let $$X_1,\dots,X_n$$ be $$n$$ independent random variables satisfying $$X_i \in [a_i,b_i]$$. Suppose $$\bar{X} = \frac1n\sum_{i=1}^n X_i, \mu = \frac1n\sum_{i=1}^n \mathbb{E}[X_i]$$, for all $$\epsilon > 0$$ we have
\begin{equation}
    \prob{}{\bar{X}-p \ge \epsilon} \le \exp(\frac{-2n^2\epsilon^2}{\sum_{i=1}^n(b_i - a_i)^2})
\end{equation}

**Definition 5** (*Stable functions*) We call a function $$f: \mathcal{X}\rightarrow \mathbb{R}$$ a *stable function* iff 
$$
    \exists c_1,\dots c_n \in \mathbb{R}.\forall x_1,\dots x_n,x_i' \in \mathcal{X},i \in [n].|f(x_1,\dots x_i\dots x_n) - f(x_1,\dots x_i'\dots x_n)| \le c_i
$$

**Theorem 5** (*McDiarmid's Inequality*) Let $$ X_1 \dots X_n \in \mathcal{X}$$ be $$n$$ independent random variables and $$ f $$ a stable function. Then, $$\forall \epsilon > 0 $$ we have
\begin{equation}
    \prob{}{|f(\mathbf{x}) - \mathbb{E}[f(\mathbf{x})]| \ge \epsilon} \le \exp(\frac{-2\epsilon^2}{\sum_{i=1}^n c_i^2})
\end{equation}
in which $$\mathbf{x} = (X_1\dots X_n)$$.

