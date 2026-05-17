---
title: How to Install ACE-Step 1.5
type: how-to
tldr: "Clone, uv sync, run"
related:
  - "[[model-zoo]]"
  - "[[how-to-configure]]"
  - "[[vram-tiers]]"
created: 2026-05-17
updated: 2026-05-17
confidence: high
---

# How to Install

## Requirements

| Item | Requirement |
|------|-------------|
| Python | 3.11-3.12 (stable release, not pre-release) |
| GPU | CUDA recommended; MPS / ROCm / Intel XPU / CPU also supported |
| VRAM | ≥4GB for DiT-only; ≥6GB for LLM+DiT |
| Disk | ~10GB for core models, more for XL/base variants |

## Quick Start

```bash
# 1. Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# 2. Clone & install
git clone https://github.com/ACE-Step/ACE-Step-1.5.git
cd ACE-Step-1.5
uv sync

# 3. Launch
uv run acestep          # Gradio UI (port 7860)
uv run acestep-api      # REST API (port 8001)
```

Models download automatically on first run.

## Download Specific Models

```bash
uv run acestep-download                              # Core (turbo + VAE + LM)
uv run acestep-download --model acestep-v15-xl-base  # XL base (stem extraction)
uv run acestep-download --model acestep-v15-xl-turbo # XL turbo (fast generation)
uv run acestep-download --all                        # Everything
```

## Shared Model Directory

Avoid duplicate downloads across installations:

```bash
# Add to ~/.bashrc or .env
export ACESTEP_CHECKPOINTS_DIR=~/ace-step-models
```

## Platform-Specific

| Platform | Backend | Notes |
|----------|---------|-------|
| NVIDIA CUDA | vllm or pt | Primary target, full support |
| Apple Silicon | MLX + MPS | mlx-lm for LM, MPS for DiT |
| AMD ROCm | pt | Set `HSA_OVERRIDE_GFX_VERSION` for RDNA2/3 |
| Intel XPU | pt | torch.compile disabled |
| CPU | pt | Very slow, inference only |

## Gotchas

- Python 3.11.0rc1 (pre-release) causes segfaults with vllm — use stable release
- ROCm on Windows requires Python 3.12 specifically
- `torchcodec` unavailable on ROCm and Intel XPU — falls back to soundfile
- LoRA cannot load on quantized (INT8) models — disable quantization first
