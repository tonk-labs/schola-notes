---
id: 1725555801-modular-multiplicative-inverse
aliases:
  - modular multiplicative inverse
tags: []
---

# Modular multiplicative inverse
1. Definition:
   For two numbers 'a' and 'm', the modular multiplicative inverse of 'a' modulo 'm' is a number 'b' such that:
   
   (a * b) % m = 1

   In other words, 'b' is the number that when multiplied by 'a', gives a remainder of 1 when divided by 'm'.

2. Existence:
   The modular multiplicative inverse of 'a' modulo 'm' exists if and only if 'a' and 'm' are coprime (their greatest common divisor is 1).

3. Usage in [[RSA]]:
   In the context of RSA, we use the modular multiplicative inverse to calculate the private key 'd' given the public exponent 'e' and the totient φ(n). We need to find 'd' such that:
   
   (e * d) % φ(n) = 1

4. Calculation:
   The most common way to calculate the modular multiplicative inverse is using the [[extended Euclidean algorithm]].

## Rust Implementation
```rust
use num_bigint::BigUint;
use num_traits::{Zero, One};

fn mod_inverse(a: &BigUint, m: &BigUint) -> Option<BigUint> {
    let mut t = BigUint::zero();
    let mut newt = BigUint::one();
    let mut r = m.clone();
    let mut newr = a.clone();

    while !newr.is_zero() {
        let quotient = &r / &newr;
        t = std::mem::replace(&mut newt, t - &quotient * &newt);
        r = std::mem::replace(&mut newr, r - &quotient * &newr);
    }

    if r > BigUint::one() {
        None // Modular inverse doesn't exist
    } else if t < BigUint::zero() {
        Some(t + m)
    } else {
        Some(t)
    }
}

fn main() {
    let a = BigUint::from(3u32);
    let m = BigUint::from(11u32);
    
    if let Some(inverse) = mod_inverse(&a, &m) {
        println!("The modular multiplicative inverse of {} modulo {} is {}", a, m, inverse);
        // This should print: The modular multiplicative inverse of 3 modulo 11 is 4
        // Because (3 * 4) % 11 = 1
    } else {
        println!("Modular inverse doesn't exist");
    }
}
```

This implementation uses the [[extended Euclidean algorithm]] to find the modular multiplicative inverse. This works for all cases where the inverse exists.
