---
id: 1725615525-modular-exponentiation
aliases:
  - Modular exponentiation
tags: []
---

# Modular exponentiation
Efficient algorithm to calculate (base^exp) % modulus.

The key idea is to break down the exponent into its binary representation and use the property that:

(a * b) mod m = ((a mod m) * (b mod m)) mod m

This allows the algorithm to perform the modulus operation at each step, keeping the intermediate results small and manageable.

## Rust Implementation
```rust
fn mod_pow(base: &BigUint, exp: &BigUint, modulus: &BigUint) -> BigUint {
    let mut result = BigUint::one();
    let mut base = base.clone();
    let mut exp = exp.clone();
    while exp > BigUint::zero() {
        if exp.bit(0) {
            result = (result * &base) % modulus;
        }
        base = (&base * &base) % modulus;
        exp >>= 1;
    }
    result
}
```
1. The function takes three parameters:
   - `base`: The base number
   - `exp`: The exponent
   - `modulus`: The modulus for the operation
   All parameters are references to `BigUint` (big unsigned integer) values.

2. Initialize `result` with 1 (BigUint::one()).

3. Clone `base` and `exp` to create mutable copies.

4. Enter a loop that continues while `exp` is greater than zero:

   a. Check if the least significant bit of `exp` is 1 using `exp.bit(0)`.
      If true, multiply `result` by `base` and take the modulus.

   b. Square `base` and take the modulus.

   c. Right-shift `exp` by 1 bit (equivalent to dividing by 2).

5. Return the final `result`.

This algorithm is efficient because it reduces the number of multiplications needed from O(exp) to O(log(exp)).
