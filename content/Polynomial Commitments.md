---
id: 1726567296-polynomial-commitments
aliases:
  - Polynomial Commitments
tags: []
---

# Polynomial Commitments
Cryptographic primitives that enable a prover to commit to a polynomial $f(x)$ and later reveal evaluations $f(s)$ at specific points $s$, with assurance to the verifier that the evaluations are correct, without revealing the entire polynomial. Essential in [[zero-knowledge proofs]], [[verifiable computation]], and blockchain technologies.

## Overview
Polynomial commitment schemes allow a prover to commit to a polynomial in such a way that:
- The commitment is *binding*: once committed, the prover cannot change the polynomial without being detected
- The commitment is *hiding* (scheme-dependent): the committed polynomial remains secret until the prover decides to reveal it
- The prover can efficiently prove to a verifier that a given value is the correct evaluation of the polynomial at a specific point

They consist of the following algorithms:
- *Setup* — generates public parameters for the scheme
- *Commit* — commits to a polynomial $f(x)$
- *Open* — produces a rpoof that $f(s)=y$ without revealing $f(x)$
- *Verify* — checks the proof to confirm that $f(s)=y$ holds for the committed polynomial

## Applications
Essential for constructing efficient cryptographic protocols where the size of the proof and the verification time are independent of the degree of the polynomial or size of the data.
- *zk-SNARKs* — used in privacy-preserving cryptocurrencies like Zcash
- *Verifiable Computation* — allows clients to outsource computation to servers and verify results efficiently
- *Blockchain Scaling Solutions* — used in schemes like [[data availability sampling]] in Ethereum 2.0 to ensure data availability without downloading all the data (and increasingly in L2 rollups)

## Properties
- *Correctness* — if prover and verifier follow protocol honestly, verifier should accept valid proofs
- *Binding* — should be computationally infeasible for the prover to find two different polynomials $f(x)$ and $f'(x)$ that produce the same commitment
- *Hiding (sometimes)* — in some schemes, commitment hides the polynomial, providing privacy until the prover chooses to reveal it
- *Efficiency*
    - *Compactness* — size of commitment and proofs should be small, ideally constant-sized regardless of polynomial's degree
    - *Fast Verification* — verifier's work should be minimal, enabling practical development

## Types of Commitment Schemes
### Kate Commitments ([[1726567313-kzg|KZG]] Commitments)
- *Setup* — trusted setup generates public parameters, including sequence $\{\tau^i\}$ for $i=0$ to $d$, where $\tau$ is a secret
- *Commit* — given polynomial $f(x)=\sum^d_{i=0}{f_ix^i}$, the commitment is:
$$C=\sum_{i=0}^d{f_iG^{\tau^i}}$$
where $G$ is a generator of an elliptic curve group.
- *Open* — to prove that $f(s)=y$, prover computes a proof $\pi$ based on the polynomial's evaluation at $s$.
- *Verify* — verifier checks pairing equations to confirm proof's validity

#### Use of Pairings
KZG commitments rely on [[bilinear pairings]] on [[1726567251-elliptic-curves|elliptic curves]], which enable efficient verification through pairing-based equations.
- *Bilinear Pairing* — a map $e:G_1\times G_2\rightarrow G_T$ satisfying bilinearity, non-degeneracy, and computability
- *Verification Equation* — $e(C-y\cdot G, H)=e(\pi, G-s\cdot H)$, here, $H$ is another generator, and $s$ is the evaluation point

#### Advantages
- *Constant-size commitments and proofs* — independent of the polynomial's degree
- *Efficient Verification* — fast verification using pairings

#### Drawbacks
- *Trusted setup* — requires a secure generation of secret $\tau$
- *Security assumptions* — relies on hardness of [[1726567251-elliptic-curves#discrete-logarithm-problem-dlp|Discrete Logarithm Problem]] and *Computational Diffie-Hellman Problem* in pairing groups.

### Other Schemes
#### Pedersen Commitments
A hiding and binding commitment scheme based on discrete logarithms. Less efficient for polynomial commitments due to linear-sized proofs.

#### Bulletproofs
Non-interactive zk proofs that support logarithmic-sized range proofs that can be adapted for polynomial commitments.

Advantages
- Doesn't require trusted setup

Drawbacks
- Verification is more computationally intensive compared to KZG commitments

#### DARK (discrete-log-based Accumulators and RPCs)
Uses groups of unknown order (such as RSA groups) to achieve polynomial commitments without pairings.

Advantages
- No need for pairings

Drawbacks
- Less efficient
- Requires groups of unknown order

## Applications
### Zero-Knowledge Proofs
- Used in constructing zk-SNARKs and zk-STARKs, enabling efficient and succinct proofs that a computation was performed correctly without revealing the computation's inputs or polynomial itself.

### Verifiable Computation
- Clients can verify that a server correctly performed computations on polynomials (e.g., evaluations, integrations) without redoing the computations.

### Blockchain Technologies
- *Ethereum 2.0* uses KZG commitments in *danksharding* proposal for scalable data availability proofs
- *Plonk Protocol* is a universal SNARK protocol that uses polynomial commitments for efficient proof generation and verification

## Connection to [[1725960857-lagrange-interpolation-formula|Lagrange Interpolation]]
Any polynomial $f(x)$ of degree $d$ can be uniquely distributed by $d+1$ evaluations at distinct points, which is fundamental in constructing and verifying polynomial commitments.

## Implementation Considerations
### Trusted Setup
- *Multi-Party Computation (MPC)* — to mitigate trust issues, setups can be performed via MPC ceremonies where multiple parties contribute randomness, ensuring that as long as one party is honest, the setup remains secure

### Efficiency Optimisations
- *Batch verification* — verifying multiple proofs simultaneously to reduce computational overhead
- *Parallelisation* — leveraging parallel computing for operations like pairings and exponentiations

### Security Considerations
- *Security Assumptions* — based on well-studied problems such as discrete logarithm problem in elliptic curve groups
- *Side-Channel Attacks* — implementations must protect against timing attacks and other side-channel exploits

### Choice of Curves
- *Pairing-Friendly Curves* — curves such as BN256 or BLS12-381 are chosen for efficiency in pairing operations

## References and Further Reading
- “Polynomial Commitments” by Aniket Kate, Gregory M. Zaverucha, and Ian Goldberg.
- “A Practical Introduction to Pairings on Elliptic Curves” by Craig Costello.
- “Why and How zk-SNARK Works: Definitive Explanation” by Vitalik Buterin.
- Ethereum Research and EIPs related to polynomial commitments and KZG schemes.
- “A Decade of Lattice Cryptography” by Chris Peikert, for alternative approaches in cryptography.

*Note: edited ChatGPT 01-preview output was used in the compilation of this document.*
