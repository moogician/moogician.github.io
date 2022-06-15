---
layout: post
title: Håstad's Switching Lemma
date: 2022-06-13 20:00:00
description: Håstad's switching lemma and its applications
tags: complexity circuit-complexity TCS
categories: classic-tools
---

<link rel="stylesheet" href="/assets/css/amsmath.css">

## Håstad's Switching Lemma

### General Idea
A general two-level circuit can be seen as either a $$CNF$$ or a $$DNF$$. 
If the fan-in is restricted, then the circuit becomes a $$k-CNF$$ or $$k-DNF$$.
We can describe how a circuit simplifies under a random restriction.
This would allow us to reduce the depth of a circuit.

### Formalization and Proof
**Lemma** 1(*Håstad's Switching Lemma*)
    Suppose $$f$$ is an $$n$$ input function expressible as a $$k-DNF$$, after a random restriction $$\rho$$ of $$t$$ randomly selected inputs, for every $$s \ge 2$$:
    \begin{equation}
        \mathop{\mathrm{P}}\limits_{\rho} (f|_\rho\text{ is not expressible as an } s-CNF) \le \left(\frac{(n-k)k^{10}}n\right)^{\frac{s}2}
    \end{equation}
    in which $$f|_\rho$$ is $$f$$ under the restriction $$\rho$$.

*Proof.* 
To prove this lemma, we need to prove that the number of "bad restrictions" that restrict $$f$$ to functions not expressible as an $$s-CNF$$ is relatively small. The way we do this is to find a one-to-one mapping from such a restriction $$\rho$$ to a member in $$R_{n-t-s} \times [2]^c$$, in which $$c = O(s \log k)$$ and $$R_m$$ is the set of restrictions on $$m$$ inputs.
    We could see this as mapping $$\rho$$ to $$(\rho\pi, \text{ some auxiliary information})$$, where the auxiliary information is only necessary to compute the invert of the mapping.

Before we construct such a mapping, we need to point out an important fact.
    Since $$f|_\rho$$ is inexpressible as a $$s-CNF$$, there must be a set of variables $$\{x_i\}$$ such that by a proper restriction on these variables we could make $$f|_\rho$$ a zero function with the size of the set $$>s$$ (or we could find all such sets and $$\wedge$$ the clauses derived from each such set).
    Additionally, there isn't a subset of this set that has the same property (exists a restriction that turns the original function into zero function).
    We denote by $$\pi$$ the corresponding restriction on this set.

Next, we try to construct such a mapping.
    Suppose $$ C_1 \dots C_{m} $$ are the clauses (ordered) in the $$k-DNF$$ that is equal to $$f$$.
    Since $$f|_\rho \neq \mathbf{1}$$, the clauses are either fixed to 0 or undecided.
    Thus, from the left to the right, there is a clause $$C_{i_1}$$ that is the first to not be set to 0. 
    Let $$ \pi_1 $$ be a restriction that only restricts the free variables $$\pi$$ restricts in this clause.
    Since $$\pi$$ makes the function zero, $$\pi_1$$ must also set $$C_{i_1}$$ to zero.
    We iterate this process to get $$\pi_2\dots$$ until the total number of variables restricted is equal to $$s$$. 
    The last $$\pi_p$$ might be trimmed arbitrarily.
    Let $$\sigma_i = \neg \pi_i$$, we define the restriction in the mapping of $$\rho$$ as $$\rho\sigma_1\dots\sigma_p$$.

Next, we show how to revert this process. 
    Define $$s_i$$ as the number of variables restricted by each $$\sigma_i$$.
    We find from the left to the right the first clause that is fixed to 1.
    By the process we get the $$\sigma_i$$'s, all the variable $$\sigma_1$$ restricts is in this clause.
    We then use $$s_1 \log n$$ bits to record all the variables restricted by $$\sigma_1$$.
    We could then compute $$\rho\pi_1\sigma_2\dots\sigma_p$$ and repeat this process to find all the $$\sigma_i$$.
    For $$\sigma_p$$, the first clause might be undecided, but this would not tamper with the inversion.
    Thus, we need at most $$\sum_{i}s_i \log n = s\log n$$ auxiliary bits.

With such a one-to-one mapping, we know that there are at most $$\binom{n}{t+s}2^{t+s}\cdot 2^{s\log n} = \binom{n}{t+s}2^{t+s}n^s$$ such "bad restrictions".
    Thus, the probability that $$\rho$$ is a bad restriction is at most 

    \begin{equation}
        \frac{\binom{n}{t+s}2^{t+s}n^s}{\binom{n}{t}2^t} \le \left(\frac{(n-k)k^{10}}n\right)^{\frac{s}2}
    \end{equation}

The last inequality needs some estimate techniques.  $$\tag*{$\blacksquare$}$$

### Remarks

- This lemma also stands if $$CNF$$ and $$DNF$$ switch place.
- This lemma does not concern the *size* of $$f$$.
- In reality $$t$$ is often close to $$n$$, say, $$n-\sqrt{n}$$.

## Example Applications

One of the most well-known applications of this lemma is that

**Theorem 1** (*Parity is not in AC0*)
    $$ \oplus \notin \mathrm{\mathbf{AC}}^0 $$, in which $$ \oplus (x_1,\dots x_n) = \sum_{i} x_i \text{ (mod 2)}$$.

*Proof.* Suppose we have a circuit in $$\mathrm{\mathbf{AC}}^0$$ that could compute $$\oplus$$. 
    Without loss of generality, suppose the circuit satisfies some conditions:
    - All fan-outs are 1
    - There is no $$\neg$$ gates in the circuit
    - The $$\wedge$$ and $$\vee$$ gates alternate between each layer
    - The bottom level has $$\wedge$$ gates of fan-in 1 
    We show that we could use the switching lemma to gradually reduce the depth of the circuit to 2.
    Suppose the size of the circuit has upper bound $$n^b$$.
    Consider the two bottom layers of the circuit, $$\vee$$ and $$\wedge$$. Each $$\vee$$ computes a $$CNF$$.
    For the $$i^{th}$$ step, let $$t_i = n_i - \sqrt{n_i}$$ and $$k_i = 10b2^i$$.
    Thus, after the restriction, with probability at least $$1 - \frac1{10n^b}$$, we could express the new function with a $$k_{i+1}-CNF$$.
    We could then merge the top layer of the new $$CNF$$ with the third layer in the circuit, which is also a $$\wedge$$ layer.
    We could then repeatedly use the switching lemma.
    With high probability (at least $$1-\frac{dep(C)}{10n^b}$$ by union bound), we could get a two layer circuit that has a small width $$k$$.
    Thus, we could fix the $$k$$ free variables to make the function constant.
    However, $$\oplus$$ could not be made constant by fixing $$k < n$$ variables, which is a contradiction.

Thus, $$\oplus \notin \mathrm{\mathbf{AC}}^0 \tag*{$\blacksquare$}$$

