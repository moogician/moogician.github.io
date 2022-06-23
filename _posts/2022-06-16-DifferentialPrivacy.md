---
layout: post
title: Differential Privacy
date: 2022-06-16 20:00:00
description: How to ensure privacy in publicly distributed data
tags: privacy cryptography
categories: problem classic-tools
---

# Problem Setting
When distributing some data calculated from a dataset publicly, we need to ensure that any malicious party cannot recover from the released data any information about personal data in the original dataset. Mathematically speaking, we need to ensure that our algorithm has *Differential Privacy*.

First, we need to define a special pair of sets called neighbouring set:

**Definition 1** (*Neighbouring Sets*)
Two sets $$S, S'$$ are neighbouring sets iff there exists $$\pi: [|S|]\rightarrow S,\pi':[|S'|]\rightarrow S'$$ such that $$\exists! n \pi(n) \neq \pi'(n)$$, or that $$S$$ and $$S'$$ differs only by one element.

With this definition, we could define DP:

**Definition 1** (*Differential Privacy*)
Let A be a randomized algorithm, $$A : X^n → R$$, with input $$D$$, we say $$A$$ satisfies $$\epsilon$$-differential privacy if for all neighboring sets $$D, D′ \in X^n$$, and all set $$S \subseteq A(X^n)$$, we have
\begin{equation}
\prob{x\in D}{A(x) \in S} \le e^{\epsilon}\prob{x\in D'}{A(x) \in S}
\end{equation}

