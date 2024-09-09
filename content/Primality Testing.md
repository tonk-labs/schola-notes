---
id: 1725895944-primality-testing
aliases:
  - Primality Testing
tags: []
---

# Primality Testing
The process of determining whether a given number is prime or composite.

## Trivial Division
- Principle: if a number $n$ is not prime, it must have a factor less than or equal to $\sqrt{n}$
- Method: check if $n$ is divisible by any integer from $2$ to $\sqrt{n}$
- Time complexity: $O(\sqrt{n})$
- Pros: simple to implement and understand
- Cons: very slow for large numbers

## Fermat Primality Test
Probabilistic test based on [[Fermat's Little Theorem]].
- Principle: if $n$ is prime, then for any integer $a$ [[1725892132-coprime|coprime]] to $n$, $a^{(n-1)} \equiv 1$ (mod $n$).
- Method:
    - Choose a random base $a$ ($1<a<n-1$)
    - Compute $a^{(n-1)}$ mod $n$
    - If the result is not $1$, $n$ is definitely composite
    - If the result is $1$, $n$ is probably prime
- Time complexity: $O(\log n)$ using fast exponentiation
- Pros: much faster than trial division for large numbers
- Cons: can be fooled by Carmichael numbers (composite numbers that satisfy the condition for all $a$ coprime to $n$)

## [[1725614892-miller-rabin-primality-test|Miller-Rabin Primality Test]]
An extension of the Fermat test that overcomes the problem of Carmichael numbers.
- Principle: if $n$ is an odd prime, then $n-1=2^s \cdot d$ where $d$ is odd, and either: $a^d \equiv 1$ (mod $n$), or $a^{((2^r) \cdot d)} \equiv -1$ (mod $n$) for some $r$ where $0 \leq r \leq s$
- Method:
    - Write $n-1$ as $2^s \cdot d$
    - Choose a random base $a$ ($1 \lt a \lt n-1$)
    - Compute $a^d$ mod $n$
    - If it's $1$ or $-1$, probably prime
    - Otherwise, repeatedly square the result, checking for $-1$ each time
    - If you never get $-1$, definitely composite
- Time complexity: $O(k \log^3 n)$ where $k$ is the number of rounds
- Pros: very fast and can be made arbitrarily accurate by increasing rounds
- Cons: still probabilistic (but with controllable error probability)

## AKS Primality Test
Deterministic polynomial-time algorithm, proving that primality testing is in $P$.
- Principle: based on generalization of [[Fermat's Little Theorem]] to polynomial rings
- Method: check if $(X+a)^n \equiv X^n + a$ (mod $X^r-1,n$) for suitable $r$ and $a$
- Time complexity: $O(\log^6 n)$ in the basic version, improved to $O(\log^4 n)$ in optimized versions
- Pros: deterministic and polynomial-time
- Cons: still slower than probabilistic methods for practical use

## Specialized tests for certain forms of numbers
There are efficient primality tests for numbers of special forms, like Mersenne numbers ($2^p-1$) or Fermat numbers ($2^{(2^n)}+1$).

In practice, a combination of these methods is often used:
1. First, try trial division by small primes (e.g., up to 100) to quickly eliminate many composites.
2. Then use a probabilistic test like [[1725614892-miller-rabin-primality-test|Miller-Rabin]] for several rounds.
3. If a number passes these tests and absolute certainty is required, use a deterministic test.

For cryptographic uses, probabilistic tests are usually sufficient, as the probability of a composite number passing multiple rounds of testing can be made astronomically small.
