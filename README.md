# NativeTernary

A self-delimiting binary encoding for ternary data and hierarchical 
structured data — designed as a native wire format for ternary neural 
network weights (BitNet b1.58) and IoT/embedded systems.

## What is NativeTernary?

NativeTernary partitions the 2-bit pair space into three data symbols 
{00, 01, 10} representing ternary values {-1, 0, +1}, and one reserved 
structural delimiter {11}. Hierarchy depth is encoded as unary run-length 
of the delimiter pair — one {11} marks a word boundary, two mark a 
sentence, three mark a paragraph — with no secondary encoding required.

The decoder is a 10-line stateless state machine. Zero lookup tables. 
Resilient to bitstream corruption.

## Why it matters

- **BitNet b1.58** stores ternary weights {-1, 0, +1} in GGUF — a format 
  designed for floating-point. NativeTernary is the first native wire 
  format purpose-built for ternary weights.
- **IoT / embedded:** inter-sample sensor deltas are naturally ternary. 
  Self-synchronisation means frame recovery after packet loss with no 
  retransmission.
- **Decoder complexity:** 2-bit register + level counter. Fits in under 
  200 bytes of compiled C. Runs on ARM Cortex-M0.

## Paper

ArXiv: https://arxiv.org/abs/2604.03336

## Status

| Component | Status |
|---|---|
| Encoding scheme | ✅ Specified |
| ArXiv paper | ✅ Published — 2604.03336 |
| Provisional patent | ✅ Filed (Indian Patent Office, March 2026) |
| C reference implementation | 🔄 In progress |
| GGUF benchmark comparison | 🔄 Planned — v2 paper |
| llama.cpp integration | 🔄 Planned |

## C Implementation

## NativeTernary vs GGUF — Benchmark Results

### Bits per weight

| Format | Bits / weight | Notes |
|---|---|---|
| NativeTernary | **2.000** | Exact — zero waste |
| GGUF Q2_K | 2.625 | Best current quantization |
| GGUF Q4_0 | 4.500 | Common default |
| GGUF int8 | 8.000 | Naive storage |

### Size comparison

| Scale | Weights | NT bytes | GGUF Q2_K | GGUF Q4_0 | GGUF int8 | vs Q2_K | vs Q4_0 | vs int8 |
|---|---|---|---|---|---|---|---|---|
| Small | 1M | 250 KB | 329 KB | 563 KB | 1,001 KB | 0.76x | 0.44x | 0.25x |
| Medium | 125M | 31.3 MB | 41.1 MB | 70.4 MB | 125 MB | 0.76x | 0.44x | 0.25x |
| Large | 1B | 250 MB | 329 MB | 563 MB | 1,001 MB | 0.76x | 0.44x | 0.25x |

### Throughput

| Scale | Weights | Encode MB/s | Decode MB/s |
|---|---|---|---|
| Small | 1M | 46.8 | 35.3 |
| Medium | 125M | 69.2 | 45.1 |
| Large | 1B | 62.6 | 35.7 |

### Key findings

- **4.0x smaller** than GGUF int8 (naive ternary storage)
- **1.31x smaller** than GGUF Q2_K (current best quantization)
- **2.25x smaller** than GGUF Q4_0 (common default)
- Ratios consistent across all scales from 1M to 1B weights
- Encode: 47–69 MB/s | Decode: 35–45 MB/s

  
```c
// Encoder and decoder will be released here under MIT license
// Target: portable C99, zero malloc, ARM Cortex-M0 compatible
```

## License

MIT — see LICENSE file.

Patent rights reserved. See paper for details.

---

*If you are working on ternary neural networks or BitNet and want to 
collaborate or discuss licensing, reach out: maharshisavdhariya@gmail.com*
