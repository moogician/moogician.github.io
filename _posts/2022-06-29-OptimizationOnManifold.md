---
layout: post
title: Convex Optimization On Manifold
date: 2022-06-29 20:00:00
description: classic algorithms
tags: optimization convex-optimization smooth-manifold
categories: classic-results
---

{% include pseudocode.html id="RGD" code="
\begin{algorithm}
\caption{Riemannian Gradient Descent}
\begin{algorithmic}
\ENSURE: x
\FOR{$k = 0,1,2\dots$}
    \STATE{Pick a step size $\alpha_k > 0$}
    \STATE{$x_{k+1} = R_{x_k}(s_k)$, with step $s_k = -\alpha_k \text{grad}f(x_k)$}
\ENDFOR
\end{algorithmic}
\end{algorithm}
" %}
