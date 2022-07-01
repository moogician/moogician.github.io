---
layout: post
title: Convex Optimization On Manifold
date: 2022-06-29 20:00:00
description: classic algorithms
tags: optimization convex-optimization smooth-manifold
categories: classic-results
---
# First-Order Methods
## Riemannian Gradient Descent (RGD)

### Algorithm
{% include pseudocode.html id="RGD" code="
\begin{algorithm}
\caption{Riemannian Gradient Descent}
\begin{algorithmic}
\ENSURE{$x_0 \in \mathcal{M}$}
\FOR{$k = 0,1,2\dots$}
    \STATE{Pick a step size $\alpha_k > 0$}
    \STATE{$x_{k+1} = R_{x_k}(s_k)$, with step $s_k = -\alpha_k \text{grad}f(x_k)$}
\ENDFOR
\end{algorithmic}
\end{algorithm}
" %}

### Problem setting

- Lower Bound
- Good Step Picking Algorithm: $$f(x_k) - f(x_{k+1}) \ge c\lVert \text{grad}f(x_k) \rVert$$
- For a given $$S \subset T\mathcal{M}, \exists L.\forall (x,s) \in S. f(R_x(s)) \le f(x) + \langle \text{grad}f(x), s\rangle + \frac12 \lVert s\rVert^2$$

## Riemannian Stochastic Gradient Descent (RSGD) 
[Arxiv Link](https://arxiv.org/pdf/1111.5280.pdf)

## Riemannian Accelerated Gradient Descent (RAGD) 
[Arxiv Link](https://arxiv.org/pdf/1806.02812.pdf)

## Riemannian Langevin Diffusion (RLD) 
[Paper Link](https://proceedings.neurips.cc/paper/2020/file/dab10c50dc668cd8560df444ff3a4227-Paper.pdf)

