---
title: Model Zoo
type: concept
tldr: "2B and XL variants: base, sft, turbo"
related:
  - "[[architecture]]"
  - "[[task-types]]"
  - "[[vram-tiers]]"
created: 2026-05-17
updated: 2026-05-17
confidence: high
---

# Model Zoo

## DiT Models (2B)

| Model | Steps | CFG | All Tasks? | Fine-Tunable |
|-------|-------|-----|------------|-------------|
| `acestep-v15-base` | 50 | Yes | **Yes** (extract/lego/complete) | Easy |
| `acestep-v15-sft` | 50 | Yes | No (text2music/cover/repaint only) | Easy |
| `acestep-v15-turbo` | 8 | No | No (text2music/cover/repaint only) | Medium |
| `acestep-v15-turbo-shift1` | 8 | No | No | Medium |
| `acestep-v15-turbo-shift3` | 8 | No | No | Medium |
| `acestep-v15-turbo-continuous` | 8 | No | No (experimental, shift 1-5) | Medium |

### Turbo Sub-Variants

| Variant | Character |
|---------|-----------|
| turbo (default) | Balanced creativity + semantic clarity |
| turbo-shift1 | Richer detail, weaker semantics |
| turbo-shift3 | Clearer timbre, may sound "dry" |

## DiT XL Models (4B) — Higher Quality

| Model | Steps | CFG | All Tasks? |
|-------|-------|-----|------------|
| `acestep-v15-xl-base` | 50 | Yes | **Yes** |
| `acestep-v15-xl-sft` | 50 | Yes | No |
| `acestep-v15-xl-turbo` | 8 | No | No |

## LM Models

| Model | Base | Audio Understanding | Composition | Melody Copy |
|-------|------|---------------------|-------------|-------------|
| `acestep-5Hz-lm-0.6B` | Qwen3-0.6B | Medium | Medium | Weak |
| `acestep-5Hz-lm-1.7B` | Qwen3-1.7B | Medium | Medium | Medium |
| `acestep-5Hz-lm-4B` | Qwen3-4B | Strong | Strong | Strong |

## VAE Options

| VAE | Source | Notes |
|-----|--------|-------|
| official | Bundled in main download | Default Oobleck VAE |
| scragvae | `scragnog/Ace-Step-1.5-ScragVAE` | Community finetune, MIT license |

Select via `ACESTEP_VAE_CHECKPOINT` env var or Gradio dropdown.

## Download Commands

```bash
uv run acestep-download                              # Core (turbo + VAE + LM 1.7B)
uv run acestep-download --all                        # Everything
uv run acestep-download --model acestep-v15-xl-base  # Specific model
uv run acestep-download --list                       # List available
```
