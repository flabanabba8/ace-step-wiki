---
title: ACE-Step 1.5 Architecture
type: concept
tldr: "Two-brain: LM planner + DiT executor"
related:
  - "[[model-zoo]]"
  - "[[flow-matching]]"
  - "[[distillation]]"
created: 2026-05-17
updated: 2026-05-17
confidence: high
---

# Architecture

ACE-Step 1.5 uses a **decoupled hybrid** three-component pipeline:

```
Text Input (tags + lyrics + metadata)
  → 5Hz LM (Qwen3-based, generates semantic audio codes via CoT)
  → DiT (flow matching, denoises in 25Hz latent space)
  → 1D Oobleck VAE Decoder (latents → 48kHz stereo waveform)
```

## The Two Brains

### 1. Language Model — The Planner

Based on Qwen3 (0.6B, 1.7B, or 4B variants). Generates a comprehensive song blueprint via chain-of-thought reasoning:

- Infers metadata (BPM, key, duration) from vague descriptions
- Optimizes captions and rewrites queries
- Generates 5Hz semantic audio codes containing melody, orchestration, and timbre information
- Uses FSQ (Finite Scalar Quantization) with ~64k codebook

**Auto-skipped** for cover, repaint, and extract tasks — these don't need planning.

| LM Model | Params | Audio Understanding | Composition | Melody Copy |
|----------|--------|---------------------|-------------|-------------|
| acestep-5Hz-lm-0.6B | 0.6B | Medium | Medium | Weak |
| acestep-5Hz-lm-1.7B | 1.7B | Medium | Medium | Medium |
| acestep-5Hz-lm-4B | 4B | Strong | Strong | Strong |

### 2. DiT — The Executor

Diffusion Transformer that turns the LM's plan into audio through rectified flow matching. Gradually denoises from noise to audio in 25Hz latent space.

- **2B variant:** Odd layers use sliding window attention, even layers use global GQA
- **4B XL variant:** 32 layers, 32 attention heads, hidden size 2560
- Input patchification: 25Hz → 12.5Hz (2x reduction)
- Conditioning via cross-attention from Qwen3 embeddings + timbre/lyric encoders

## 1D Oobleck VAE

NOT a mel-spectrogram VAE — operates directly on raw waveforms.

| Parameter | Value |
|-----------|-------|
| Sample rate | 48,000 Hz stereo |
| Latent dimension | 64 |
| Downsampling strides | [2, 4, 4, 6, 10] = 1920x total |
| Output rate | 25 Hz |
| Activation | Snake |

**Compression math:** 48000 Hz ÷ 1920 = 25 Hz latent rate. One minute of audio = 1,500 latent frames.

## What "5Hz" Means

- VAE produces 25Hz latents (64-dim)
- FSQ tokenizer compresses 25Hz → 5Hz discrete codes (~64k codebook)
- This 5:1 compression lets the LM reason over entire songs efficiently

## Reference Audio Processing

When reference audio is provided (covers, etc.):
1. Load and normalize to stereo 48kHz
2. If < 30 seconds, repeat to fill ≥ 30 seconds
3. Randomly select three 10-second segments (front, middle, back), concatenate
4. Encode via VAE `tiled_encode` into latents
5. Average temporal dimension — acts globally on entire generation (controls timbre, mixing style, atmosphere)
