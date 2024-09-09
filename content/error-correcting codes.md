---
id: 1725901374-error-correcting-codes
aliases:
  - error-correcting codes
tags: []
---

# error-correcting codes
Crucial aspect of information theory and digital communication. Allow for the reliable transmission of data over noisy channels.

## Basic Concept
- Error-correcting codes add redundancy to data to detect and correct errors that may occur during transmission or storage.

## Key Terms
- *Codeword* — a valid sequence of symbols in the code
- *Hamming distance* — the number of positions in which two codewords differ
- *Minimum distance* — the smallest Hamming distance between any two distinct codewords

## Types of Errors
- *Bit flips* — 0 becomes 1 or vice versa
- *Erasures* — bits are lost or unreadable
- *Burst errors* — multiple consecutive bits are corrupted

## Error Detection vs. Error Correction
- *Detection* — identify that an error has occurred
- *Correction* — identify and fix the error

## Linear Codes
- A class of codes where any linear combination of codewords is also a codeword

## Common Error-Correcting Codes
- Hamming Codes
    - Can correct single-bit errors and detect double-bit errors
    - Ex: (7,4) Hamming code encodes 4 data bits into 7 bits
- [[Reed-Solomon Codes]]
    - Effective against burst errors
    - Widely used in storage systems (CDs, DVDs) and digital TV
- Low-Density Parity-Check (LDPC) Codes
    - Approach the [[Shannon limit]] for channel capacity
    - Used in Wi-Fi, 10GBase-T Ethernet, and more
- Turbo Codes
    - Parallel concatenation of two or more convolutional codes
    - Used in 3G/4G mobile communications
- Polar Codes
    - Achieve channel capacity for binary-input symmetric memoryless channels
    - Used in 5G communications

## Encoding and Decoding
- Encoding: add redundancy to the original message
- Decoding: use redundancy to detect and correct errors

## Mathematical Foundations
- [[finite fields|Finite fields]] (Galois fields) are often used in the construction of error-correcting codes
- Linear algebra is crucial for understanding and analysing linear codes

## Key Properties
- *Code rate* — ratio of data bits to total bits (including redundancy)
- *Error-correction capability* — number of errors that can be corrected

## Techniques
- *Block codes* — operate on fixed-size blocks of data
- *Convolutional codes* — operate on a stream of data
- *Interleaving* — spreading burst errors across multiple codewords

## Applications
- Digital communication (mobile networks, satellite communications)
- Data storage (hard drives, SSDs, QR codes)
- Deep space communication
- DNA storage and sequencing

## Trade-offs
- More redundancy allows for better error correction but reduces data rate
- More complex codes can provide better performance but require more processing power

## Advanced Concepts
- *Soft-decision decoding* — using probabilistic information about received symbols
- *List decoding* — outputting a list of possible codewords
- *Rateless codes* — adapting to channel conditions without prior knowledge

## Simple Python Implementation
```python
def encode(data):
    # Add a parity bit (1 if odd number of 1s, 0 if even)
    return data + str(sum(int(bit) for bit in data) % 2)

def decode(received):
    # Check if parity is correct
    if sum(int(bit) for bit in received) % 2 == 0:
        return received[:-1]  # Return data without parity bit
    else:
        print("Error detected!")
        return None

# Example usage
original = "1011"
encoded = encode(original)
print(f"Encoded: {encoded}")

# Simulate error-free transmission
received = encoded
decoded = decode(received)
print(f"Decoded: {decoded}")

# Simulate transmission with error
received_with_error = encoded[:-1] + ('0' if encoded[-1] == '1' else '1')
decoded_with_error = decode(received_with_error)
print(f"Decoded (with error): {decoded_with_error}")
```
