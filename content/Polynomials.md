---
id: 1725897202-polynomials
aliases:
  - Polynomials
tags: []
---

# Polynomials
An expression consisting of variables (usually denoted by letters) and coefficients, that involves only the operations of addition, subtraction, multiplication, and non-negative integer exponents.

General form: $P(x)=a_0+a_1x+a_2x^2+\dots+a_nx^n$ where $a_0,a_1,\dots,a_n$ are constants (coefficients) and $n$ is a non-negative integer.

## Key Terms
- *Degree* — the highest power of the variable in the polynomial
- *Leading coefficient* — the coefficient of the highest degree term
- *Constant term* — the term without a variable ($a_0$ in the general form)
- *Monomials* — polynomials with only one term

## [[Polynomial Arithmetic]]
### Addition and Subtraction
Add or subtract the coefficients of like terms.

Example: $(3x² + 2x + 1) + (x² - x + 4) = 4x² + x + 5$

### Multiplication
Use the distributive property and combine like terms.

Example: $(x + 2)(x - 3) = x² - 3x + 2x - 6 = x² - x - 6$

### Division
Long division or synthetic division for dividing by linear factors.

## Important Theorems:
### Fundamental Theorem of Algebra:
Every non-constant single-variable polynomial with complex coefficients has at least one complex root.

### Factor Theorem:
A polynomial $P(x)$ has a factor $(x - a)$ if and only if $P(a) = 0$.

### Remainder Theorem:
The remainder of $P(x)$ divided by $(x - a)$ is equal to $P(a)$.

## Roots and Factoring
Roots (or zeros) are values of $x$ that make $P(x) = 0$.
Factoring is expressing a polynomial as a product of simpler polynomials.

Methods include:
- Factoring out common factors
- Grouping
- Difference of squares: $a² - b² = (a+b)(a-b)$
- Sum/difference of cubes: $a³ ± b³ = (a ± b)(a² ∓ ab + b²)$

## Polynomial Functions
A polynomial function is a function defined by a polynomial.

### Properties
- Continuous everywhere
- Differentiable everywhere
- The degree determines the maximum number of turning points and x-intercepts

## Applications
- Curve fitting and data approximation
- Signal processing
- Control systems
- Cryptography (e.g., in error-correcting codes)

## Advanced Concepts
- [[Polynomial Rings]]
    - Algebraic structures consisting of all polynomials over a given field.
- [[Galois Theory]]
    - Uses properties of polynomials to study field extensions and solvability of equations.
- Orthogonal Polynomials
    - Special polynomial sequences used in mathematical analysis and physics.

## Computational Aspects
- Evaluating polynomials efficiently (e.g., Horner's method)
- Finding roots numerically (e.g., Newton's method)
- Interpolation: constructing a polynomial that passes through given points
