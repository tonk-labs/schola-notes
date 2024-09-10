---
id: 1725904360-shamirs-secret-sharing
aliases:
  - Shamir's secret sharing
tags: []
---

# Shamir's secret sharing
## Implementation Steps
1. Implement polynomial evaluation
2. Create a function to generate random coefficients
3. Implement the share generation function
4. Implement the secret reconstruction function

## Generate Shares
```rust
/// Generates Shamir's Secret Sharing scheme shares for a given secret.
///
/// # Arguments
/// * `secret` - The secret to be shared
/// * `num_shares` - The total number of shares to generate
/// * `threshold` - The minimum number of shares required to reconstruct the secret
///
/// # Returns
/// A vector of tuples, where each tuple contains a share ID and its corresponding value
fn generate_shares(
    secret: &BigUint,
    num_shares: usize,
    threshold: usize,
) -> Vec<(BigUint, BigUint)> {
    let prime = BigUint::parse_bytes(PRIME.as_bytes(), 10).unwrap();
    let mut rng = thread_rng();
    let mut coefficients = vec![secret.clone()];
    for _ in 1..threshold {
        coefficients.push(rng.gen_biguint_below(&prime));
    }

    (1..=num_shares)
        .map(|x| {
            let x_biguint = BigUint::from(x);
            let mut y = BigUint::zero();
            for (i, coeff) in coefficients.iter().enumerate() {
                y += coeff * mod_pow(&x_biguint, &BigUint::from(i), &prime);
                y %= &prime;
            }
            (x_biguint, y)
        })
        .collect()
}
```

### Notes
1. Function signature:
   ```rust
   fn generate_shares(
       secret: &BigUint,
       num_shares: usize,
       threshold: usize,
   ) -> Vec<(BigUint, BigUint)>
   ```
   This function takes a secret (as a `BigUint` reference), the number of shares to generate, and the threshold (minimum number of shares needed to reconstruct the secret). It returns a vector of tuples, where each tuple represents a share (x, y).

2. Prime number initialization:
   ```rust
   let prime = BigUint::parse_bytes(PRIME.as_bytes(), 10).unwrap();
   ```
   This line parses a predefined prime number (PRIME) from a string to a `BigUint`.

3. Random number generator initialization:
   ```rust
   let mut rng = thread_rng();
   ```
   This creates a thread-local random number generator.

4. Coefficient generation:
   ```rust
   let mut coefficients = vec![secret.clone()];
   for _ in 1..threshold {
       coefficients.push(rng.gen_biguint_below(&prime));
   }
   ```
   This creates a vector of coefficients for a polynomial. The first coefficient is the secret itself, and the rest are randomly generated numbers below the prime.

5. Share generation:
   ```rust
   (1..=num_shares)
       .map(|x| {
           // ... (explained below)
       })
       .collect()
   ```
   This generates `num_shares` shares using a range from 1 to `num_shares` (inclusive) and mapping each value to a share.

6. Share calculation:
   ```rust
   let x_biguint = BigUint::from(x);
   let mut y = BigUint::zero();
   for (i, coeff) in coefficients.iter().enumerate() {
       y += coeff * mod_pow(&x_biguint, &BigUint::from(i), &prime);
       y %= &prime;
   }
   (x_biguint, y)
   ```
   For each share:
   - Convert the x value to a `BigUint`.
   - Calculate the y value using the polynomial with the generated coefficients.
   - Use modular exponentiation (`mod_pow`) for each term.
   - Sum up the terms and take the modulus with the prime number.
   - Return the (x, y) pair as a share.

The secret can be reconstructed only when a minimum number of shares (threshold) are combined.

## Reconstruct Secret
```rust
/// Reconstructs the secret from a set of Shamir's Secret Sharing scheme shares.
///
/// # Arguments
/// * `shares` - A slice of tuples, where each tuple contains a share ID and its corresponding value
/// * `threshold` - The number of shares used for reconstruction (must match the threshold used in generation)
///
/// # Returns
/// The reconstructed secret as a BigUint
fn reconstruct_secret(shares: &[(BigUint, BigUint)], threshold: usize) -> BigUint {
    let prime = BigUint::parse_bytes(PRIME.as_bytes(), 10).unwrap();
    let prime_int = prime.to_bigint().unwrap();
    let mut secret = BigInt::zero();

    for i in 0..threshold {
        let (xi, yi) = &shares[i];
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

        let term = yi.to_bigint().unwrap() * numerator * mod_inverse(&denominator, &prime_int);
        secret += term;
        secret %= &prime_int;
    }

    if secret < BigInt::zero() {
        secret += prime_int;
    }
    secret.to_biguint().unwrap()
}
```

### Notes
1. Function signature:
   ```rust
   fn reconstruct_secret(shares: &[(BigUint, BigUint)], threshold: usize) -> BigUint
   ```
   The function takes a slice of tuples (shares) and a threshold, and returns a BigUint.

2. Initialize variables:
   - `prime`: A large prime number (constant) parsed from a string.
   - `prime_int`: The prime converted to BigInt.
   - `secret`: Initialized to zero, will hold the reconstructed secret.

3. Main loop:
   Iterates through the shares up to the threshold:
   ```rust
   for i in 0..threshold {
   ```

4. For each share:
   - Extract `xi` and `yi` from the current share.
   - Initialize `numerator` and `denominator` to 1.

5. Inner loop:
   For each share (except the current one):
   ```rust
   for j in 0..threshold {
       if i != j {
   ```
   - Update `numerator` and `denominator` using [[Lagrange interpolation formula]].
   - Use modular arithmetic to keep values within the prime field.

6. Calculate term:
   ```rust
   let term = yi.to_bigint().unwrap() * numerator * mod_inverse(&denominator, &prime_int);
   ```
   - Multiply `yi` by the calculated numerator and the modular inverse of the denominator.

7. Update secret:
   ```rust
   secret += term;
   secret %= &prime_int;
   ```
   - Add the term to the secret and apply modulo operation.

8. Final adjustment:
   ```rust
   if secret < BigInt::zero() {
       secret += prime_int;
   }
   ```
   - Ensure the secret is non-negative.

9. Return:
   ```rust
   secret.to_biguint().unwrap()
   ```
   - Convert the secret back to BigUint and return it.

This implementation uses the [[Lagrange interpolation formula]] to reconstruct the secret from the given shares. It works in a [[finite field]] defined by the prime number to ensure security and proper reconstruction.

## Main Function
```rust
/// The main function that demonstrates the usage of Shamir's Secret Sharing scheme.
///
/// This function:
/// 1. Creates a secret
/// 2. Generates shares for the secret
/// 3. Reconstructs the secret from a subset of shares
/// 4. Compares the reconstructed secret with the original
fn main() {
    let secret = BigUint::parse_bytes(b"123456789012345678901234567890", 10).unwrap();
    let num_shares = 5;
    let threshold = 3;

    let shares = generate_shares(&secret, num_shares, threshold);
    println!("Shares:");
    for (i, share) in shares.iter().enumerate() {
        println!("Share {}: ({}, {})", i + 1, share.0, share.1);
    }

    let reconstructed_secret = reconstruct_secret(&shares[0..threshold], threshold);
    println!("Reconstructed secret:      {}", reconstructed_secret);
    println!("Original secret:           {}", secret);
    println!(
        "Reconstruction successful: {}",
        reconstructed_secret == secret
    );
}
```

### Notes
In the above code, `num_shares` and `threshold` are chosen as fixed values:

```rust
let num_shares = 5;
let threshold = 3;
```

These values are hard-coded in the `main` function. What they represent and how they are typically chosen in Shamir's Secret Sharing scheme:

1. `num_shares` (5 in this case):
   - Total number of shares that will be generated from the secret.
   - It determines how many participants can hold a part of the secret.

2. `threshold` (3 in this case):
   - Minimum number of shares required to reconstruct the secret.
   - It must be less than or equal to `num_shares`.

The choice of these values depends on the specific security requirements and use case. Some considerations for choosing these values:

1. Security: A higher threshold increases security, as more shares are needed to reconstruct the secret.
2. Availability: A lower threshold makes it easier to reconstruct the secret if some shares are lost.
3. Number of participants: `num_shares` should be at least equal to the number of participants who need to hold a share.
