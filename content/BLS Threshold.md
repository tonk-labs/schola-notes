---
id: 1726567320-bls-threshold
aliases:
  - BLS Threshold
tags: []
---

# BLS Threshold
Threshold cryptography allows a group of parties to jointly perform cryptographic operations such that only a subset (threshold) of the parties is required to collaborate, enhancing both security and availability. The Boneh-Lynn-Shacham (BLS) signature scheme is particularly well-suited for threshold implementations due to its simplicity and the properties of pairing-based cryptography.

## Overview
A cryptographic algorithm that enables short signatures and efficient aggregation of signatures. Base on [[bilinear pairings]] over [[elliptic curves]].

Key Features:
- *Short Signatures* — signatures are elements of an elliptic curve group, resulting in compact representations
- *Signature Aggregation* — multiple signatures can be combined into a single signature, reducing verification overhead
- *Deterministic Signing* — signing process doesn't require randomness, simplifying implementation

### Mathematical Foundation
Rely on properties of [[bilinear pairings]].

### Basic BLS Signature Scheme
- Setup
    - Choose a bilinear pairing: $e:G_1\times G_2\rightarrow G_T$
    - Select generators $g_1\in G_1,g_2\in G_2$
- Key Generation
    - Private Key: $sk=x\in\Bbb{Z}_r$
    - Public Key: $pk=g_2^x\in G_2$
- Signing
    - Hash the message $m$ to a point $H(m)\in G_1$
    - Compute the signature $\sigma=H(m)^x\in G_1$
- Verification
    - Accept if $e(\sigma,g_2)=e(H(m),pk)$

## Threshold Cryptography
### What is it?
A technique where a cryptographic operation is distributed among multiple parties, and a minimum number of parties (threshold $t$) must collaborate to perform the operation.
- *Enhances Security* — no single party holds the complete secret key, reducing risk of key compromise
- *Improves Availability* — system can tolerate up to $n-t$ faulty or compromised parties

### Threshold Signature Schemes
Allows any subset of at least $t$ out of $n$ participants to produce a valid signature, while subsets smaller than $t$ cannot.
- *Distributed Key Generation* — secret key is never reconstructed; parties hold shares of the key
- *Partial Signatures* — each participant produces a partial signature
- *Signature Reconstruction* — partial signatures are combined to form a complete signatue

## Threshold BLS Signatures
Combining BLS with threshold cryptography results in *threshold BLS signatures*, which enable a group of parties to collaboratively sign messages using BLS signatures.

### Core Components
- *Threshold $t$* — minimum number of parties required to produce signature
- *Participants $n$* — total number of parties in system
- *Secret Sharing* — private key is shared among parties using a secret sharing scheme

### Secret Sharing Schemes
See [[Shamir's Secret Sharing]].

### Threshold BLS Signature Protocol
- Setup
    - *Distributed Key Generation (DKG)* — parties collaboratively generate shares of the private key without revealing it
    - *Public Key* — $pk=g_2^x$ is publicly known
- Signing
    1. Partial Signing
        - Each party $i$ computes their partial signature:
        $$\sigma_i=H(m)^{x_i}$$
    2. Broadcast Partial Signatures
        - Parties share their $\sigma_i$ with the combiner
    3. Signature Reconstruction
        - Using [[Lagrange interpolation formula|Lagrange interpolation]], combine $t$ partial signatures to form the full signature:
        $$\sigma=\Pi_{i\in S,j\neq1}\sigma_i^{\lambda_i}$$
        where $S$ is the set of participating parties and $\lambda_i$ are Lagrange coefficients:
        $$\lambda_i=\Pi_{j\in S,j\neq i}\frac{j}{j-i}$$
- Verificaiton
    - Same as in basic BLS scheme:
    $$e(\sigma,g_2)=e(H(m),pk)$$

## Advantages of Threshold BLS Signatures
### Efficiency
- *Compact Signatures* — the final signature is the same size as a regular BLS signature
- *Aggregation* — multiple threshold signatures can be aggregated

### Security
- *Robustness* — the system remains secure even if up to $n-t$ parties are compromised
- *Non-Interactive Signing* — parties can produce partial signatures independently

### Practicality
- *Simplified Key Management* — no need to reconstruct the secret key at any point
- *Scalability* — suitable for large networks where coordination among all parties is impractical

## Applications in Cryptography and Computer Science
### Distributed Systems
- *Blockchain Consensus* — used in protocols like *proof of stake* to sign blocks collectively
- *Distributed Key Management* — secure storage and use of cryptographic keys across multiple servers

### Secure Multi-Party Computation (MPC)
- *Collaborative Signing* — parties compute signatures without revealing their private keys
- *Threshold Encryption* — extending the concept to encryption schemes for secure message decryption by a group

### Internet Infrastructure
- *Domain Name System Security Extensions (DNSSEC)* — threshold signatures enhance security by distributing trust
- *Certificate Authorities* — prevent single points of failure in issuing digital certificates

## Implementation Considerations
### Dealing with Malicious Parties
- *Verification of Partial Signatures* — ensure partial signatures are valid before combining
- *Zero-Knowledge Proofs* — parties provide proofs that their shares are correctly computed

### Communication Overhead
- *Broadcast Channels* — partial signatures need to be shared, requiring efficient communication protocols
- *Threshold Adjustments* — systems should allow for dynamic thresholds to adapt to changing network conditions

### Key Generation
- *Distributed Key Generation (DKG)* — avoids the need for a trusted dealer; parties jointly generate key shares

### Liveness and Availability
- *Resilience to Dropouts* — protocol should handle parties becoming unavailable
- *Timeouts and Retries* — implement mechanisms to proceed in the presence of non-responsive parties

## Security Analysis
### Threshold Unforgeability
- An adversary cannot forge a signature without at least $t$ valid shares
- *Collusion Resistance* — up to $t-1$ colluding parties cannot reconstruct the secret key

### Robustness Against Attacks
- *Replay Attacks* — each signature is unique to the message, preventing reuse
- *Adaptive Chosen-Message Attacks* — the security of BLS signatures holds under such attacks

### Assumptions
- *Random Oracle Model* — security proofs often assume the hash function behaves like a random oracle
- *Bilinear Pairing Security* — relies on the hardness of pairing-based cryptographic assumptions

## Example Walkthrough
### Setup
- Threshold $t=3$, Participants $n=5$
- Secret Key $x\in\Bbb Z$
- Sharing Polynomial $f(z)=x+a_1z+a_2z^2$

### Key Sharing
Shares:
- Party 1: $x_1=f(1)$
- Party 2: $x_2=f(2)$
- Party 3: $x_3=f(3)$
- Party 4: $x_4=f(4)$
- Party 5: $x_5=f(5)$

### Partial Sharing
Each party computes $\sigma_i=H(m)^{x_i}$

### Signature Reconstruction
- Suppose parties 1, 2, and 3 collaborate
- Compute Lagrange coefficients:
$$\lambda_1 = \frac{(0 - 2)(0 - 3)}{(1 - 2)(1 - 3)} = \frac{(-2)(-3)}{(-1)(-2)} = \frac{6}{2} = 3$$
$$\lambda_2 = \frac{(0 - 1)(0 - 3)}{(2 - 1)(2 - 3)} = \frac{(-1)(-3)}{(1)(-1)} = \frac{3}{-1} = -3$$
$$\lambda_3 = \frac{(0 - 1)(0 - 2)}{(3 - 1)(3 - 2)} = \frac{(-1)(-2)}{(2)(1)} = \frac{2}{2} = 1$$
- Combine partial signatures:
$$\sigma = \sigma_1^{\lambda_1} \cdot \sigma_2^{\lambda_2} \cdot \sigma_3^{\lambda_3}$$

### Verification
Verify $e(\sigma, g_2)=e(H(m), pk)$

## Challenges and Limitations
### Trusted Setup
- Issue: initial key generation may require trust among parties
- Solution: use distributed key generation protocols to eliminate need for trusted dealer

### Complex Coordination
- Issue: requires coordination among $t$ parties, which may be challenging in large or dynamic networks
- Solution: implement efficient protocols and allow for flexible thresholds

### Computational Overhead
- Issue: operations like pairing computations are resource-intensive
- Solution: optimise implementations and leverage hardware acceleration

## References and Further Reading
- “Short Signatures from the Weil Pairing” by Dan Boneh, Ben Lynn, and Hovav Shacham.
- “Threshold Cryptosystems” by Yvo Desmedt.
- “Secure Distributed Key Generation for Discrete-Log Based Cryptosystems” by R. Gennaro, S. Jarecki, H. Krawczyk, and T. Rabin.
- “A Survey of Threshold Cryptography in Blockchain” by M. M. Fernandes et al.
- “An Introduction to Threshold Cryptography” by Victor Shoup.

*Note: edited ChatGPT 01-preview output was used in the compilation of this document.*
