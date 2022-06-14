---
layout: post
title: 
date: 2022-06-13 20:00:00
description: classic techniques on solving MAB problems
tags: OnlineLearning
categories: lecture-note
---

<link rel="stylesheet" href="/assets/css/amsmath.css">

<div class="theorem" text="Håstad's Switching Lemma">
    Suppose $f$ is an $n$ input function expressible as a $k-DNF$, after a random restriction $\rho$ of $t$ randomly selected inputs, for every $s \ge 2$:
    \begin{equation}
        \mathrm{\mathop{P}}\limits_{\rho} ($f|_\rho$\text{ is not expressible as a} s-CNF) \le \left(\frac{(n-k)k^10}n\right)^{\frac{s}2}
    \end{equation}
    in which $f|_\rho$ is f under the restriction $\rho$.
</div>

## Setting
Good
