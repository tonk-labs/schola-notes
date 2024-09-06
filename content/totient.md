---
id: 1725554049-totient
aliases:
  - totient
tags: []
---

# Euler's totient function

1. Definition:
   For a positive integer n, the totient function φ(n) counts the number of integers between 1 and n that are coprime to n (i.e., their greatest common divisor with n is 1).

2. Properties:
   - For a prime number p, φ(p) = p - 1
   - The function is multiplicative: if a and b are coprime, then φ(ab) = φ(a) * φ(b)

3. In RSA:
   In the context of RSA, we use φ(n) where n = p * q (p and q are prime numbers)
   φ(n) = φ(p) * φ(q) = (p - 1) * (q - 1)

4. Importance:
   The totient is crucial for generating the private key in RSA. It's used to find the [[modular multiplicative inverse]] of the public exponent.
