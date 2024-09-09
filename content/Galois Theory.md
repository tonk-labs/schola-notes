---
id: 1725899101-galois-theory
aliases:
  - Galois Theory
tags: []
---

# Galois Theory
Provides a deep connection between field theory and group theory.

## Core Idea
- Establishes correspondence between subfields of a [[1725898288-extension-fields|field extension]] and the subgroups of a certain group of automorphisms of the extension field
- This correspondence allows us to study the structure of field extensions using group theory

## Galois Group
- For a field extension $E/F$, the Galois group $\text{Gal}(E/F)$ is the group of all [[1725899418-automorphism|automorphisms]] of $E$ that fix every element of $F$
- These automorphisms permute the roots of polynomials over $F$ in $E$

## Galois Extensions
- An algebraic extension $E/F$ is called a Galois extension if it is both normal and separable
- For such extensions, the Galois theory correspondence is most complete

## Fundamental Theorem of Galois Theory
This is the cornerstone of Galois theory. For a finite Galois extension $E/F$:
- There is a one-to-one correspondence between the intermediate fields $K$ $(F ⊆ K ⊆ E)$ and the subgroups $H$ of $\text{Gal}(E/F)$.
- If $K$ corresponds to $H$, then $[E:K] = |H|$ and $[K:F] = [\text{Gal}(E/F):H]$.
- $K$ is the fixed field of $H$ (elements of $E$ fixed by all automorphisms in $H$).
- $\text{Gal}(E/K) = H$.
- $K$ is a normal extension of $F$ if and only if $H$ is a normal subgroup of $\text{Gal}(E/F)$.

## Applications
- Solvability of Polynomial Equations:
    - Galois theory provides a criterion for determining whether a polynomial equation is solvable by radicals. An equation is solvable by radicals if and only if its Galois group is a solvable group.
- Impossibility Proofs:
    - It proves the impossibility of certain geometric constructions (like trisecting an angle or squaring a circle) and the non-existence of formulas for roots of general polynomials of degree 5 or higher.
- Field Automorphisms:
    - Helps in understanding the structure of field automorphisms, crucial in algebraic number theory and algebraic geometry.

## Steps in Galois Theory Analysis
1. Identify the splitting field of a polynomial.
2. Determine the Galois group of this splitting field over the base field.
3. Analyse the subgroups of the Galois group.
4. Use the Galois correspondence to understand the structure of subfields.

## Advanced Concepts
- Inverse Galois Problem:
    - Asks which finite groups can be realized as Galois groups over a given field. Still open for the rational numbers.
- Infinite Galois Theory:
    - Extends the ideas to infinite extensions, using topological groups.
- Differential Galois Theory:
    - Applies Galois-theoretic ideas to differential equations.

## Historical Significance
- Galois theory revolutionized algebra by shifting focus from finding explicit solutions to understanding the structure of the solutions.
- It laid the groundwork for much of modern abstract algebra.

## Connection to Other Areas
- Galois theory has profound connections to number theory, algebraic geometry, and even areas of physics like quantum mechanics.
