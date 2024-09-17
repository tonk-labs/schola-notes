---
id: 1726567251-elliptic-curves
aliases:
  - Elliptic Curves
tags: []
---

# Elliptic Curves
A set of points satisfying a specific type of cubic equation, along with a special point at infinity. Over a field $K$, an elliptic curve $E$ can be defined using the *Weierstrass equation*:
$$y^2=x^3+ax+b$$
where $a,b\in K$ and the curve is *non-singular*, meaning it has no cusps or self-intersections. The discriminant $\Delta=-16(4a^3+27b^2)$ must be non-zero to ensure non-singularity.

## Group Law on Elliptic Curves
Fascinatingly, elliptic curves form [[Abelian group|abelian groups]] under well-defined addition operations. This group law underscores such curves' cryptographic utility.

### Point Addition
Given two points $P$ and $Q$ on the curve, their sum $R=P+Q$ is also a point on the curve, defined geometrically:
1. **Case 1:** If $P\neq Q$, draw a line through $P$ and $Q$. This line will intersect the curve at a third point $-R$. Reflect $-R$ over the x-axis to get $R$.
2. **Case 2:** If $P=Q$, draw the tangent line at $P$. This line intersects the curve at $-R$. Reflect to get $R$.

### Point Doubling
When adding a point to itself $(P+P)$, we use the tangent at $P$.
$$P+P=2P$$
The slope $m$ of the tangent is:
$$m=\frac{3x^2_P+a}{2y_P}$$

### Identity Element
The *point at infinity*, often denoted $\mathcal{O}$, serves as the identity element of the group. For any point $P$:
$$P+\mathcal{O}=P$$

## Elliptic Curves over [[finite fields|Finite Fields]]
In cryptography, we often work with elliptic curves over finite fields $\Bbb{F}_p$ (where $p$ is a prime) or $\Bbb{F}_2^m$ (binary fields). Finite fields ensure a finite number of points on the curve, making computation practical.

### Discrete Logarithm Problem (DLP)
Given points $P$ and $Q=kP$, find $k$.

The discrete logarithm problem is believed to be very computationally difficult, especially over large finite fields. Thus, a solid foundation for cryptographic schemes.

## Elliptic Curve Cryptography (ECC)
Uses the properties of elliptic curves to create cryptographic algorithms that are both secure and efficient.

### Advantages of ECC
- *Stronger security per bit* — ECC offers equivalent security with smaller key sizes than RSA or DSA
- *Performance* — Smaller keys lead to faster computations and reduced storage requirements

### Common ECC Algorithms
- *Elliptic Curve Diffie-Hellman (ECDH)* — key agreement protocol allowing two parties to establish shared secret over an insecure channel
- *Elliptic Curve Digital Signature Algorithm (ECDSA)* — method for creating digital signatures, ensuring message integrity and authenticity
- *Elliptic Curve Integrated Encryption Scheme (ECIES)* — hybrid encryption scheme combining ECC with symmetric encryption for data confidentiality

## [[1726579238-bilinear-pairings|Pairings]] on Elliptic Curves
Bilinear maps that take two points on an elliptic curve and output an element in a finite field, enabling advanced cryptographic protocols.

### Definition
A pairing $e:E[r]\times E[r]\rightarrow\mu_r$ satisfies:
- *Bilinearity* — $e(aP,bQ)=e(P,Q)^{ab}$
- *Non-degeneracy* — $e(P,Q)\neq 1$ if $P$ and $Q$ are of order $r$
- *Computability* — there exists an efficient algorithm to compute $e(P,Q)$

### Types of Pairings
- *Weil Pairing*
- *Tate Pairing*
- *Optimal Ate Pairing*

Each have respective applications for which they have better computational advantages and suitability.

### Applications in Cryptography
- *Identity-Based Encryption (IBE)* — allows the use of arbitrary strings (e.g., email addresses) as public keys
- *Short Signatures* — schemes such as [[1726567320-bls-threshold|BLS]] (Boneh-Lynn-Shacham) enable very short signatures with security based on hardness of certain problems in pairing-friendly groups
- *Attribute-Based Encryption (ABE)* — enables fine-grained access control over encrypted data

## Common Curves
Standardisation bodies and cryptographic libraries often use specific curves optimised for security and performance.

### NIST Curves
- *P-256, P-384, P-521* — curves recommended by NIST with prime field sizes of 256, 384, and 521 bits

### SEC Curves
- *secp256k1* — used extensively in cryptocurrencies such as Bitcoin

### Montgomery and Edwards Curves
- *Curve25519* — a Montgomery curve known for its performance and security, used in protocols such as TLS and SSH
- *Edwards Curves (e.g., Ed25519)* — offer faster arithmetic and simpler, more secure implementations

### Pairing-Friendly Curves
- *Barreto-Naehrig (BN) Curves* — designed to optimise the efficiency of pairings, suitable for cryptographic protocols requiring bilinear maps
- *BLS12-381*: used in Ethereum 2.0 for its efficient pairings and high security level

## Implementation Considerations
### Field Arithmetic
Efficient implementation requires optimised algorithms for field operations:
- Modular addition/subtraction
- Modular multiplication
- Modular inversion

### Scalar Multiplication
Computing $kP$ efficiently is crucial:
- *Double-and-Add Algorithm*
- *Montgomery Ladder* — offers protection against certain side-channel attacks

### Security Considerations
- *Side-Channel Attacks* — implementations must protect against timing and power analysis attacks
- *Curve Selection* — choosing curves with well-understood security properties is essential

## References and Further Reading
- “An Introduction to Mathematical Cryptography” by Hoffstein, Pipher, and Silverman.
- “Guide to Elliptic Curve Cryptography” by Hankerson, Vanstone, and Menezes.
- Standards for Efficient Cryptography (SEC) documents for detailed specifications on various curves.
- Research papers on Pairing-Based Cryptography by Boneh, Lynn, and Shacham.

*Note: edited ChatGPT 01-preview output was used in the compilation of this document.*
