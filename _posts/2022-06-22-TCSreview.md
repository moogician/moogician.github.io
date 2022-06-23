---
layout: post
title: TCS review
date: 2022-06-22 20:00:00
description: a review before the TCS final
tags: tcs complexity circuit-complexity cryptography
categories: summary
---

# Chapter 2
- **Theorem 2.1** $$\P \subseteq \NP \subseteq \EXP$$
- **Theorem 2.2** $$\NP = \bigcup_{c\in \mathbb{N}}\NTIME{n^c}$$
- **Theorem 2.3**(*Cook-Levin Theorem*) $$\SAT\&3\SAT$$ are $$\NP$$-complete.  
*Hint: Snapshot expression*
- **Theorem 2.4** $$\P = \NP \Rightarrow \text{decision = search for }L \in \NP$$
- **Theorem 2.5** $$\P = \NP \Rightarrow \EXP = \NEXP$$  
*Hint: Padding*

# Chapter 3
- **Theorem 3.1**(*Time Hierarchy Theorem*) $$f(n)\log f(n) = o(g(n)) \Rightarrow \DTIME{f(n)} \nsubseteq \DTIME{g(n)}$$  
*Hint: Time Limit+Diagonalization*
- **Theorem 3.2**(*Nondeterministic Time Hierarchy Theorem*) $$f(n+1) = o(g(n)) \Rightarrow \NTIME{f(n)} \nsubseteq \NTIME{g(n)}$$  
*Hint: Lazy Diagonalization*
- **Theorem 3.3**\*(*Ladner's Theorem*) $$\P \neq \NP \Rightarrow \NP - \P \neq \NPC$$ *P64*

