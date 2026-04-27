# 🤖 Awesome GPU for LLM

**VRAM Requirements, Model Compatibility, and Real-World GPU Guide (2026)**

A practical, developer-focused resource for running local large language models (LLMs) efficiently — including VRAM requirements, GPU recommendations, benchmarks, and when to use cloud.

> If it doesn't fit in VRAM, it doesn't run.

[![GPU Platforms](https://img.shields.io/badge/GPU-CUDA%20%7C%20ROCm-blue?style=for-the-badge)](https://developer.nvidia.com/cuda-gpus)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=for-the-badge)](CONTRIBUTING.md)

---

# 🧠 TL;DR — Quick Decision Guide

| Scenario | Recommendation |
| ----------------- | -------------- |
| Just testing LLMs | RTX 4060 |
| Most developers | RTX 4090 |
| Want 24GB cheap | Used RTX 3090 |
| Running 70B+ | Cloud GPU |

---

# 📊 VRAM Requirements by Model Size

| Model Size | Min VRAM | Comfortable VRAM | Notes |
| ---------- | -------- | ---------------- | ---------------------- |
| 7B | 6GB | 8GB | Easy local inference |
| 13B | 10GB | 16GB | Best balance |
| 34B | 20GB | 24GB | High-end consumer GPUs |
| 70B | 40GB+ | 48GB+ | Cloud or multi-GPU |

> **Rule**: If it doesn't fit in VRAM, it doesn't run.

**Quick Formula**:
```
VRAM = (Parameters × Bytes per Weight) + KV Cache + Overhead
FP16: Parameters × 2 bytes
INT8:  Parameters × 1 byte  
INT4:  Parameters × 0.5 bytes
```

---

# 📈 Real-World Benchmark Data

## ⚡ Tokens/sec (Approximate, Q4_K_M quantization)

| GPU | VRAM | Llama 7B | Llama 13B | Llama 34B | Llama 70B |
|-----|------|----------|-----------|-----------|-----------|
| RTX 4060 | 8GB | 35 tok/s | 18 tok/s | ❌ | ❌ |
| RTX 4060 Ti | 16GB | 40 tok/s | 24 tok/s | ❌ | ❌ |
| RTX 4070 | 12GB | 45 tok/s | 22 tok/s | ❌ | ❌ |
| RTX 4070 Ti Super | 16GB | 50 tok/s | 28 tok/s | ❌ | ❌ |
| RTX 4090 | 24GB | 80 tok/s | 45 tok/s | 18 tok/s | ⚠️ Cloud |
| RTX 3090 | 24GB | 70 tok/s | 40 tok/s | 15 tok/s | ⚠️ Cloud |
| A100 | 80GB | 120 tok/s | 80 tok/s | 40 tok/s | 25 tok/s |

> **Notes**: Values vary based on quantization level (Q4_K_M vs Q8). Framework matters (llama.cpp vs vLLM). CPU may bottleneck on lower-end setups.

## 🧠 VRAM vs Model Fit

| GPU VRAM | Max Model (FP16) | Max Model (Q4) |
|----------|-----------------|----------------|
| 6GB | 3B | 7B |
| 8GB | 4B | 7B |
| 12GB | 7B | 13B |
| 16GB | 10B | 20B |
| 24GB | 14B | 34B |
| 40GB | 24B | 65B |
| 80GB | 45B | 70B+ |

> Full benchmark analysis: [bestgpuforllm.com/compare](https://bestgpuforllm.com/compare)

---

# ⚙️ GPU Recommendations for LLM

## 🔹 Entry Level — RTX 4060 / 4060 Ti

| Spec | Value |
|------|-------|
| VRAM | 8-16GB |
| Good for | 7B models, testing |
| Price | $300-400 new |
| Quantization | Q4 runs smooth |

## 🔹 Best Overall — RTX 4090

| Spec | Value |
|------|-------|
| VRAM | 24GB |
| Good for | 13B–34B models |
| Price | $1,500-1,800 new |
| Why | Best balance of VRAM + compute |

## 🔹 Budget 24GB — Used RTX 3090

| Spec | Value |
|------|-------|
| VRAM | 24GB |
| Good for | 13B–34B models |
| Price | $500-700 used |
| Why | Still the best value in used market |

## 🔹 High-End — RTX 5090

| Spec | Value |
|------|-------|
| VRAM | 32GB |
| Good for | Any consumer model |
| Price | $2,000+ |
| When | Maximum performance needed |

---

# 🤖 Model Compatibility Matrix

| Model | Min GPU | Recommended | Quantization Options |
| --------- | ------- | --------------- | --------------------- |
| Llama 3.1 8B | RTX 3060 | RTX 4060 | FP16, Q4, Q8 |
| Llama 3.1 70B | RTX 4090 ×2 | Cloud | Q4, Q8 |
| Mistral 7B | RTX 3060 | RTX 4060 | FP16, Q4 |
| Mixtral 8x7B | RTX 4070 | RTX 4090 | Q4 (needs ~24GB) |
| Qwen 2.5 14B | RTX 4070 | RTX 4090 | FP16, Q4 |
| Qwen 2.5 32B | RTX 4090 | RTX 4090 | Q4 |
| DeepSeek 33B | RTX 4090 | RTX 4090 | Q4 (fits in 24GB) |
| Phi-3 Medium | RTX 4060 | RTX 4070 | Q4 |

> For full compatibility details, visit: [bestgpuforllm.com/best-gpu-for-ollama](https://bestgpuforllm.com/best-gpu-for-ollama)

---

# 🧩 Common LLM Setups

## 🔧 Local Dev Setup (Budget)

| Component | Recommendation |
| --------- | --------------- |
| GPU | RTX 4060 8GB or RTX 4060 Ti 16GB |
| Framework | Ollama (easiest) or llama.cpp |
| Models | Llama 3.1 8B, Mistral 7B |
| Price | $300-500 total |

## ⚡ Serious Local Inference

| Component | Recommendation |
| --------- | --------------- |
| GPU | RTX 4090 24GB |
| Framework | vLLM (throughput) or Ollama (simplicity) |
| Models | Llama 3.1 13B, Qwen 2.5 14B, DeepSeek 33B |
| Price | $1,500-2,000 |

## 🏢 Production / Multi-GPU

| Component | Recommendation |
| --------- | --------------- |
| GPU | 2× RTX 4090 or RTX 3090 |
| Framework | vLLM |
| Models | Llama 70B, Mixtral |
| Use case | High throughput, concurrent users |

## ☁️ Cloud for Large Models

| Provider | Best For | Ref |
| -------- | -------- | --- |
| RunPod | LLM inference | [ref=7a4cz5kl](https://app.runpod.io?r=7a4cz5kl) |
| Vast.ai | Competitive pricing | Ask for deals |

---

# ☁️ When to Use Cloud Instead

## Use Cloud GPU When:

- ✅ Running 70B+ models regularly
- ✅ Need burst compute (not daily use)
- ✅ Want flexibility to switch GPU types
- ✅ Experimenting with different model sizes
- ✅ Batch processing with variable demand

## Use Local GPU When:

- ✅ Running inference daily (20+ hours/week)
- ✅ Privacy matters (data stays on your machine)
- ✅ Consistent performance is critical
- ✅ You're actively developing and iterating

**Break-even**: If you use GPU >20 hours/week, local is cheaper long-term.

> Full cost analysis: [TCO Calculator](https://bestgpuforllm.com/tco-calculator)

---

# ⚠️ Common Mistakes (And How to Avoid)

## ❌ Mistake 1: Buying Based on FLOPS Instead of VRAM

Benchmarks often show peak FLOPS but ignore memory constraints.

**Reality**: A RTX 4090 (330 TFLOPS) cannot run a 70B model. A RTX 3090 (330 TFLOPS) also can't. VRAM is the gate.

**Fix**: Start with your target model size, then find the GPU with enough VRAM.

## ❌ Mistake 2: Trying to Run Large Models on Small VRAM

You cannot "optimize around" insufficient VRAM.

**Reality**: 70B at FP16 needs 140GB. Even Q4 needs 35GB+. No consumer GPU handles this.

**Fix**: Use cloud for 70B+, or accept that local means 34B maximum on consumer hardware.

## ❌ Mistake 3: Overpaying for Unused Compute

A $1,600 RTX 4090 for running 7B models is wasteful.

**Reality**: 7B models run fine on RTX 4060 at a fraction of the cost.

**Fix**: Match GPU to your actual model size. Upgrade only when you hit VRAM limits.

## ❌ Mistake 4: Ignoring Quantization Tradeoffs

Q4 isn't "worse" — it's practical for most use cases.

**Reality**: Q4_K_M vs FP16 on 13B: 7GB vs 26GB. The quality difference for inference tasks is minimal.

**Fix**: Use Q4 quantization to fit larger models in your VRAM. The speed and capacity gains outweigh the minor quality loss.

---

# 🛠️ Framework Quick Reference

| Framework | Best For | NVIDIA | AMD ROCm | Apple MLX |
|-----------|----------|--------|----------|-----------|
| [Ollama](https://ollama.com) | Easiest setup | ✅ | ⚠️ | ✅ |
| [llama.cpp](https://github.com/ggerganov/llama.cpp) | Quantized models, Mac | ✅ | ✅ | ❌ |
| [vLLM](https://docs.vllm.ai) | High-throughput batching | ✅ | ❌ | ❌ |
| [LM Studio](https://lmstudio.ai) | Best GUI experience | ✅ | ❌ | ✅ |
| [text-generation-webui](https://github.com/oobabooga/text-generation-webui) | Most flexible | ✅ | ✅ | ❌ |

> For detailed framework comparisons, visit: [bestgpuforllm.com](https://bestgpuforllm.com)

---

# 📚 Detailed Guides

## From bestgpuforllm.com

| Guide | What It Covers |
| ----- | -------------- |
| [Best GPU for Ollama](https://bestgpuforllm.com/best-gpu-for-ollama) | Getting started with Ollama |
| [How Much VRAM for LLM?](https://bestgpuforllm.com/how-much-vram-for-local-llm) | Detailed VRAM breakdown |
| [RunPod vs Vast.ai](https://bestgpuforllm.com/runpod-vs-vast-ai) | Cloud GPU comparison |
| [GPU Comparison Tool](https://bestgpuforllm.com/compare) | Interactive GPU table |
| [VRAM Calculator](https://bestgpuforllm.com/vram-calculator) | Estimate your requirements |
| [Tokens/sec Predictor](https://bestgpuforllm.com/tokens-per-second) | Estimate inference speed |

---

# 🤝 Contributing

Contributions welcome! Please submit PRs for:

- New benchmark results (include testing conditions)
- GPU releases and pricing updates
- Model compatibility improvements
- Framework updates

**When adding benchmark data, please include**:
- GPU model and VRAM
- Model and quantization level (Q4_K_M, Q8, FP16, etc.)
- Framework and version
- Context length used
- Average tokens/sec over 100+ token generation

**Benchmark submission template**:
```markdown
## GPU Name

| Model | Quantization | Framework | Tokens/sec | Notes |
|-------|-------------|-----------|------------|-------|
| RTX 4090 | Q4_K_M | llama.cpp | 45 tok/s | 2048 ctx |
```

---

# ⭐️ Why This Repo Exists

Most LLM GPU guides have these problems:

- ❌ Too generic — not aligned with real dev workflows
- ❌ Outdated — prices and benchmarks change fast
- ❌ Marketing-focused — optimized for affiliate clicks, not developer needs

This repo focuses on:

- ✅ Real setups that developers actually use
- ✅ Actual constraints (VRAM first, compute second)
- ✅ Practical decisions (not theoretical benchmarks)

---

# 📌 Final Note

The best GPU is not the most expensive one.

It's the one that:

- ✅ Fits your model size in VRAM
- ✅ Matches your budget
- ✅ And doesn't overbuy for your actual needs

Get those 3 right, and you're already ahead of most people.

---

**⭐️ If this helped you, consider starring the repo!**

[![Star on GitHub](https://img.shields.io/github/stars/airdropkalami/awesome-gpu-for-llm?style=social)](https://github.com/airdropkalami/awesome-gpu-for-llm)
[![Follow on X](https://img.shields.io/twitter/follow/thurmon_demich?style=social)](https://twitter.com/thurmon_demich)