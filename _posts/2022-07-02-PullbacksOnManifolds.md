---
layout: post
title: Pullbacks on Manifolds
date: 2022-07-02 20:00:00
description: properties regarding pullbacks
tags: optimization smooth-manifold
categories: recent-results
---

# Pullbacks 

## Definition
For an arbitrary function $$f:\mathcal{M} \rightarrow \mathbb{R}$$, its *pullback* at $$x\in\mathcal{M}$$ is defined as
$$
\hat{f_x} = f \circ Exp_{x}: T_x\mathcal{M} \rightarrow \mathbb{R}
$$

## Pullbacks in Optimization
In an optimization setting, there is a cost function $$f$$. To design&prove an algorithm, we need to evaluate two consecutive states: before&after a step of the algorithm. 

This is relatively easier in an Euclidean space. However, when the problem is on a (riemannian) manifold, we only have norms and metrics in the tangent space of a point $$T_x\mathcal{M}$$. We could analyze the manifold's structure directly. Or, we could use the pullback of the cost function to prove some properties of the algorithm.

## Results
### Formulae
**Lemma 1**([*Agarwal et al.*][1]) Given $$f:\mathcal{M} \rightarrow \mathbb{R}$$ twice continuously differentiable, and $$(x,s)$$ in the domain of a retraction $$R$$, the gradient and Hessian of the pullback $$\hat{f_x}$$ at $$s \in T_x\mathcal{M}$$ are given by
\begin{equation}
    \nabla\hat{f_x}(s) = T_s^*\text{grad}f(R_x(s))\;\text{and}\; \nabla^2\hat{f_x}(s) = T_s^* \circ \text{Hess} f(R_x(s)) \circ T_s + W_s
\end{equation}
in which $$T_s$$ is the differential of $$R_x$$ at $$s$$, and $$W_s$$ is defined through polarization by
$$
    \langle W_s[\dot{s}], \dot{s}\rangle = \langle \text{grad}f(R_x(s)), c^{\prime\prime}(0)\rangle
$$
in which $$c(t) = R_x(s+t\dot{s})$$.

[1]: <https://arxiv.org/pdf/1806.00065.pdf> "Agarwal et al., Adaptive regularization with cubics on manifolds"

