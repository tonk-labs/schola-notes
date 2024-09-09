---
id: 1725899418-automorphism
aliases:
  - automorphism
tags: []
---

# automorphism
Fundamental concept in algebra, crucial in [[1725899101-galois-theory|Galois Theory]].

## Definition
- An automorphism of a mathematical object is an [[isomorphism]] from the object to itself.

## Properties
- Bijective (one-to-one and onto)
- Structure-preserving (respects the operations defined on the object)
- Invertible (the inverse if also an automorphism)

## Automorphism Group
- The set of all automorphisms of an object forms a group under composition
- Group called the automorphism of the object

## Types of Automorphisms
- Preserve the group operation
    - $\varphi(ab)=\varphi(a)\varphi(b)$ for all $a,b$ in the group
    - Ex: in $(Z,+)$, the map $x \mapsto -x$ is an automorphism
- Ring Automorphisms
    - Preserve both addition and multiplication
    - Ex: complex conjugation is an automorphism of $C$
- Field Automorphisms
    - Preserve addition, multiplication, and multiplicative inverses
    - **Crucial in Galois theory**
- Vector Space Automorphisms
    - Linear transformations that are bijective
    - For finite-dimensional spaces, these are precisely the invertible linear transformations

## Inner Automorphisms
- In group theory, an inner automorphism is one that can be achieved by conjugation by an element of the group.
- The inner automorphisms form a normal subgroup of the full automorphism group.

## Fixed Points
- Elements that are left unchanged by an automorphism
- The study of fixed points is often crucial in understanding the structure of the automorphism

## Automorphisms in Galois Theory
- The Galois group of a field extension $E/F$ is the group of the automorphisms of $E$ that fix every element of $F$
- These automorphisms permute the roots of polynomials over $F$ in $E$
- The fixed field of a subgroup of automorphisms corresponds to an intermediate field in the extension

## Applications
- Cryptography
    - Automorphisms are used in various cryptographic protocols, especially those based on group theory
- Coding Theory:
    - Automorphisms of codes help in understanding code structure and in constructing new codes
- Algebraic Geometry:
    - Automorphisms of algebraic varieties provide insights into their structure
- Number Theory:
    - Automorphisms of number fields are crucial in studying their arithmetic properties

## Continuous Automorphisms
- In topological or differential structures, we often consider automorphisms that are also continuous or differentiable.

## Computability Aspects
- For finite structures, finding all automorphisms is computable but can be computationally expensive. For infinite structures, the situation can be much more complex.
