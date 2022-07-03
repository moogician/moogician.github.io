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
**Lemma 1**(Lem. 5 in [*Agarwal et al.*][1]) Given $$f:\mathcal{M} \rightarrow \mathbb{R}$$ twice continuously differentiable, and $$(x,s)$$ in the domain of a retraction $$R$$, the gradient and Hessian of the pullback $$\hat{f_x}$$ at $$s \in T_x\mathcal{M}$$ are given by

\begin{equation}
    \nabla\hat{f_x}(s) = T_s^*\text{grad}f(R_x(s)) 
\end{equation}

and

\begin{equation}
    \nabla^2\hat{f_x}(s) = T_s^* \circ \text{Hess} f(R_x(s)) \circ T_s + W_s,
\end{equation}

in which $$T_s$$ is the differential of $$R_x$$ at $$s$$, and $$W_s$$ is defined through polarization by

$$
    \langle W_s[\dot{s}], \dot{s}\rangle = \langle \text{grad}f(R_x(s)), c^{\prime\prime}(0)\rangle_{R_x(s)},
$$

in which $$c(t) = R_x(s+t\dot{s})$$.

*Sketch of Proof*: Consider calculating the first and second order derivative of $$g(t) = f\circ R_x(s + t\dot{s}) = \hat{f_x}(s + t\dot{s})$$ in two differnt ways.

### Transfer of Lipschitz Continuity

**Theorem 1**(Thm. 2.7 in [*Criscitiello et al.*][2]) Given a Riemannian manifold $$\mathcal{M}$$, suppose that the sectional curvature of $$\mathcal{M}$$ is bounded by $$K$$ and $$\nabla R$$($$R$$ is the Riemannian curvature endomorphism) is bounded by $$F$$ in operator norm. 
Suppose $$f: \mathcal{M} \rightarrow \mathbb{R}$$ is twice continuously differentiable and $$b$$ is selected such that $$0 < b \le \min\left(\frac1{4\sqrt{K}}, \frac{K}{4F}\right)$$. Also, suppose we pick $$x$$ such that $$Exp_x$$ is defined on the closed ball $$B_x(b) \subseteq T_x\mathcal{M}$$. We would have

- If $$f$$ has $$L$$-Lipschitz continuous gradient and $$\lVert \text{grad}f(x)\rVert \le Lb$$, the pullback would have $$2L$$-Lipschitz continuous gradient in $$B_x(b)$$.
- If $$f$$ has $$\rho$$-Lipschitz continuous Hessian, the Hessian of the pullback would be $$\rho+L\sqrt{K}$$-Lipschitz continuous.
- The singular values of $$T_s = DExp_x$$ in $$B_x(b)$$ is bounded in $$[\frac23, \frac43]$$


[1]: <https://arxiv.org/pdf/1806.00065.pdf> "Adaptive regularization with cubics on manifolds"
[2]: <https://arxiv.org/pdf/2008.02252.pdf> "An accelerated first-order method for non-convex optimization on manifolds"

