---
id: 1725960857-lagrange-interpolation-formula
aliases:
  - Lagrange interpolation formula
tags: []
---

# Lagrange interpolation formula
A mathematical method used to reconstruct a polynomial given a set of points. In the context of Shamir's Secret Sharing, it's used to reconstruct the secret from the shared points.

The general form of the Lagrange interpolation formula is:

$$f(x) = Σ(i=1 to n) yi * Li(x)$$

Where:
- `f(x)` is the reconstructed polynomial
- `yi` are the y-coordinates of the known points
- `Li(x)` are the Lagrange basis polynomials

In the [[Shamir's Secret Sharing]] code, this formula is implemented as follows:

1. The outer loop `for i in 0..threshold` corresponds to the summation in the formula.

2. The `Li(x)` term is calculated in parts:
   ```rust
   let mut numerator = BigInt::one();
   let mut denominator = BigInt::one();

   for j in 0..threshold {
       if i != j {
           let (xj, _) = &shares[j];
           numerator *= xj.to_bigint().unwrap();
           numerator %= &prime_int;
           denominator *=
               (xj.to_bigint().unwrap() - xi.to_bigint().unwrap() + &prime_int) % &prime_int;
           denominator %= &prime_int;
       }
   }
   ```

   This calculates:
   $$Li(x) = Π(j≠i) (x - xj) / (xi - xj)$$

3. The `yi` term is multiplied in:
   ```rust
   let term = yi.to_bigint().unwrap() * numerator * mod_inverse(&denominator, &prime_int);
   ```

4. Each term is added to the secret:
   ```rust
   secret += term;
   secret %= &prime_int;
   ```

In [[Shamir's Secret Sharing]]:
- The secret is the constant term of a polynomial.
- Shares are points on this polynomial.
- Reconstruction uses Lagrange interpolation to find the polynomial and extract the constant term (the secret).

The use of modular arithmetic (with a prime modulus) ensures:
1. The calculations are performed in a [[finite field]].
2. Security properties of the scheme are maintained.
3. All operations "wrap around" the prime, keeping values manageable.

This implementation is secure because:
- It requires a threshold number of shares to reconstruct.
- Without enough shares, the polynomial (and thus the secret) remains undetermined.
