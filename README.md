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

ArXiv: [link coming — submission in progress]

## Status

| Component | Status |
|---|---|
| Encoding scheme | ✅ Specified |
| Provisional patent | ✅ Filed (Indian Patent Office, March 2026) |
| C reference implementation | 🔄 In progress |
| GGUF benchmark comparison | 🔄 Planned — v2 paper |
| llama.cpp integration | 🔄 Planned |

## C Implementation (coming soon)
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
