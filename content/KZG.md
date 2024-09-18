---
id: 1726567313-kzg
aliases:
  - KZG
tags: []
---

# KZG
A type of polynomial commitment scheme that enables a prover to commit to a polynomial and later prove the correctness of its evaluations at specific points efficiently. KZG commitments are ntoable for their constant-size commitments and proofs (agnostic of polynomial degree), making them highly efficient for cryptographic applications such as zk-SNARKs and blockchain technologies.

## Overview
KZG commitments allow a prover to:
- *Commit* to a polynomial $f(x)$ of degree $d$ with a single group element $C$
- *Open* the commitment at any point $s$ by providing a proof $\pi$ that the evaluation $y=f(s)$ is correct
- *Verify* the proof efficiently, with the verifier performing computations independent of $d$

## Mathematical Foundations
Builds on:
- [[Polynomial Arithmetic]]
- [[Elliptic Curves#elliptic-curve-cryptography-ecc|Elliptic Curve Cryptography]]
- [[Elliptic Curves#pairing-friendly-curves|Pairings]]

### Bilinear Pairings
A [[bilinear pairings|bilinear pairing]] is a map:
$$e:G_1\times G_2 \rightarrow G_T$$

where:
- $G_1,G_2,$ and $G_T$ are cyclic groups of prime order $r$
- The pairing $e$ has the following properties:
    - *Bilinearity* — $e(aP,bQ)=e(P,Q)^{ab}$ for all $P\in G_1$, $Q\in G_2$, $a,b \in \Bbb{Z}_r$
    - *Non-degeneracy* — $e(P,Q)\neq 1$ if $P$ and $Q$ are generators
    - *Computability* — there exists an efficient algorithm to compute $e(P,Q)$

## The KZG Commitment Scheme
Involves the following algorithms:
- *Setup*
- *Commit*
- *Open*
- *Verify*

### Setup
A trusted party generates public parameters using a secret $\tau\in\Bbb{F}_r$, where $\Bbb{F}_r$ is a finite field of prime order $r$.

Parameters:
- $G$ and $H$ are generators of $G_1$ and $G_2$ respectively
- $\{G^{\tau^i}\}_{i=0}^d$: elements in $G_1$, precomputed powers of $\tau$
- $H^\tau$ is an element in $G_2$

Outputs:
- Public parameters: $PP=(G,H,\{G^{\tau^i}\}_{i=0}^d,H^\tau)$
- Secret $\tau$ is discarded or securely erased

### Commit
Given a polynomial $f(x)=\sum_{i=0}^d{f_ix^i}$, the prover computes the commitment $C$ as:
$$C=\sum_{i=0}^d{f_iG^{\tau^i}}\in G_1$$

Alternatively, using polynomial notation:
- Represent $f(\tau)$ as an element in $G_1$:
    - $C=G^{f(\tau)}$

This uses the [[homomorphism]] between polynomials evaluated at $\tau$ and group elements.

### Open
To prove that $y=f(s)$ for some $s\in \Bbb{F}_\tau$, the prover computes a proof $\pi$ as follows:
1. Compute the *witness polynomial*:
$$w(x)=\frac{f(x)-y}{x-s}$$
2. Compute the proof $\pi$:
$$\pi=\text{Commit}(w)=G^{w(\tau)}\in G_1$$

This works because $f(x)-y=(x-s)w(x)$, so $w(\tau)=\frac{f(\tau)-y}{\tau-s}$.

### Verify
The verifier checks the proof using the pairing operation:
1. Compute $e(C-G^y,H)$
2. Compute $e(\pi,H^\tau-H^s)$
3. Check if $e(C-G^y,H)=e(\pi,H^{\tau-s})$

Since $H^{\tau-s}=H^\tau\cdot(H^s)^{-1}$, this can be computed efficiently.

Alternatively, the verifier checks:
$$e(C-G^y,H)=e(\pi,H^{\tau-s})$$

## Security Analysis
### Correctness
If both prover and verifier follow the protocol, the verification will succeed.

#### Proof Sketch
From the prover's side:
$$\pi=G^{w(\tau)}=G^{\frac{f(\tau)-y}{\tau-s}}$$

From the verifier's side:
$$e(C-G^y,H)=e(G^{f(\tau)-y},H)=e(G^{f(\tau)-y},H)$$
$$e(\pi, H^{\tau-s})=e(G^{\frac{f(\tau)-y}{\tau-s}},H^{\tau-s})=e(G^{f(\tau)-y},H)$$

Thus, both sides are equal.

### Binding
The binding property relies on the [[elliptic curves#discrete-logarithm-problem-dlp|discrete logarithm problem]] and the assumption that the prover cannot find two different polynomials $f(x)$ and $f'(x)$ such that $f(\tau)=f'(\tau)$, unless $f(x)=f'(x)$.

#### Security Assumption
- *Computational Diffie-Hellman (CDH) Problem* — hardness of computing $G^{ab}$ given $G^a$ and $G^b$
- *Knowledge of Coefficient (KoC)* — the commitment $C$ binds the prover to a specific polynomial $f(x)$

### Soundness
The prover cannot produce a vlaid proof $\pi$ for an incorrect evaluation $y$ without solving a hard problem in the underlying group.

## Trusted Setup and Its Mitigation
### Trusted Setup
- *Vulnerability* — if the secret $\tau$ is known to the prover, she can create fake proofs
- *Mitigation* — use a *Multi-Party Computation (MPC) ceremony* where multiple participants contribute to the generation of $\tau$ without any single party knowing it entirely

### MPC Ceremony
- Each participant $i$ picks a secret $\tau_i$
- The combined secret is $\tau=\Pi_i\tau_i$
- As long as one participant keeps their $\tau_i$ secret, the overall $\tau$ remains secure

## Efficiency and Practicality
### Constant-Size Commitments and Proofs
Both the commitment $C$ and the proof $\pi$ are single elements in $G_1$, regardless of the polynomial's degree.

### Verification Efficiency
- The verifier performs a small number of pairing operations, independent of the polynomial's degree
- Pairing operations are computationally intensive but practical with modern cryptographic libraries

### Prover Efficiency
- The prover's work involves computing $w(x)$ and exponentiations
- Efficient algorithms like [[fast fourier transform|Fast Fourier Transforms (FFT)]] can optimise polynomial operations when dealing with large degrees.

## Applications of KZG Commitments
### zk-SNARKs
- KZG commitments are used in constructing *Succinct Non-Interactive Arguments of Knowledge*
- Protocols like *Plonk* and *Sonic* leverage KZG commitments for efficient zero-knowledge proofs

### Blockchain Technologies
- *Ethereum 2.0* uses KZG commitments in *danksharding* proposal for scalable data availability proofs
- *Plonk Protocol* is a universal SNARK protocol that uses polynomial commitments for efficient proof generation and verification

### Verifiable Computation
- Clients can verify that a server correctly performed computations on polynomials (e.g., evaluations, integrations) without redoing the computations.

## Example Walkthrough
A simple example to illustrate how KZG commitments operate.

### Setup
- Let $r=23$, a small prime for simplicity
- The trusted setup selects $\tau=5$
- Compute $G^{\tau^i}$ for $i=0$ to $d$ (let's say $d=2$)

### Prover's Commitment
- Polynomial: $f(x)=3x^2+2x+1$
- Commitment: $C=f(\tau)\cdot G=f(5)\cdot G=(3\cdot5^2+2\cdot5+1)\cdot G=(75+10+1)\cdot G=86\cdot G$

### Prover's Proof of Evaluation at $s=2$
- Compute $y=f(2)=3\cdot4+2\cdot2+1=12+4+1=17$
- Compute witness polynomial:
$$w(x)=\frac{f(x)-y}{x-s}=\frac{f(x)-17}{x-2}$$
- Calculate $w(\tau)$
$$w(5)=\frac{f(5)-17}{5-2}=\frac{86-17}{3}=\frac{69}{3}=23$$
- Compute proof:
$$\pi=w(\tau)\cdot G=23\cdot G$$

### Verifier's Check
- Compute $C-y\cdot G=(86-17)\cdot G=69\cdot G$
- Compute $H^{\tau-s}=H^{5-2}=H^3$
- Verify:
$$e(69\cdot G,H)=e(23\cdot G,H^3)$$

Since $e(a\cdot G,H)=e(G,H)^a$:

$e(G,H)^69=e(G,H^3)^23=(e(G,H)^3)^23=e(G,H)^69$

Verification succeeds!

## Implementation Considerations
### Choice of Elliptic Curves
- *Pairing-Friendly Curves* — curves such as BLS12-381 or BN254 are chosen for their efficiency in pairing operations
- *Security Level* — select curves that provide at least 128-bit security

### Handling Large Polynomials
- *FTT Optimisation* — use FFTs over finite fields for efficient polynomial opertaions
- *Multi-Exponentiation* — optimise exponentiations involving multiple bases

### Side-Channel Protections
- Implement constant-time algorithms to prevent timing attacks
- Use blinding techniques to protect against side-channel leakage

## Advantages and Limitations
### Advantages
- *Constant-size proofs* — independent of the polynomial's degree
- *Efficiency* — fast verification for applications requiring scalability
- *Compatibility* — integrates well with existing cryptographic protocols

### Limitations
- *Trusted Setup* — requires a secure generation of the secret $\tau$
- *Pairing Operations* — computationally intensive, though practical with optimisation
- *Assumptions* — security depends on the hardness of specific problems in pairing-friendly groups

## Variants and Extensions
### Batched Proofs
Provers can compute a single proof for multiple evaluations to improve efficiency

### Aggregation
Aggregate multiple commitments and proofs to reduce communication overhead

### Transparent Setups
Research into schemas that avoid trusted setups, though they may sacrifice some efficiency.

## References and Further Reading
- “Constant-Size Commitments to Polynomials and Their Applications” by Aniket Kate, Gregory M. Zaverucha, and Ian Goldberg.
- “Pairing for Beginners” by Craig Costello.
- Ethereum Improvement Proposals (EIPs) related to KZG commitments and data availability.
- “Plonk: Permutations over Lagrange-bases for Oecumenical Noninteractive arguments of Knowledge” by Ariel Gabizon, Zachary J. Williamson, and Oana Ciobotaru.
- “Why and How zk-SNARK Works: Definitive Explanation” by Vitalik Buterin.

*Note: edited ChatGPT 01-preview output was used in the compilation of this document.*
