---
id: Euclidean algorithm
aliases:
  - standard Euclidean algorithm
tags: []
---

see also: [[extended Euclidean algorithm]]

# Euclidean algorithm
A simple and efficient method for computing the greatest common divisor (GCD) of two numbers.

1. Start with two positive integers a and b.
2. Divide a by b and get the remainder r.
3. If r is 0, b is the GCD.
4. If not, replace a with b and b with r.
5. Repeat from step 2 until r becomes 0.

## Rust Implementation
```rust
fn gcd(mut a: u64, mut b: u64) -> u64 {
    while b != 0 {
        let temp = b;
        b = a % b;
        a = temp;
    }
    a
}

fn main() {
    let a = 48;
    let b = 18;
    let result = gcd(a, b);
    println!("The GCD of {} and {} is {}", a, b, result);

    let c = 17;
    let d = 23;
    let result2 = gcd(c, d);
    println!("The GCD of {} and {} is {}", c, d, result2);
}
```

This implementation:
1. Takes two unsigned 64-bit integers as input.
2. Uses a while loop to repeatedly apply the division step.
3. Swaps the values of a and b in each iteration, with b becoming the remainder.
4. Continues until b becomes 0, at which point a is the GCD.

The algorithm works because of the following property:
If a = bq + r, then GCD(a,b) = GCD(b,r)

This property allows us to continually reduce the problem to smaller numbers until we reach the GCD.

Key points about the Euclidean algorithm:

1. It's efficient: the number of steps is at most 5 times the number of digits in the smaller number.
2. It works with any two positive integers, regardless of which is larger.
3. It can be extended to find the GCD of more than two numbers by repeatedly applying it.

The main difference between the standard and [[extended Euclidean algorithm|extended Euclidean algorithms]] is that the extended version also finds the coefficients of [[Bézout's identity]], which expresses the GCD as a linear combination of the two original numbers.
