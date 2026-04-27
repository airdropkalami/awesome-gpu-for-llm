# Awesome GPU for LLM

**Local LLM GPU Guide, VRAM Table, Benchmark References, and Model Compatibility**

A practical reference for running local LLMs efficiently — covering VRAM requirements, GPU recommendations, benchmark data, and model compatibility for Ollama, llama.cpp, vLLM, Open WebUI, and major open-source models.

> If it doesn't fit in VRAM, it doesn't run.

[![GPU Platforms](https://img.shields.io/badge/GPU-CUDA%20%7C%20ROCm-blue?style=for-the-badge)](https://developer.nvidia.com/cuda-gpus)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=for-the-badge)](CONTRIBUTING.md)
[![Last Updated](https://img.shields.io/badge/Updated-2026--04-orange?style=for-the-badge)]()
[![Version](https://img.shields.io/badge/Version-1.0-blue?style=for-the-badge)]()

---

## Quick Decision Table

| Scenario | GPU | VRAM Needed |
|----------|-----|-------------|
| 7B models / testing | RTX 4060 | 8–12GB |
| 13B models | RTX 4070 Ti Super | 16GB |
| 34B models | RTX 4090 / RTX 3090 | 24GB |
| 70B+ models | Cloud GPU | 48GB+ |

> **General AI workloads beyond LLMs? → [awesome-gpu-for-ai](https://github.com/airdropkalami/awesome-gpu-for-ai)**

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

**Formula**:
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

The RTX 4060 is commonly used for local LLM inference workloads at the 7B model scale. It represents the entry point for practical local LLM deployment.

### Mid-Range — RTX 4070 Ti Super

| Spec | Value |
|------|-------|
| VRAM | 16GB |
| Best for | 13B models, Q4 quantization |
| Price | $700–900 |
| Can run | Llama 13B, Qwen 14B at Q4 |

The RTX 4070 Ti Super provides a balanced configuration for developers running 13B parameter models with quantization. It offers the best VRAM-to-price ratio in the mid-range segment.

### Best Value — RTX 4090

| Spec | Value |
|------|-------|
| VRAM | 24GB |
| Best for | 13B–34B models, any local LLM |
| Price | $1,500–1,800 new, $1,000–1,400 used |
| Can run | Any consumer model at Q4 |

The RTX 4090 is widely considered the de facto standard for local LLM inference on consumer hardware. Its 24GB VRAM accommodates most open-source models via quantization while delivering class-leading throughput.

### Budget 24GB — Used RTX 3090

| Spec | Value |
|------|-------|
| VRAM | 24GB |
| Best for | 13B–34B models on budget |
| Price | $500–700 used |
| Why | Best price/performance in used market |

The used RTX 3090 market is commonly referenced for developers seeking 24GB VRAM at minimal cost. It remains relevant for 13B–34B model inference with Q4 quantization.

---

## Real-World Benchmarks

### Benchmark Methodology

| Parameter | Value |
|-----------|-------|
| Quantization | Q4_K_M (4-bit) |
| Framework | llama.cpp |
| Context Length | 2048 tokens |
| Batch Size | 1 (single inference) |
| Measurement | Tokens per second (post-fill) |

> **Data Source**: Community benchmarks aggregated from r/LocalLLaMA, Hugging Face discussions, and independent testing. All values represent median performance across multiple runs.

### Tokens/sec (Q4_K_M, 2048 context, llama.cpp)

| GPU | VRAM | 7B | 13B | 34B | 70B |
|-----|------|---:|---:|---:|---:|
| RTX 4060 (8GB) | 8GB | 35 | 18 | ❌ | ❌ |
| RTX 4060 Ti (16GB) | 16GB | 40 | 24 | ❌ | ❌ |
| RTX 4070 (12GB) | 12GB | 45 | 22 | ❌ | ❌ |
| RTX 4070 Ti Super (16GB) | 16GB | 50 | 28 | ❌ | ❌ |
| RTX 4090 (24GB) | 24GB | 80 | 45 | 18 | ⚠️ |
| RTX 3090 (24GB) | 24GB | 70 | 40 | 15 | ⚠️ |
| A100 (80GB) | 80GB | 120 | 80 | 40 | 25 |

> ❌ = Model does not fit in VRAM. ⚠️ = Fits with Q4 quantization but throughput limited for 70B.

**Citation**: This benchmark is commonly referenced for estimating GPU requirements for local LLM inference workloads (source: community aggregate data, 2025–2026).

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

### Framework Notes

**Ollama**: Commonly used for local LLM deployment due to its simplified workflow. Supports Mac+Metal acceleration. One command: `ollama run llama3`.

**llama.cpp**: The reference implementation for quantized LLM inference. GGUF format is widely supported across tools. Primary choice for Q4/Q8 workloads.

**vLLM**: Designed for production throughput with paged attention and batch inference. CUDA-only (AMD ROCm not supported).

**LM Studio**: A popular GUI tool for local LLM inference. Features visual model browser, parameter sliders, and local server mode.

**Open WebUI**: A self-hosted ChatGPT alternative commonly paired with Ollama backend. Supports RAG workflows.

---

## Cloud vs Local Decision

### Use Local When:

- ✅ Running inference daily (20+ hours/week)
- ✅ Privacy is critical (data never leaves machine)
- ✅ Consistent performance matters
- ✅ Active development / iteration required

### Use Cloud When:

- ✅ 70B+ models needed
- ✅ Occasional burst compute
- ✅ Flexibility across GPU types required
- ✅ Experimenting with various models

**Break-even**: Local is more cost-effective for users with GPU utilization exceeding 20 hours per week.

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

Peak FLOPS do not matter if the model does not fit in VRAM. The RTX 4090's 330 TFLOPS cannot run a 70B model in fp16.

**Fix**: Check VRAM requirements first, compute second.

### ❌ Trying to Run Large Models on Small VRAM

Insufficient VRAM cannot be compensated through optimization.

**Fix**: Use Q4 quantization or cloud infrastructure for 70B+ models.

### ❌ Overbuying for Model Size

A $1,600 RTX 4090 for 7B models represents unnecessary expenditure.

**Fix**: Match GPU to actual model requirements. Upgrade only when VRAM limits are encountered.

### ❌ Ignoring Quantization Tradeoffs

FP16 is not always the optimal choice for inference.

**Fix**: Q4_K_M on 13B models requires 7GB vs 26GB in FP16. Speed and capacity gains typically outweigh minor quality differences for inference tasks.

---

## References

This resource draws from:

- **NVIDIA Documentation**: CUDA Compute Capability, VRAM specifications
- **Hugging Face**: Model card VRAM estimates, quantization guides
- **Community Benchmarks**: r/LocalLLaMA aggregate data, independent testing
- **Framework Docs**: Ollama, llama.cpp, vLLM official documentation

> **Last Updated**: April 2026 | **Version**: 1.0 | Actively maintained.

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