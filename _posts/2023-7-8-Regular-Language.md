---
layout: post
read_time: true
show_date: true
title: Regular Languages
date: 2023-7-8 1:21:20 -0600
description: Introduction for Regular Languages
img: posts/cover/COCO-Regular-Language.jpg
tags: [Computability and Complexity]
author: Thomas Men
toc: yes
mathjax: yes
---

## Finite Automata

Finite automata are good models for computers with an extremely limited amount of memory. The controller moves from state to state, depending on the input it receives. Finite automata and their probabilistic counterpart **Markov Chains** are useful tools when we are attempting to recognize patterns in data.

### Formal Definition of a finite automaton

​ A finite automaton is a list of those five objects: set of states, input alphabet, rules for moving, start state, and accept states.

`Definition` A _finite_ _automaton_ is a 5-tuple $(Q,\sum,\delta,q_0,F)$, where:

- $Q$ is a finite set called the state,
- $\sum$ is a finite set called the alphabet,
- $\delta: Q\times\sum\rightarrow Q$ is the transition function,
- $q_0\in Q$ is the start state, and
- $F\subseteq Q$ is the set of accept states.

`Definition` If $A$ is the set of all strings that machine $M$ accepts, we say that $A$ is the _language of machine_ $M$ and write $L(M)=A$. We say that $M$ _recognizes_ $A$ or that $M$ _accepts_ $A$. A machine may accepts several strings, but it always recognizes only one language. If the machine accepts no strings, it still recognizes only one language the empty language $\emptyset$.

### Formal Definition of Computation

​ Let $M=(Q,\sum,\delta,q_0,F)$ be a finite automaton and let $w=w_1w_2\cdots w_n$ be a string where each $w_i$ is a member of the alphabet $\sum$. Then $M$ _accepts_ $w$ if a sequence of states $r_0,r_1,\cdots, r_n$ in $Q$ exists with three conditions:

1. $r_0=q_0$
2. $\delta(r_i,w_{i+1})=r_{i+1}$, for $i=0,1,\cdots,n-1$, and
3. $r_n\in F$.

We say that $M$ _recognizes_ _language_ $A$, if $A=\\\{ w\mid M~ accepts~ w\\\}$.

`Definition` A language is called a _regular language_ if some finite automaton recognizes it.

Above definition equivalent to, $\forall a\in A$, $M$ accepts $a$.

### The Regular Operations

`Definition` Let $A$ and $B$ be languages. We define the regular operations _union_, _concatenation_, and _star_ as follows:

- _Union_: $A\cup B=\\{ x\mid x\in A\textit{ or }x\in B\\}$.
- _Concatenation_: $A\circ B=\\{ xy\mid x\in A~ and ~ y\in B\\}$
- _Star_: $A^{\*}=\\{ x_1x_2,\cdots,x_k\mid k\geq 0\textit{ and each }x_i\in A\\}$. The empty string $\epsilon$ is always a member of $A^*$, no matter what $A$ is. The set can be treated as all possible combination of the substring in $A$.

Generally speaking, a collection of objects is _closed_ under some operation if applying that operation to members of the collection returns an object still in the collection.

`Theorem` The class of regular language is closed under the union operation and the concatenation operation, star, and complementation.

`Lemma` Given a finite automaton $M(Q,\sum,\delta,q_0,F)$ that recognizes languages $L$, for any $\sum'\supseteq\sum$ there exists an automaton $M'=(Q',\sum',\delta',q_0',F')$ that also recognizes $L$.

If $L$ can be recognized by a finite automaton $M$ with alphabet $\sum$, then there it can also be recognized by another finite automaton $M'$ using a larger alphabet $\sum'$.

## Nondeterminism

When the machine is in a given state and reads the next input symbol, we know what the next state will be -- it is determined. We call this _deterministic_ computation. In a _nondeterministic_ machine, several choices may exist for the next state at any point.

Every state of deterministic finite automaton (**DFA**) always has exactly one exiting transition arrow for each symbol in alphabet. In an nondeterministic finite automaton (**NFA**), a state may have zero, one, or many exiting arrows for each alphabet symbol, including $\epsilon$.

`How NFA compute? `

​ After reading that symbol, the machine splits into multiple copies of itself and follows all the possibilities in parallel. Each copy of the machine takes one of the possible ways to proceed and continues as before. Finally, if _any one_ of these copies of the machine is in an accept state at the end of the input, the NFA accepts the input string.

### Formal Definition of a Nondeterministic finite automaton

In a DFA, the transition function takes a state and an input symbol and produces the next state. In an NFA, the transition function takes a state and an input symbol _or the empty string_ and produce the _set of possible next states_.

For any set $Q$ we write $P(Q)$ to be the collection of all subsets of $Q$. Here $P(Q)$ is called the power set of $Q$. For any alphabet $\sum$ we write $\sum_\epsilon$ to be $\sum\cup\\{ \epsilon\\}$.

`Definition` A _nondeterministic finite automaton_ is a 5-tuple $(Q,\sum,\delta,q_0,F)$ where:

- $Q$ is a finite set of states,
- $\sum$ is a finite set called the alphabet,
- $\delta: Q\times\sum_\epsilon\rightarrow P(Q)$ is the transition function,
- $q_0\in Q$ is the start state, and
- $F\subseteq Q$ is the set of accept states.

### Equivalence of NFAs and DFAs

DFA and NFA recognize the same class of languages. We say that two machines are _equivalent_ if they recognize the same language.

`Theorem` Every NFA has an equivalent deterministic finite automaton.

The idea is to use DFA to ssimulate every possible state transfer in NFA.

`Proof`

Let $N=(Q, \sum, \delta, q_0, F)$ be the NFA recognizing some language $A$. We construct a DFA $M=(Q',\sum,\delta',q_0',F')$ recognizing $A$. Let's first consider the easier case wherein $N$ has no $\epsilon$ arrows.

1. $Q'=P(Q)$.

2. For $R\in Q'$ and $a\in\sum$, let $\delta'(R,a)=\\{ q\in Q\mid q\in\delta(r,a) \textit{ for some   } r\in R\\}$, or simply,

   $$
   \delta'(R,a)=\bigcup_{r\in R}\delta(r,a)
   $$

   If $k$ is the # of states of the NFA, it has $2^k$ subset of states. Each subset corresponds to one of the possibilities that the DFA must remember, so the DFA simulating the NFA will have $2^k$ states.

3. $q_0'=\\{ q_0\\}$.

4. $F'=\\{ R\in Q'\mid R \textit{ contains an accept state of } N\\}$.

To consider the $\epsilon$ errors, we define $E(R)$ to be the collection of states that can be reached from members of $R$ by going only along $\epsilon$ arrows, including the members of $R$ themselves.

- The new transition function can be written as:

<center>

$$
\delta'(R,a)=\{ q\in Q\mid q\in E(\delta(r,a)) \textit{ for some }r\in R\}
$$

</center>

- Changing $q_0'$ to be $E(\\{ q_0\\})$.

After drawing the graph, we may simplify the machine by removing those states that does not contain the input arrow.

`Corollary` A language is regular if and only if some NFA recognizes it.

`Definition` The class of regular language is closed under the union, concatenation and star operation.

## Regular Expression

`Definition` Say that $R$ is a _regular expression_ if $R$ is:

- $a$ for some $a$ in the alphabet $\sum$,
- $\epsilon$,
- $\emptyset$, the empty language
- $(R_1\cup R_2)$, where $R_1$ and $R_2$ are regular expressions,
- $(R_1\circ R_2)$, where $R_1$ and $R_2$ are regular expressions, or
- $(R_1^*)$, where $R_1$ is a regular repression.

When we do calculation, the star operation ( \* ) has first priority, followed by concatenation $(\circ)$ and finally union $(\bigcup)$, unless parentheses change the usual order. For convenience, we let $R^{\+}=RR^{\*}$ and we write $L(R)$ to be the language of $R$.

Generally we have:

<center>
$$

\begin{align}
R\bigcup\emptyset&=R\\
R\circ\epsilon&=R
\end{align}

$$

</center>

However,

<center>
$$

\begin{align}
R\bigcup\epsilon&\neq R\\
R\circ\emptyset&\neq R
\end{align}

$$
</center>

Why? just take $R=\epsilon$ as an counter example. Pay attention on the difference between the regular language and regular expression.

`Theorem ` A language is regular (recognized by DFA/NFA) if and only if some regular expression describes it.

`Theorem 1.60` If a language is described by a regular expression, then it is regular; If a language is regular, then it is described by a regular expression.

`Definition 1.64` A _generalized nondeterministic finite automaton_ is a 5-tuple, $(Q,\sum,\delta,q_{start},q_{accept})$, where

1. $Q$ is the finite set of states,
2. $\sum$ is the input alphabet,
3. $\delta:(Q-\\{ q_{accept}\\})\times(Q-\\{ q_{start}\\})\longrightarrow \mathcal{R}$ is the transition function,
4. $q_{start}$ is the start state,
5. $q_{accept}$ is the accept state.

`Proof Idea for Theorem 1.60`

To convert DFAs into GNFA, and then GNFAs into regular expression.

1. DFA $\to$ GNFA.
   1. Add a new start state with an $\epsilon$ arrow to the old start state and a new accept state with $\epsilon$ arrows from the old accept states.
   2. If there are multiple arrows going between the same two states in the same direction, we replace each with a single arrow whose label is the union of the previous labels.
   3. Add arrows labelled $\emptyset$ between states that had no arrows.
2. GNFA $\to$ regular expression
   1. Assume the GNFA has $k$ states, if $k>2$, we construct an equivalent GNFA with $k-1$ states by removing the state $q_{rip}(q_2)$, which is the bridge state between $q_i$ and $q_j$, add an equivalent expression edge meanwhile.

      <img src="./assets/img/posts/typora-user-images/image-20210402174247244.png" alt="image-20210402174247244" style="zoom:50%;" />

   2. Repeat (i) until $k=2$, the GNFA has a single arrow that goes from the start state to the accept state. The label of this arrow is the equivalent regular expression.

## Nonregular Languages

`Theorem` _Pumping lemma_: If $A$ is a regular language, then there is a number $p$ (the pumping length) where if $s$ is any string in $A$ of length at least $p$, then $s$ may be divided into three pieces, $s=xyz$, satisfying the following conditions:

1. for each $i\geq 0, xy^iz\in A$,
2. $\mid y\mid >0$, and
3. $\mid xy\mid \leq p$.

This theorem states that all regular languages have a special property. If we can show that a language does not have this property, we are guaranteed that it is not regular. The property states that all strings in the language can be _pumped_ if they are at least as long as a certain special value, called the _pumping length_. That means every such string contains a section that can be repeated any number of times with the resulting string remaining in the language.

`Applying Pumping Lemma Contrapositively`

1. Assume $L$ is regular, thus $p$ exists. Use $p$ to define some $s\in L$ such that $\mid s\mid\geq p$.
2. Split $s$ in **all** possible ways into $xyz$ such that $\mid y\mid >0$ and $\mid xy\mid \leq p$.
   1. For every split, find some $i\neq 1$ (often $i=0$ or $i=2$ works) such that $xy^iz\notin L$.
3. Conclude: $\mid s\mid \geq p$, but there does not exist a split of $s$ into $xyz$ such that **all** 3 properties hold. (A language is regular if there exists a partition that satisfy $xy^iz\in L$).
   1. Contradiction! The assumption that $L$ is regular must be false.
   2. $L$ is not regular.


