# Contributing to awesome-gpu-for-llm

Thank you for your interest in contributing!

## How to Contribute

### Adding Benchmark Data

When adding benchmark results, please include:

1. **GPU model and VRAM** — e.g., "RTX 4090 24GB"
2. **Model and quantization** — e.g., "Llama 3.1 13B Q4_K_M"
3. **Framework and version** — e.g., "llama.cpp v0.1.45"
4. **Context length** — e.g., "2048 tokens"
5. **Average tokens/sec** — run at least 100 tokens for accuracy
6. **Testing conditions** — batch size, temperature, etc.

### Adding Model Compatibility

When adding model compatibility data:

1. Test with actual hardware (not theoretical)
2. Note the quantization method used
3. Include any stability issues or notes

### GPU Release Updates

When new GPUs are released:

1. Add to the appropriate tier (Entry/Best/Budget/High-End)
2. Update benchmark data if available
3. Include pricing and availability notes

## Pull Request Guidelines

- Keep descriptions clear and concise
- Reference the source of benchmark data (own testing preferred)
- Update the table of contents if adding new sections
- Test links before submitting PR

## Reporting Issues

- Use GitHub Issues for bugs, broken links, or outdated data
- Include your testing setup when reporting benchmark discrepancies
- Suggest improvements and new content ideas welcome

---

**Note**: This repo focuses on real-world, developer-focused benchmarks — not synthetic benchmarks. Please include actual testing conditions with your data.
