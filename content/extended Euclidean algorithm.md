---
id: 1725556992-extended-euclidean-algorithm
aliases:
  - extended Euclidean algorithm
tags: []
---

# extended Euclidean algorithm
Extension of [[Euclidean algorithm|standard Euclidean algorithm]] for
finding the GCD of two numbers. Particularly useful in number theory and
cryptography.

1. Purpose:
   - Find the GCD of two numbers a and b
   - Express the GCD as a linear combination of a and b: gcd(a,b) = ax + by

2. Algorithm steps:
   1. Initialize variables:
      r0 = a, r1 = b
      s0 = 1, s1 = 0
      t0 = 0, t1 = 1
   2. While r1 ≠ 0:
      - Calculate quotient q = r0 / r1
      - Update remainders: r2 = r0 - q * r1
      - Update coefficients: s2 = s0 - q * s1, t2 = t0 - q * t1
      - Shift values: r0 = r1, r1 = r2, s0 = s1, s1 = s2, t0 = t1, t1 = t2
   3. When the algorithm terminates, r0 is the GCD, and s0 and t0 are the coefficients

## Rust Implementation
```rust
fn extended_gcd(a: i64, b: i64) -> (i64, i64, i64) { let mut r0 = a; let
mut r1 = b; let mut s0 = 1; let mut s1 = 0; let mut t0 = 0; let mut t1
= 1;

    while r1 != 0 {
        let q = r0 / r1;
        let r2 = r0 - q * r1;
        let s2 = s0 - q * s1;
        let t2 = t0 - q * t1;

        r0 = r1; r1 = r2;
        s0 = s1; s1 = s2;
        t0 = t1; t1 = t2;
    }

    // Return (gcd, x, y) such that ax + by = gcd
    (r0, s0, t0)
}

fn main() {
    let a = 48;
    let b = 18;
    let (gcd, x, y) = extended_gcd(a, b);
    
    println!("GCD of {} and {} is {}", a, b, gcd);
    println!("Coefficients: x = {}, y = {}", x, y);
    println!("Verification: {}*{} + {}*{} = {}", a, x, b, y, gcd);
}
```

This implementation:
1. Finds the GCD of two numbers
2. Calculates the coefficients x and y
3. Verifies that ax + by = gcd
