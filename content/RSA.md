---
id: RSA
aliases: []
tags: []
---

[Implementation Overview](https://link.springer.com/chapter/10.1007/978-3-319-21936-3_15)

# Basic Implementation Steps
1. Generate two prime numbers ($p$ and $q$)
2. Calculate n = p * q
3. Calculate the [[totient]] φ(n) = (p-1) * (q-1)
4. Choose a public exponent $e$ (usually 65537)
5. Calculate the private exponent $d$ using the [[modular multiplicative inverse]]
6. Implement encryption: $c = m^e mod n$
7. Implement decryption: $m = c^d mod n$

## Optimised Rust Implementation
```rust
use num_bigint::{BigInt, BigUint, RandBigInt, ToBigInt};
use num_integer::Integer;
use num_traits::{One, Zero};
use rand::prelude::*;
use rayon::prelude::*;
use std::sync::Arc;

const SMALL_PRIMES: [u32; 12] = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37];

fn generate_prime(bits: u64) -> BigUint {
    let mut rng = thread_rng();
    loop {
        let n: BigUint = rng.gen_biguint(bits);
        if n.is_odd() && is_prime(&n) {
            return n;
        }
    }
}

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

fn is_prime(n: &BigUint) -> bool {
    for &p in &SMALL_PRIMES {
        if *n == p.into() {
            return true;
        }
        if n % p == BigUint::zero() {
            return false;
        }
    }

    if n < &BigUint::from(2047u32) {
        return miller_rabin_test(n, &BigUint::from(2u32));
    }

    let bases: Arc<Vec<BigUint>> = Arc::new(
        if n.bits() < 64 {
            vec![2u32, 3, 5, 7, 11, 13, 17]
        } else if n.bits() < 128 {
            vec![2u32, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]
        } else {
            vec![2u32, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41]
        }
        .into_iter()
        .map(BigUint::from)
        .collect::<Vec<BigUint>>(),
    );

    bases.par_iter().all(|b| miller_rabin_test(n, b))
}

fn mod_inverse(a: &BigUint, m: &BigUint) -> Option<BigUint> {
    let a = a.to_bigint().unwrap();
    let m = m.to_bigint().unwrap();
    let (mut t, mut newt) = (BigInt::zero(), BigInt::one());
    let (mut r, mut newr) = (m.clone(), a);

    while !newr.is_zero() {
        let quotient = &r / &newr;
        (t, newt) = (newt.clone(), t - &quotient * &newt);
        (r, newr) = (newr.clone(), r - &quotient * &newr);
    }

    if r > BigInt::one() {
        None
    } else {
        while t < BigInt::zero() {
            t += &m;
        }
        Some((t % &m).to_biguint().unwrap())
    }
}

fn generate_keypair() -> (BigUint, BigUint, BigUint) {
    let (p, q) = rayon::join(|| generate_prime(1024), || generate_prime(1024));
    let n = &p * &q;
    let phi = (&p - 1u32) * (&q - 1u32);
    let e = BigUint::from(65537u32);
    let d = mod_inverse(&e, &phi).unwrap();
    (n, e, d)
}

fn encrypt(m: &BigUint, e: &BigUint, n: &BigUint) -> BigUint {
    m.modpow(e, n)
}

fn decrypt(c: &BigUint, d: &BigUint, n: &BigUint) -> BigUint {
    c.modpow(d, n)
}

fn main() {
    let (n, e, d) = generate_keypair();
    println!("Public key (n, e): ({}, {})", n, e);
    println!("Private key (d): {}", d);

    let message = BigUint::from(758562222222222343u64);
    println!("Original message: {}", message);

    let encrypted = encrypt(&message, &e, &n);
    println!("Encrypted message: {}", encrypted);

    let decrypted = decrypt(&encrypted, &d, &n);
    println!("Decrypted message: {}", decrypted);

    assert_eq!(message, decrypted);
}
```

### Notes
This code implements the RSA (Rivest-Shamir-Adleman) cryptosystem, a widely used public-key cryptography algorithm. Here's a general outline of what the code does and how it works:

1. Import necessary libraries for big integer operations, random number generation, and parallel processing.
2. Define constants and helper functions:
   - `SMALL_PRIMES`: A list of small prime numbers for initial primality checks.
   - `generate_prime`: Generates a prime number of a specified bit length.
   - `miller_rabin_test`: Implements the [[Miller-Rabin primality test]].
   - `is_prime`: Checks if a number is prime using small prime divisions and Miller-Rabin tests.
   - `mod_inverse`: Calculates the [[modular multiplicative inverse]].
3. Implement core RSA functions:
   - `generate_keypair`: Generates RSA public and private keys.
   - `encrypt`: Encrypts a message using the public key.
   - `decrypt`: Decrypts a message using the private key.
4. In the `main` function:
   - Generate a keypair (public and private keys).
   - Create a sample message.
   - Encrypt the message using the public key.
   - Decrypt the encrypted message using the private key.
   - Verify that the decrypted message matches the original.

The code uses parallel processing (via the `rayon` library) and optimized primality testing to improve performance. It also employs big integer arithmetic to handle the large numbers involved in RSA cryptography.

- [[Miller-Rabin primality test]] for numbers larger than 2^32
- [[Modular exponentiation]] to calculate (base^exp) % modulus
