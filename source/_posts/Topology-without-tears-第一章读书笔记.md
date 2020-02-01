---
title: Topology without tears 第一章读书笔记
date: 2020-02-01 04:53:45
tags: [Topology, 读书笔记]
---

# Topological Spaces

<!-- more -->
Here is series of reading notes for I reading the book "Toplogy without tears".
And I'm so sorry about my poor English skill which caused u reading difficulties.

<!-- Before beginning I will tell u how I read this book, and what my notes will seem to be. -->


## 1.1 Topology

So my first question is  what is the concept 'Topology' be.

>Definitions. Let X be a non-empty set. A set τ of subsets of X is said to be a topology on X if
>
>(i) $X$ and the empty set, $\emptyset$, belong to $\tau$ ,
>
>(ii) the union of any (finite or infinite) number of sets in $\tau$ belongs to $\tau$, and
>
>(iii) the intersection of any two sets in $\tau$ belongs to $\tau$. The pair $(X,\tau)$ is called a **topological space**.

Here is a example

Let $X$ = {a, b, c, d, e, f} and
$\tau_1$ = ${X,Ø,{a},{c,d},{a,c,d},{b,c,d,e,f}}$.

Then $\tau_1$ is a topology on $X$ as it satisfies conditions (i), (ii) and (iii) of Definitions,and we can say that $(X,\tau_1)$ is a topological space.

Now it's time for some special definitions.

>Definitions. Let $X$ be any non-empty set and let $\tau$ be the collection of all subsets of $X$. Then $\tau$ is called the **discrete topology** on the set $X$. The topological space $(X,\tau)$ is called a **discrete space**.

<!-- How to prove that $\tau = P(X)$ is a topology on $X$ ?

Clearly we have $A,\emptyset \in \tau$

To Prove (ii)

Let $A, B \in X$, for all $A, B$,

we have $A,B \subseteq X$ and then $A \cup B \in P(X)$

Likely, it's eazy to prove $A \cap B \in \tau$ -->

 
>Definitions. Let $X$ be any non-empty set and $\tau$ = $\{X, \emptyset\}$. Then $\tau$ is called the **indiscrete topology** and $(X,\tau)$ is said to be an **indiscrete space**.

<!-- This is very eazy to prove. -->


>Proposition. If $(X,\tau)$ is a topological space such that, for every $x \in X$, the singleton set $\{x\}$ is in $\tau$ , then $\tau$ is the discrete topology.

Prove this proposition is not hard,so I last it for u.(ps: use mathematical induction)

>Definitions. 
>
>$\tau_1$ consists of $\mathbb{N}$,$\emptyset$, and every set $\{1,2,...,n\}$, for n any positive integer. $\tau_1$ called the **initial segment topology**.
>
>$\tau_2$ consists of $\mathbb{N}$,$\emptyset$, and every set $\{n,n+1,...\}$, for n any positive integer. $\tau_2$ is called the **final segment topology**.
