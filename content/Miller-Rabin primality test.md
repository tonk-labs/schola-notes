---
id: 1725614892-miller-rabin-primality-test
aliases:
  - Miller-Rabin primality test
tags: []
---

# Miller-Rabin primality test
- Probabilistic algorithm used to determine wheter a given number is prime.
- Particularly efficient for large numbers.

1. Concept:
   - For a prime number p, the equation a^(p-1) ≡ 1 (mod p) holds for all a coprime to p ([[Fermat's Little Theorem]]).
   - Miller-Rabin extends this idea by considering how a number reaches 1 when repeatedly squared.

2. Algorithm steps:
   a. Express n-1 as (2^s) * d, where d is odd.
   b. Choose a random base a (2 < a < n-2).
   c. Compute x = a^d mod n.
   d. If x = 1 or x = n-1, the number might be prime.
   e. Square x up to s-1 times. If we ever get n-1, the number might be prime.
   f. If we never get n-1 in step e, the number is definitely composite.
   g. Repeat with different bases for higher confidence.

## Rust Implementation
```rust
fn miller_rabin_test(n: &BigUint, a: &BigUint) -> bool {
    if *n == BigUint::from(2u32) {
        return true;
    }
    if n.is_even() {
        return false;
    }

    let n_minus_one = n - 1u32;
    let s = n_minus_one.trailing_zeros().unwrap();
    let d = &n_minus_one >> s;

    let mut x = a.modpow(&d, n);
    if x == BigUint::one() || x == n_minus_one {
        return true;
    }

    for _ in 0..s - 1 {
        x = (&x * &x) % n;
        if x == n_minus_one {
            return true;
        }
    }

    false
}
```
### Notes
Here's a step-by-step explanation of the function:

1. The function takes two `BigUint` parameters: `n` (the number to test for primality) and `a` (a random base used in the test).
2. First, it checks if `n` is 2, which is prime, so it returns `true`.
3. If `n` is even (and not 2), it's not prime, so it returns `false`.
4. It calculates `n_minus_one` as `n - 1`.
5. It finds `s` and `d` such that `n - 1 = 2^s * d`, where `d` is odd:
   - `s` is the number of trailing zeros in the binary representation of `n_minus_one`.
   - `d` is `n_minus_one` right-shifted by `s` bits.
6. It computes `x = a^d mod n` using the `modpow` function.
7. If `x` is 1 or `n - 1`, it returns `true` (probably prime).
8. It then enters a loop that runs `s - 1` times:
   - In each iteration, it squares `x` modulo `n`.
   - If `x` becomes `n - 1`, it returns `true` (probably prime).
9. If the loop completes without returning, it returns `false` (composite).

This test is not definitive but provides a strong probabilistic indication of primality. To increase certainty, the test is typically run multiple times with different random bases `a`.
