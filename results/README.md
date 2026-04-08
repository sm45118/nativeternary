# Benchmark Results

## Key finding: 460x smaller framing overhead

NativeTernary vs GGUF on real BitNet b1.58 2B4T architecture (24 layers, ~170 tensors, 2B parameters):

| Format | Framing overhead |
| --- | --- |
| NativeTernary | 91 bytes |
| GGUF tensor headers | ~42 KB |
| **Ratio** | **460x smaller** |

## Bits per weight

| Format | Bits / weight | Size (1B weights) |
| --- | --- | --- |
| NativeTernary | 2.000 | 250 MB |
| GGUF Q2_K | 2.625 | 329 MB |
| GGUF Q4_0 | 4.500 | 563 MB |
| GGUF int8 | 8.000 | 1,001 MB |

## Size comparison (synthetic weights)

| Scale | Weights | NT | GGUF Q2_K | GGUF Q4_0 | GGUF int8 |
| --- | --- | --- | --- | --- | --- |
| Small | 1M | 250 KB | 329 KB | 563 KB | 1,001 KB |
| Medium | 125M | 31.3 MB | 41.1 MB | 70.4 MB | 125 MB |
| Large | 1B | 250 MB | 329 MB | 563 MB | 1,001 MB |

## Throughput

| Scale | Weights | Encode MB/s | Decode MB/s |
| --- | --- | --- | --- |
| Small | 1M | 46.8 | 35.3 |
| Medium | 125M | 69.2 | 45.1 |
| Large | 1B | 62.6 | 35.7 |

## Notes

- GGUF Q2_K overhead estimated at 2.625 bpw based on published block quantization parameters
- GGUF tensor header estimated at 256 bytes per tensor; actual size varies by model
- Synthetic uniform {-1, 0, +1} weight arrays used; real trained weights may vary
- Throughput measured on single-core commodity hardware

Full paper: https://arxiv.org/abs/2604.03336
