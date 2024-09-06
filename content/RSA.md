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

## Rust Implementation
```rust
use rand::Rng;

fn is_prime(n: u64) -> bool {
    if n <= 1 {
        return false;
    }

    if n <= 3 {
        return true;
    }

    if n % 2 == 0 || n % 3 == 0 {
        return false;
    }

    let limit = (n as f64).sqrt() as u64;
    let mut i = 5;
    while i <= limit {
        if n % i == 0 || n % (i + 2) == 0 {
            return false;
        }
        i += 6;
    }

    if n < 2_u64.pow(32) {
        return true;
    }

    miller_rabin(n)
}

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

    for _ in 0..s - 1 {
        x = mod_mul(x, x, n);
        if x == n - 1 {
            return true;
        }
    }
    false
}

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

fn generate_prime(min: u64, max: u64) -> u64 {
    let mut rng = rand::thread_rng();
    loop {
        let n = rng.gen_range(min..=max);
        if is_prime(n) {
            return n;
        }
    }
}

fn gcd(a: u64, b: u64) -> u64 {
    if b == 0 {
        a
    } else {
        gcd(b, a % b)
    }
}

fn mod_inverse(a: u64, m: u64) -> Option<u64> {
    for i in 1..m {
        if (a * i) % m == 1 {
            return Some(i);
        }
    }
    Nne
}

fn generate_keys() -> ((u64, u64), (u64, u64)) {
    let p = generate_prime(100, 1000);
    let q = generate_prime(100, 1000);
    let n = p * q;
    let phi = (p - 1) * (q - 1);

    let mut e = 65537; // Common choice for e
    while gcd(e, phi) != 1 {
        e += 2;
    }

    let d = mod_inverse(e, phi).unwrap();

    ((e, n), (d, n)) // ((public_key), (private_key))
}

fn encrypt(message: u64, public_key: (u64, u64)) -> u64 {
    let (e, n) = public_key;
    mod_pow(message, e, n)
}

fn decrypt(ciphertext: u64, private_key: (u64, u64)) -> u64 {
    let (d, n) = private_key;
    mod_pow(ciphertext, d, n)
}

fn main() {
    let (public_key, private_key) = generate_keys();
    println!("Public Key (e, n): {:?}", public_key);
    println!("Private Key (d, n): {:?}", private_key);

    let message = 42;
    println!("Original Message: {}", message);

    let encrypted = encrypt(message, public_key);
    println!("Encrypted: {}", encrypted);

    let decrypted = decrypt(encrypted, private_key);
    println!("Decrypted: {}", decrypted);
}
```

### Notes
- [[Miller-Rabin primality test]] for numbers larger than 2^32
- [[Modular exponentiation]] to calculate (base^exp) % modulus
