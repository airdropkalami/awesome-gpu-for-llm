# Awesome GPU for LLM

**Local LLM GPU Guide, VRAM Table, Benchmark References, and Model Compatibility**

A practical reference for running local LLMs efficiently — covering VRAM requirements, GPU recommendations, benchmark data, and model compatibility for Ollama, llama.cpp, vLLM, Open WebUI, and major open-source models.

> If it doesn't fit in VRAM, it doesn't run.

[![GPU Platforms](https://img.shields.io/badge/GPU-CUDA%20%7C%20ROCm-blue?style=for-the-badge)](https://developer.nvidia.com/cuda-gpus)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=for-the-badge)](CONTRIBUTING.md)

---

## Quick Decision Table

| Scenario | GPU | VRAM Needed |
|----------|-----|-------------|
| 7B models / testing | RTX 4060 | 8–12GB |
| 13B models | RTX 4070 Ti Super | 16GB |
| 34B models | RTX 4090 / RTX 3090 | 24GB |
| 70B+ models | Cloud GPU | 48GB+ |

---

## VRAM Requirements by Model

| Model | Size | Min VRAM | Comfortable | Recommended GPU |
|-------|------|----------|-------------|-----------------|
| Llama 3.1 | 8B | 6GB | 8GB | RTX 4060 |
| Llama 3.1 | 13B | 10GB | 16GB | RTX 4070 |
| Llama 3.1 | 70B | 40GB | 48GB+ | Cloud |
| Mistral | 7B | 6GB | 8GB | RTX 4060 |
| Mixtral | 8x7B | 20GB | 24GB | RTX 4090 |
| Qwen 2.5 | 14B | 12GB | 16GB | RTX 4070 |
| Qwen 2.5 | 32B | 20GB | 24GB | RTX 4090 |
| DeepSeek | 33B | 18GB | 24GB | RTX 4090 |
| Phi-3 | Medium | 8GB | 12GB | RTX 4060 |
| Gemma | 9B | 8GB | 12GB | RTX 4060 |

**The Rule**: If it doesn't fit in VRAM, it doesn't run.

**Quick Formula**:
```
VRAM = (Parameters × Bytes per Weight) + KV Cache + Overhead

FP16: Parameters × 2 bytes
INT8:  Parameters × 1 byte
Q4:    Parameters × 0.5 bytes

KV Cache (FP16): ~1GB per 2048 tokens for 7B models
Overhead: 1–3GB depending on framework
```

---

## GPU Recommendations

### Entry Level — RTX 4060 / RTX 4060 Ti

| Spec | Value |
|------|-------|
| VRAM | 8–16GB |
| Best for | 7B models, testing |
| Price | $300–400 new |
| Can run | Llama 8B, Mistral 7B, Phi-3 Medium |

### Mid-Range — RTX 4070 Ti Super

| Spec | Value |
|------|-------|
| VRAM | 16GB |
| Best for | 13B models, Q4 quantization |
| Price | $700–900 |
| Can run | Llama 13B, Qwen 14B at Q4 |

### Best Value — RTX 4090

| Spec | Value |
|------|-------|
| VRAM | 24GB |
| Best for | 13B–34B models, any local LLM |
| Price | $1,500–1,800 new, $1,000–1,400 used |
| Can run | Any consumer model at Q4 |

### Budget 24GB — Used RTX 3090

| Spec | Value |
|------|-------|
| VRAM | 24GB |
| Best for | 13B–34B models on budget |
| Price | $500–700 used |
| Why | Best price/performance in used market |

---

## Real-World Benchmarks

### Tokens/sec (Q4_K_M, 2048 context, llama.cpp)

| GPU | 7B | 13B | 34B | 70B |
|-----|---:|---:|---:|---:|
| RTX 4060 (8GB) | 35 | 18 | ❌ | ❌ |
| RTX 4060 Ti (16GB) | 40 | 24 | ❌ | ❌ |
| RTX 4070 (12GB) | 45 | 22 | ❌ | ❌ |
| RTX 4070 Ti Super (16GB) | 50 | 28 | ❌ | ❌ |
| RTX 4090 (24GB) | 80 | 45 | 18 | ⚠️ |
| RTX 3090 (24GB) | 70 | 40 | 15 | ⚠️ |
| A100 (80GB) | 120 | 80 | 40 | 25 |

> **Notes**: Q4_K_M quantization. Framework: llama.cpp. ❌ = does not fit. ⚠️ = fits with Q4 but slow for 70B.

### VRAM vs Maximum Model

| GPU VRAM | Max Model (FP16) | Max Model (Q4_K_M) |
|----------|-----------------|-------------------|
| 6GB | 3B | 7B |
| 8GB | 4B | 7B |
| 12GB | 7B | 13B |
| 16GB | 10B | 20B |
| 24GB | 14B | 34B |
| 40GB | 24B | 65B |
| 80GB | 45B | 70B+ |

---

## Model Compatibility

| Model | FP16 | Q8 | Q4 | Q2 | Min GPU |
|-------|------|----|----|----|---------|
| Llama 3.1 8B | 16GB | 9GB | 5GB | 3GB | RTX 3060 |
| Llama 3.1 13B | 26GB | 14GB | 8GB | 5GB | RTX 4070 |
| Llama 3.1 70B | 140GB | 70GB | 38GB | 22GB | Cloud |
| Mistral 7B | 14GB | 8GB | 5GB | 3GB | RTX 3060 |
| Mixtral 8x7B | 56GB | 30GB | 16GB | 10GB | RTX 4090 |
| Qwen 2.5 14B | 28GB | 15GB | 9GB | 5GB | RTX 4070 |
| Qwen 2.5 32B | 64GB | 34GB | 19GB | 11GB | RTX 4090 |
| DeepSeek 33B | 66GB | 35GB | 19GB | 11GB | RTX 4090 |
| Phi-3 Medium | 16GB | 9GB | 5GB | 3GB | RTX 4060 |
| Gemma 9B | 18GB | 10GB | 6GB | 3.5GB | RTX 4060 |

> Full compatibility table: [bestgpuforllm.com/compare](https://bestgpuforllm.com/compare)

---

## Framework Support

| Framework | Best For | 7B | 13B | 34B | Notes |
|-----------|----------|:--:|:--:|:--:|-------|
| [Ollama](https://ollama.com) | Easiest setup | ✅ | ✅ | ✅ | One-command local LLM |
| [llama.cpp](https://github.com/ggerganov/llama.cpp) | Quantized models | ✅ | ✅ | ✅ | Best for Q4/Q8 |
| [vLLM](https://docs.vllm.ai) | High throughput | ✅ | ✅ | ⚠️ | No AMD ROCm |
| [LM Studio](https://lmstudio.ai) | GUI experience | ✅ | ✅ | ✅ | Great for beginners |
| [Open WebUI](https://openwebui.com) | Self-hosted ChatGPT | ✅ | ✅ | ✅ | Works with Ollama backend |

### Framework Quick Notes

**Ollama**: Best for beginners. One command: `ollama run llama3`. Supports Mac+Metal.

**llama.cpp**: Best for quantization.GGUF format, runs on everything. Primary choice for Q4/Q8.

**vLLM**: Best for production throughput. Paged attention, batch inference. CUDA only.

**LM Studio**: Best GUI. Visual model browser, slider for parameters, local server mode.

**Open WebUI**: Best for ChatGPT-like experience. Works with Ollama backend. Supports RAG.

---

## Cloud vs Local Decision

### Use Local When:

- ✅ Running inference daily (20+ hours/week)
- ✅ Privacy is critical
- ✅ Consistent performance matters
- ✅ Active development / iteration

### Use Cloud When:

- ✅ 70B+ models needed
- ✅ Occasional burst compute
- ✅ Flexibility across GPU types
- ✅ Experimenting with various models

**Break-even**: Local cheaper if you use >20 hours/week.

| Provider | Best For | Link |
|----------|----------|------|
| RunPod | LLM inference | [ref=7a4cz5kl](https://app.runpod.io?r=7a4cz5kl) |
| Vast.ai | Competitive pricing | Ask for deals |

> TCO Calculator: [bestgpuforllm.com/tco-calculator](https://bestgpuforllm.com/tco-calculator)

---

## Common LLM Setups

### Casual / Testing — RTX 4060

```
GPU: RTX 4060 8GB
Framework: Ollama
Models: Llama 3.1 8B, Mistral 7B
Cost: $300–400
```

### Daily Driver — RTX 4070 Ti Super

```
GPU: RTX 4070 Ti Super 16GB
Framework: Ollama + Open WebUI
Models: Llama 3.1 13B, Qwen 2.5 14B
Cost: $700–900
```

### Serious Inference — RTX 4090

```
GPU: RTX 4090 24GB
Framework: vLLM or Ollama
Models: Llama 3.1 70B (Q4), DeepSeek 33B
Cost: $1,500–2,000
```

### Budget Powerhouse — Used RTX 3090

```
GPU: RTX 3090 24GB (used, ~$500–700)
Framework: llama.cpp
Models: Llama 3.1 13B, Mixtral 8x7B at Q4
Cost: $500–700
```

---

## Common Mistakes

### ❌ Buying Based on FLOPS Instead of VRAM

Peak FLOPS don't matter if your model doesn't fit.

**Fix**: Check VRAM requirements first, compute second.

### ❌ Trying to Run Large Models on Small VRAM

You cannot optimize around insufficient VRAM.

**Fix**: Use Q4 quantization or cloud for 70B+.

### ❌ Overbuying for Your Model Size

RTX 4090 for 7B models is wasteful.

**Fix**: Match GPU to actual model size. Upgrade when you hit limits.

### ❌ Ignoring Quantization

FP16 is not always better.

**Fix**: Q4_K_M on 13B: 7GB vs 26GB. Quality loss is minimal for inference tasks.

---

## Detailed Guides

| Guide | Covers |
| ----- | ------ |
| [Best GPU for Ollama](https://bestgpuforllm.com/best-gpu-for-ollama) | Ollama-specific setup |
| [How Much VRAM for LLM?](https://bestgpuforllm.com/how-much-vram-for-local-llm) | VRAM deep dive |
| [RunPod vs Vast.ai](https://bestgpuforllm.com/runpod-vs-vast-ai) | Cloud comparison |
| [GPU Comparison](https://bestgpuforllm.com/compare) | Interactive table |
| [VRAM Calculator](https://bestgpuforllm.com/vram-calculator) | Estimate needs |
| [Tokens/sec Predictor](https://bestgpuforllm.com/tokens-per-second) | Speed estimation |

---

## Contributing

Contributions welcome! Please submit PRs for:

- New benchmark results (include testing conditions)
- Model compatibility updates
- GPU releases and pricing
- Framework updates

**When adding benchmark data**:
```markdown
## GPU Name

| Model | Quantization | Framework | Tokens/sec | Notes |
|-------|-------------|-----------|------------|-------|
| RTX 4090 | Q4_K_M | llama.cpp | 45 tok/s | 2048 ctx |
```

**Include**: GPU, VRAM, model, quantization, framework, version, context length, tokens/sec over 100+ tokens.

---

## Why This Repo

Most LLM GPU guides are:

- ❌ Generic, not developer-focused
- ❌ Outdated fast (prices/benchmarks change)
- ❌ Marketing-led, not practical

This repo focuses on:

- ✅ Real developer setups
- ✅ Actual VRAM constraints
- ✅ Practical decisions over theoretical specs

---

**The best GPU is not the most expensive. It's the one that fits your model in VRAM and matches your actual workload.**

---

⭐️ **Star if this helped!**

[![Star on GitHub](https://img.shields.io/github/stars/airdropkalami/awesome-gpu-for-llm?style=social)](https://github.com/airdropkalami/awesome-gpu-for-llm)
[![Follow on X](https://img.shields.io/twitter/follow/thurmon_demich?style=social)](https://twitter.com/thurmon_demich)