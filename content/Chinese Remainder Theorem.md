---
id: 1725891281-chinese-remainder-theorem
aliases:
  - Chinese Remainder Theorem
tags: []
---

# Chinese Remainder Theorem
## Modular Arithmetic Basics:
- When we say $a \equiv b \text{ (mod m)}$, we mean that $a$ and $b$ have the same remainder when divided by $m$.
- "$m$ divides $(a-b)$"

## The Problem CRT Solves:
- A system of simultaneous congruences.
- Given a set of congruences: $x \equiv a_1 \text{ (mod } m_1 \text{)} \quad x \equiv a_2 \text{ (mod } m_2 \text{)} \dots x \equiv a_n \text{ (mod } m_n \text{)}$
- Where the moduli ($m_1,m_2,\dots,m_n$) are pairwise [[coprime]], CRT states that there exists a unique solution modulo $M=m_1*m_2*\dots *m_n$

## Constructing the Solution:
- Calculate $M=m_1*m_2*\dots *m_n$
- For each $i$, calculate $M_i=M/m_i c$
- Find the modular multiplicative inverse of $M_i$ modulo $m_i$, call it $y_i$ ($M_i y_i \equiv 1 \text{ (mod } m_i \text{)}$)
- The solution is: $x \equiv (a_1M_1y_1+a_2M_2y_2+\dots+a_nM_ny_n)$ (mod $M$).

## Why It Works
- Key insight: $M_i$ is divisible by all moduli except $m_i$
- When we multiply it by $y_i$, we get a number that is congruent to $1$ modulo $m_i$ and congruent to $0$ modulo all other moduli
- This allows us to "select" the right $a_i$ for each modulus.
