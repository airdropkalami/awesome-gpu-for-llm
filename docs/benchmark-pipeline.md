# Benchmark Report Generation Pipeline

## Overview

When the user approves new benchmark data (`data/benchmark.json`), run this pipeline to:
1. Deduplicate entries in `benchmark.json`
2. Generate `benchmark/dataset.md` in the approved format
3. Push to GitHub

## Approved Format Structure

```
# 📊 LLM GPU Benchmark Dataset

**Version**: {count} entries
**Last Updated**: {date}
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
{rows}

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
```

## Generation Script

Location: `/root/.openclaw/workspace-mina/projects/aaan/scripts/generate_markdown.py`

## Deduplication Rules

- Primary key: `{gpu}_{model_size}` (e.g., `rtx4090_8b`)
- On conflict: keep newer entry (based on `last_updated` or source date)
- Keep highest `tokens_per_sec` on same gpu+model

## Usage

```bash
cd /root/.openclaw/workspace-mina/projects/aaan
python3 scripts/generate_markdown.py
```

## Git Workflow

```bash
cd /tmp/llm-repo-work
git add benchmark/dataset.md
git commit -m "feat: update benchmark dataset $(date +%Y-%m-%d)"
git push origin main
```

## Key Design Principles

1. **TL;DR first** — 80% of readers only scan this
2. **Insights over data** — Key observations before raw numbers
3. **Decision-friendly** — Cheat Sheet for quick choices
4. **Citation anchor** — Explicitly invite referencing
5. **Technical depth at bottom** — Methodology for those who need it
