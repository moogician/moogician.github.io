---
layout: post
title: TCS review
date: 2022-06-22 20:00:00
description: a review before the TCS final
tags: tcs complexity circuit-complexity cryptography
categories: summary
---

# Results
## Chapter 2
- **Theorem 2.1** $$\P \subseteq \NP \subseteq \EXP$$
- **Theorem 2.2** $$\NP = \bigcup_{c\in \mathbb{N}}\NTIME{n^c}$$
- **Theorem 2.3**(*Cook-Levin Theorem*) $$\SAT\&3\SAT$$ are $$\NP$$-complete.  
*Hint: Snapshot expression*
- **Theorem 2.4** $$\P = \NP \Rightarrow \text{decision = search for }L \in \NP$$
- **Theorem 2.5** $$\P = \NP \Rightarrow \EXP = \NEXP$$  
*Hint: Padding*

## Chapter 3
- **Theorem 3.1**(*Time Hierarchy Theorem*) $$f(n)\log f(n) = o(g(n)) \Rightarrow \DTIME{f(n)} \nsubseteq \DTIME{g(n)}$$  
*Hint: Time Limit+Diagonalization*
- **Theorem 3.2**(*Nondeterministic Time Hierarchy Theorem*) $$f(n+1) = o(g(n)) \Rightarrow \NTIME{f(n)} \nsubseteq \NTIME{g(n)}$$  
*Hint: Lazy Diagonalization*
- **Theorem 3.3**\*(*Ladner's Theorem*) $$\P \neq \NP \Rightarrow \NP - \P \neq \NPC$$ *P64*
- Oracle Machines (tbd)

## Chapter 4
- **Theorem 4.1** $$\DTIME(s(n)) \subseteq \SPACE{s(n)} \subseteq \NSPACE(s(n)) \subseteq \DTIME (2^{O(s(n))})$$ *P72*  
*Hint: Configuration Graph*
- **Theorem 4.2**(*Space Hierarchy Theorem*) $$f(n) = o(g(n)) \Rightarrow \SPACE{f(n)} \nsubseteq \SPACE{g(n)}$$ *P75*
- **Theorem 4.3**(*Savitch's Theorem*) $$s(n) > \log n$$, $$\NSPACE{s(n)} \subseteq \SPACE{s^2(n)}$$ *P77*
*Hint: Configuration Graph+Recursion*

## Chapter 5
- **Theorem 5.1** $$\SIGMA{i}{p} = \PI{i}{p} \Rightarrow \PH = \SIGMA{i}{p}$$ *P87*
- **Theorem 5.2** $$\PH\text{-complete} \neq \emptyset \Rightarrow \exists i. \PH = \SIGMA{i}{p}$$
- **Theorem 5.3** $$\AP = \PSPACE$$
- Time Space Tradeoff (tbd)
- Oracle Definition (tbd)

## Chapter 6
- **Theorem 6.1** $$\P \subseteq \Ppoly$$
- **Theorem 6.2** $$L$$ is computable by a $$\P$$-uniform circuit family $$\Leftrightarrow L \in \P$$
- **Theorem 6.3** $$L$$ is computable by a $$\L$$-uniform circuit family with polynomial size $$\Leftrightarrow L \in \P$$
- **Theorem 6.4**(*TM with advice decides* $$\Ppoly$$) $$\Ppoly = \bigcup\limits_{c,d}\DTIME{n^c}/n^d$$  
*Hint: Consider Hardwiring the advice*
- **Theorem 6.5**(*Karp-Lipton Theorem*) $$\NP \subseteq \Ppoly \Rightarrow \PH = \SIGMA{2}{p}$$  
*Hint: Circuit that outputs certificates*
- **Theorem 6.6**(*Meyer's Theorem*) $$\EXP \subseteq \Ppoly \Rightarrow \EXP = \SIGMA{2}{p}$$  
- **Theorem 6.7**(*Hard Functions*) $$\exists f: \{0,1\}^{n} \rightarrow \{0,1\}$$ not computable by circuit of size $$\frac{2^n}{10n}$$  
*Hint: Probabilistic Method*
- **Theorem 6.8**(*Non-uniform Hierarchy Theorem*) $$2^n/n>T'(n)>10T(n)>0 \Rightarrow \SIZE{T(n)} \nsubseteq \SIZE{T'(n)}$$
- P completeness (tbd)
- Circuit of exponential size (tbd)

## Chapter 7
- **Theorem 7.1** $$\ZPP = \RP \cap \coRP$$
- **Claim 7.2** $$\RP, \coRP \subseteq \BPP$$
- **Theorem 7.3**(*Error Reduction*)
$$\exists M. \prob{}{M(x) = L(x)} \ge \frac12 + |x|^{-c} \Rightarrow \exists M'. \prob{}{M'(x) = L(x)} \ge 1 - 2^{-|x|^d}$$
- **Theorem 7.4** $$\BPP \subseteq \Ppoly$$
- **Theorem 7.5**(*Sipser-Gács Theorem*) $$\BPP \subseteq \SIGMA{2}{p}\cap \PI{2}{p}$$
*Hint: Translation of valid area*

## Chapter 8
- **Lemma 8.1** $$\dIP = \NP$$
- **Theorem 8.2** $$\AM[2] = \BPNP$$ *Exercise 8.3*
- **Theorem 8.3**\*(*Goldwasser-Sipser*) $$\IP[k] \subseteq \AM[k+2]$$ *P134*
- **Theorem 8.4** $$\IP = \PSPACE$$ *P139*
*Hint: Arithmetize TQBF*
- **Theorem 8.5** $$\PSPACE \subseteq \Ppoly \Rightarrow \PSPACE = \MA$$
- MIP (tbd)
- Checkers (tbd)
- IP for permant (tbd)

## Chapter 9
- Goldreich-Levin Theorem (tbd)

# Special Languages

# Techniques


<!----
PSPACE complete: TQBF 76
NL complete: PATH 80
coPATH in NL 82
CKT-SA  
---!>
