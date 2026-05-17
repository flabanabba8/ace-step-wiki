---
title: ACE-Step 1.5 Wiki Index
type: index
updated: 2026-05-17
---

# ACE-Step 1.5 Knowledge Wiki

## Concepts

- [[architecture]] — Two-brain design: 5Hz LM planner + DiT executor + Oobleck VAE
- [[model-zoo]] — All DiT variants (2B/XL, base/sft/turbo), LM models (0.6B/1.7B/4B), VAE options
- [[task-types]] — Six generation modes: text2music, cover, extract, lego, complete, repaint
- [[flow-matching]] — Rectified flow formulation, ODE vs SDE inference, DCW enhancement
- [[distillation]] — Turbo models: DMD2 + GAN, 50→8 steps, dynamic-shift sampling
- [[vram-tiers]] — GPU auto-detection, VRAM budgets, quantization, offloading strategies
- [[prompting]] — Caption structure, lyrics formatting, section markers, vocal control tags

## Workflows

- [[text-to-music]] — Standard generation from tags + lyrics
- [[stem-extraction]] — Isolate instrument stems (base model only)
- [[cover-remix]] — Style transfer while preserving melodic structure
- [[repainting]] — Regenerate time-bounded segments, infinite duration via chaining
- [[lego-track-addition]] — Add instruments to existing audio (base model only)
- [[song-completion]] — Generate accompaniment from a single track (base model only)
- [[lora-training]] — LoRA and LoKr fine-tuning for custom styles
- [[batch-api]] — REST API automation for batch generation

## How-To Guides

- [[how-to-install]] — Quick start, uv setup, launch scripts, portable packages
- [[how-to-configure]] — .env file, environment variables, model selection
- [[how-to-extract-stems]] — Step-by-step stem extraction with the base model
- [[how-to-train-lora]] — Dataset preparation, training parameters, evaluation

## Entities

- [[gradio-ui]] — Built-in web interface (port 7860)
- [[rest-api]] — REST API server (port 8001) with multi-model support
- [[openrouter-api]] — OpenAI-compatible chat completions API (port 8002)
- [[acestep-vst3]] — Official VST3 plugin for DAW integration
- [[ace-step-daw]] — Official standalone DAW project
- [[community-tools]] — Alternative UIs, training tools, integrations

## Quick Reference

- **Installation:** `git clone ... && uv sync && uv run acestep`
- **Checkpoints:** `~/Documents/Projects/ACE-Step-1.5/checkpoints/`
- **Stem extraction requires:** base model (not turbo or sft)
- **Max duration:** 600 seconds (10 minutes)
- **Turbo:** 8 steps, CFG=disabled | **Base/SFT:** 50 steps, CFG=5-9
