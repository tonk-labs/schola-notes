---
id: 1725615525-modular-exponentiation
aliases:
  - Modular exponentiation
tags: []
---

# Modular exponentiation
Efficient algorithm to calculate (base^exp) % modulus.

## Rust Implementation
```rust
fn mod_pow(mut base: u64, mut exp: u64, modulus: u64) -> u64 {
    if modulus == 1 {
        return 0;
    }

    let mut result = 1;
    base %= modulus;
    while exp > 0 {
        if exp % 2 == 1 {
            result = mod_mul(result, base, modulus);
        }

        exp >>= 1;
        base = mod_mul(base, base, modulus);
    }
    result
}

fn mod_mul(a: u64, b: u64, m: u64) -> u64 {
    ((a as u128 * b as u128) % m as u128) as u64
}
```
1. Function signature:
   - Takes three `u64` parameters: `base`, `exp` (exponent), and `modulus`
   - Returns a `u64` result

2. Edge case handling:
   - If `modulus` is 1, it returns 0 (as any number mod 1 is 0)

3. Initialization:
   - `result` is set to 1 (neutral element for multiplication)
   - `base` is reduced modulo `modulus` to ensure it's smaller than `modulus`

4. Main loop:
   - Continues while `exp` is greater than 0
   - Uses the binary exponentiation algorithm:
     - If the least significant bit of `exp` is 1, multiply `result` by `base`
     - Divide `exp` by 2 (right shift by 1 bit)
     - Square `base`
   - All multiplications are done using modular arithmetic (via `mod_mul` function)

5. Return the final `result`

This algorithm is efficient because it reduces the number of multiplications needed from O(exp) to O(log(exp)).
