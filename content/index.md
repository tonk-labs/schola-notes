---
id: index
aliases: []
tags: []
title: Schola Notes
---

Schola Notes seeks to compile our learnings at Tonk as we progress through our in-house engineering curriculum. Each week, we'll post our notes on increasingly complex mathematical and cryptographic topics, accompanied by implementations and examples in Rust.

## Getting Started
If you'd like to follow along, you may navigate to [this Notion page](https://www.notion.so/tonk/Foundation-for-Applied-Cryptography-0a33951054b84cd68c3e030bed945003)—which contains the topics of interest for each week as well as useful learning resources—and [this repository](https://github.com/tonk-gg/schola)—which includes all our solutions.

### Prerequisites
- A strong understanding of maths up to the undergraduate level is useful
    - Familiarity with discrete algebra is particularly pertinent
- A familiarity with the Rust programming language
- A burning desire to learn!

## Weeks
### [[Week 1]]
- [[Groups]], [[subgroup|subgroups]], [[rings]], [[fields]]
- [[Finite Fields]]
- [[Euler's Theorem]], [[Fermat's Little Theorem]]
- [[Chinese Remainder Theorem]]
- [[Primality Testing]]
- [[RSA]]

#### EXERCISES
- [[RSA|Implement naive RSA]]

#### REFERENCES
- [Lambda Week 1 Recommended Material](https://github.com/lambdaclass/sparkling_water_bootcamp)
- https://github.com/pluto/ronkathon/tree/main/src/algebra
- https://crypto.stanford.edu/~dabo/courses/OnlineCrypto/slides/10-numth-annotated.pdf
- https://cims.nyu.edu/~regev/teaching/discrete_math_fall_2005/dmbook.pdf (CH. 8)
- Elementary Number Theory
- Comp Intro to AA and NT

### [[Week 2]]
- [[Polynomials]]
    - [[Polynomial arithmetic]]
    - [[Extension fields]] and [[error-correcting codes]]
    - [[Reed-Solomon Codes]]
- [[Roots of unity]]
- [[Fast Fourier Transform]]

#### EXERCISES
- Implement basic [[Shamir's secret sharing]]

#### REFERENCES
- https://berthub.eu/articles/posts/reed-solomon-for-programmers/
- Lambda Week 3 Recommended Material (Polynomials)
- https://github.com/pluto/ronkathon/tree/main/src/codes
- https://github.com/pluto/ronkathon/blob/main/math/polynomial.sage
- Moonmath Manual Polynomial Arithmetic
