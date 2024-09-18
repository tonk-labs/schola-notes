---
id: 1726579238-bilinear-pairings
aliases:
  - bilinear pairings
tags: []
---

# bilinear pairings
Maps that take two points on an elliptic curve and output an element in a finite field, enabling advanced cryptographic protocols.

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
- *Short Signatures* — schemes such as [[BLS Threshold|BLS]] (Boneh-Lynn-Shacham) enable very short signatures with security based on hardness of certain problems in pairing-friendly groups
- *Attribute-Based Encryption (ABE)* — enables fine-grained access control over encrypted data
