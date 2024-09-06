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
fn miller_rabin(n: u64) -> bool {
    let witnesses = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37];
    for &a in witnesses.iter() {
        if n == a {
            return true;
        }

        if !witness(a, n) {
            return false;
        }
    }
    true
}
```
- This function tests the number against a fixed set of "witnesses".
- If the number fails the test for any witness, it's composite.
- If it passes for all witnesses, it's probably prime.

```rust
fn witness(a: u64, n: u64) -> bool {
    let mut t = n - 1;
    let mut s = 0;

    while t % 2 == 0 {
        t /= 2;
        s += 1;
    }

    let mut x = mod_pow(a, t, n);
    if x == 1 || x == n - 1 {
        return true;
    }

    for _ in 0..s-1 {
        x = mod_mul(x, x, n);
        if x == n - 1 {
            return true;
        }
    }
    false
}
```
- This function performs the actual Miller-Rabin test for a single witness.
- It first decomposes n-1 into (2^s) * t.
- Then it computes a^t mod n and checks for the conditions that indicate primality.
