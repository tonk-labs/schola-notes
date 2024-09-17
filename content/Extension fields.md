---
id: 1725898288-extension-fields
aliases:
  - Extension fields
tags: []
---

# Extension fields
Fundamental concept in abstract algebra, esp field theory. Crucial role in understanding structure of fields and have important applications in areas like algebraic number theory and [[Galois Theory|Galois theory]].

## Definition
An extension field is a larger field that contains a given base field as a subfield. If $F$ is the base field and $E$ is the extension field, we denote this as $E/F$ ("E over F").

## Degree of Extension
Degree of an extension $E/F$, denoted $[E:F]$, is the dimension of $E$ as a vector space over $F$. It can be finite or infinite.

## Types of Extensions
- Simple Extension
    - An extension $E/F$ is simple if $E=F(\alpha)$ for some $\alpha \in E$
    - $\alpha$ is called a primitive element of the extension
- Algebraic Extension
    - An extension $E/F$ is algebraic if every element of $E$ is algebraic over $F$ (i.e., is the root of some polynomial with coefficients in $F$)
- Transcendental Extension
    - An extension that is not algebraic
    - Contains at least one element that is not the root of any polynomial over $F$
- Finite Expression
    - An extension with finite degree
    - All finite extensions are algebraic
- Normal Extension
    - An algebraic extension where every irreducible polynomial over $F$ that has one root in $E$ has all its roots in $E$
- Separable Extension
    - An algebraic extension where the minimal polynomial of every element is separable (has no repeated roots in its splitting field)
- Galois Extension
    - An extension that is both normal and separable

## Constructing Extension Fields
- Adjoining Roots
    - Given a polynomial $f(x)$ over $F$, we can construct an extension field by adjoining a root of $f(x)$ to $F$
- Splitting Fields
    - The smallest extension field that contains all roots of a given polynomial

## Algebraic Closure
- An algebraically closed field is one in which every non-constant polynomial has a root
- Every field has an algebraic closure, which is an algebraic extension that is algebraically closed

## Tower Law
- For extensions $E/K$ and $K/F$, we have $[E:F]=[E:K][K:F]$.
- This is crucial in analysing the structure of field extensions.

## Applications
- Solving Polynomial Equations
    - Extension fields allow us to find roots of polynomials that don't have roots in the base field
- Constructibility Problems
    - In geometry, extension fields help determine which lengths are constructible with compass and straight-edge
- [[Galois Theory]]
    - Uses field extensions to study the solvability of polynomial equations and the relationship between roots of polynomials
- Algebraic Number Theory
    - Studies extensions of the rational numbers
    - Crucial in understanding properties of algebraic numbers

## Primitive Element Theorem
- Any finite separable extension has a primitive element, meaning it can be expressed as a simple extension

## Fundamental Theorem of [[1725899101-galois-theory|Galois Theory]]
- Establishes a correspondence between intermediate fields of a Galois extension and subgroups of its Galois group
- Provides a powerful tool for analysing field extensions
