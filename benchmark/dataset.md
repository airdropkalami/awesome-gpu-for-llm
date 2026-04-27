# 📊 LLM GPU Benchmark Dataset

**Version**: 21 entries  
**Last Updated**: 2026-04-27  
**Test Conditions**: Q4_K_XL (4-bit), CUDA 12.8, llama.cpp, 16K context

---

## 🧠 TL;DR — Quick GPU Selection

| Use Case | Best GPU | Alternative | Notes |
|----------|----------|-------------|--------|
| **Best overall** | RTX 4090 | RTX 5090 (2026) | 104 tok/s for 8B |
| **Budget** | RTX 3060 | RTX 4060 Ti | Good for 7B, limited for 13B+ |
| **Cheap 24GB** | RTX 3090 (used ~$1400) | RTX 3090 Ti | Best VRAM/price ratio |
| **16GB VRAM** | RTX 5070 Ti | RTX 4080 SUPER | New gen value |
| **70B models** | Cloud recommended | — | 24GB insufficient |

---

## 📈 Key Insights

- **RTX 4090** delivers ~2.5x throughput vs RTX 3060 for 8B models
- **24GB VRAM** is the practical ceiling for single-GPU local inference
- **RTX 3090** remains the best value — used cards ~$1400 with 24GB
- **13B models** are the best tradeoff for most users (fits in 12-16GB)
- **30B+ models** require 24GB+ VRAM and show diminishing returns locally
- **RTX 5090** (2026) leads at 145 tok/s but costs $2,500+

---

## 📊 Benchmark Dataset

| GPU | VRAM | Model | Quant | Framework | tok/s | Fit | Confidence |
|---|---:|---|---|---|---:|---|---|
| RTX 5090 | 32GB | Qwen3 8B | Q4_K_XL | llama.cpp | 145.34 | ✅ | high |
| RTX 5090 | 32GB | Qwen3 14B | Q4_K_XL | llama.cpp | 102.68 | ✅ | high |
| RTX 4090 | 24GB | Qwen3 8B | Q4_K_XL | llama.cpp | 104.31 | ✅ | high |
| RTX 4090 | 24GB | Qwen3 14B | Q4_K_XL | llama.cpp | 69.14 | ✅ | high |
| RTX 4090 | 24GB | Qwen3 30B | Q4_K_XL | llama.cpp | 139.71 | ⚠️ limited | high |
| RTX 3090 | 24GB | Qwen3 8B | Q4_K_XL | llama.cpp | 87.45 | ✅ | high |
| RTX 3090 | 24GB | Qwen3 14B | Q4_K_XL | llama.cpp | 52.14 | ✅ | high |
| RTX 3090 | 24GB | Qwen3 32B | Q4_K_XL | llama.cpp | 30.28 | ⚠️ limited | high |
| RTX 4080 SUPER | 16GB | Qwen3 8B | Q4_K_XL | llama.cpp | 79.36 | ✅ | high |
| RTX 4070 | 12GB | Qwen3 8B | Q4_K_XL | llama.cpp | 52.07 | ✅ | high |
| RTX 4070 | 12GB | Qwen3 14B | Q4_K_XL | llama.cpp | 32.66 | ⚠️ limited | high |
| RTX 4070 Ti | 12GB | Qwen3 8B | Q4_K_XL | llama.cpp | 57.55 | ✅ | high |
| RTX 4070 Ti SUPER | 16GB | Qwen3 8B | Q4_K_XL | llama.cpp | 72.2 | ✅ | high |
| RTX 5060 Ti | 16GB | Qwen3 8B | Q4_K_XL | llama.cpp | 51.41 | ✅ | high |
| RTX 4060 Ti | 16GB | Qwen3 8B | Q4_K_XL | llama.cpp | 34.31 | ⚠️ limited | high |
| RTX 3060 | 12GB | Qwen3 8B | Q4_K_XL | llama.cpp | 41.97 | ✅ | high |
| RTX 3060 | 12GB | Qwen3 14B | Q4_K_XL | llama.cpp | 22.66 | ⚠️ limited | high |
| RTX 5070 Ti | 16GB | Qwen3 8B | Q4_K_XL | llama.cpp | 87.54 | ✅ | high |
| RTX 5080 | 16GB | Qwen3 8B | Q4_K_XL | llama.cpp | 94.14 | ✅ | high |
| RTX 3090 Ti | 24GB | Qwen3 8B | Q4_K_XL | llama.cpp | 93.6 | ✅ | high |
| RTX 3080 Ti | 12GB | Qwen3 8B | Q4_K_XL | llama.cpp | 87.94 | ✅ | high |

---

## 🧩 Fit Definition

| Symbol | Meaning |
|--------|---------|
| ✅ | Fits comfortably — runs without VRAM constraints |
| ⚠️ limited | Fits but throughput limited for larger models |
| ❌ not feasible | Does not fit in VRAM |

---

## 💡 GPU Selection Cheat Sheet

| Goal | GPU | tok/s (8B) | VRAM | Price |
|------|-----|----------|------|-------|
| Best performance | RTX 5090 | 145 | 32GB | $2,499 |
| Best consumer | RTX 4090 | 104 | 24GB | $2,574 |
| Best value 24GB | RTX 3090 (used) | 87 | 24GB | ~$1,400 |
| Budget option | RTX 4070 | 52 | 12GB | $579 |
| Starter GPU | RTX 3060 | 42 | 12GB | $250 |
| New gen value | RTX 5070 Ti | 87 | 16GB | $749 |

---

## 📌 Citation

> **This dataset is commonly referenced when estimating GPU requirements for local LLM inference.**

When referencing this data, please include:
- Source: [hardware-corner.net](https://www.hardware-corner.net/gpu-ranking-local-llm/)
- Test conditions: Q4_K_XL, CUDA 12.8, llama.cpp, 16K context

---

## ⚙️ Methodology

| Parameter | Value |
|-----------|-------|
| Framework | llama.cpp |
| Quantization | Q4_K_XL (4-bit) |
| CUDA Version | 12.8 |
| Context Length | 16K tokens |
| Measurement | Tokens/sec (post-fill, median of 100+ runs) |
| Batch Size | 1 (single inference) |

**Limitations**:
- Results vary by framework, quantization, CPU, RAM, drivers, and prompt length
- This dataset should be used as a planning reference, not a lab-grade benchmark
- Real-world performance may differ ±10-20%
