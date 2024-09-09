---
id: 1725903044-fast-fourier-transform
aliases:
  - Fast Fourier Transform
tags: []
---

# Fast Fourier Transform
Highly efficient algorithm for computing the Discrete Fourier Transform (DFT) of a sequence. It's a fundamental tool in signal processing.

## Basic Concept
- FFT rapidly computes the DFT by factoring the DFT matrix into a product of sparse (mostly zero) factors
- Reduces the complexity from $O(N^2)$ for the naive DFT implementation to $O(N\log N)$, where $N$ is the length of the input sequence

## Key Ideas
- Divide and Conquer approach
- Exploits symmetries and periodicities in the complex [[roots of unity]]
- Recursively breaks down a DFT of size $N$ into smaller DFTs

## Cooley-Tukey Algorithm
- Most common FFT algorithm
    - Radix-2 Decimation in Time (DIT)
        - Splits the input sequence into even and odd-indexed sub-sequences
        - Recursively computes the DFT of these sub-sequences
        - Combines results using the [[butterfly operation]]
    - Radix-2 Decimation in Frequency (DIF)
        - Similar to DIT, but splits the output spectrum instead of the input sequence

## Other FFT Algorithms
- Split-Radix FFT: more efficient for power-of-two sizes
- Bluestein's Algorithm: handles arbitrary input sizes
- Rader's algorithm: efficient for prime-sized inputs

## Complexity
- Time complexity: $O(N\log N)$
- Space complexity: $O(N)$ for in-place algorithms

## Key Properties
- *Linearity* — $\text{FFT}(ax+by)=a\text{FFT}(x)+b\text{FFT}(y)$
- *Parseval's theorem* — energy conservation between time and frequency domains
- Convolution theorem — convolution in time domain equals multiplication in frequency domain

## Applications
- Digital signal processing
- Audio and image compression (e.g., MP3, JPEG)
- Polynomial multiplication
- Fast multiplication of large integers
- Solving partial differential equations
- Spectral analysis in various scientific fields

## Inverse FFT
- Inverse transform can be computed using the same algorithm with slight modifications
- Maintains $O(N\log N)$ complexity

## Simple Python Implementation of Cooley-Tukey
```python
import cmath

def fft(x):
    N = len(x)
    if N <= 1:
        return x
    even = fft(x[0::2])
    odd = fft(x[1::2])
    T = [cmath.exp(-2j * cmath.pi * k / N) * odd[k] for k in range(N // 2)]
    return [even[k] + T[k] for k in range(N // 2)] + \
           [even[k] - T[k] for k in range(N // 2)]

# Example usage
x = [1, 2, 3, 4, 5, 6, 7, 8]  # Input should have a length that is a power of 2
X = fft(x)
print("Input:", x)
print("FFT:", [f"{z.real:.2f} + {z.imag:.2f}j" for z in X])

# Verify by comparing with numpy's FFT
import numpy as np
np_fft = np.fft.fft(x)
print("NumPy FFT:", [f"{z.real:.2f} + {z.imag:.2f}j" for z in np_fft])
```

## Key points about the FFT
- Twiddle Factors: The complex exponentials $(e^{(-2πik/N)})$ are often called twiddle factors.
- Bit-Reversal: Many FFT implementations use a technique called bit-reversal to reorder input or output elements.
- Real-valued FFT: When the input is real-valued (as in many practical applications), specialized algorithms can nearly halve the computation time.
- Pruning: When many input values are zero or many output values are not needed, pruning techniques can speed up the computation.
- Parallel Implementations: The FFT algorithm is well-suited for parallel computing, including GPU implementations.
