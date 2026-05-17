# ACE-Step 1.5 Knowledge Wiki

A comprehensive knowledge base for [ACE-Step 1.5](https://github.com/ace-step/ACE-Step-1.5) — the open-source music generation model. Built using the [Karpathy LLM Wiki pattern](https://blog.starmorph.com/blog/karpathy-llm-wiki-knowledge-base-guide) with [AutoResearch](https://x.com/karpathy/status/1921368644069789747) for autonomous quality improvement. Clone it, `cd` into it, run `claude`, and you have an AI assistant that knows everything about ACE-Step.

## What's Covered

- **Architecture** — Two-brain design (LM planner + DiT executor), Oobleck VAE, flow matching
- **All 6 task types** — text-to-music, covers, stem extraction, lego track addition, song completion, repainting
- **Model zoo** — 2B and 4B XL variants, base vs sft vs turbo, LM model selection
- **Stem extraction** — Generative extraction of 12 instrument stems (base model only)
- **Prompting** — Caption structure, lyrics formatting, metadata, ending strategy
- **API automation** — REST API batch generation, multi-model support
- **Community ecosystem** — VST3 plugin, DAW integration, alternative UIs, training tools
- **Installation & configuration** — Quick start, .env config, GPU tiers, platform support

## Quick Start

```bash
git clone https://github.com/yourusername/ace-step-wiki.git
cd ace-step-wiki
claude
```

Ask any question about ACE-Step — the assistant navigates via `wiki/index.md`, reads relevant pages, and synthesizes answers.

## Project Structure

```
ace-step-wiki/
├── CLAUDE.md              # Schema — turns Claude into an ACE-Step assistant
├── wiki/
│   ├── index.md           # Master catalog — start here
│   ├── log.md             # Operation log
│   ├── concepts/          # Architecture, models, tasks, prompting
│   ├── workflows/         # Step-by-step guides (stem extraction, API, etc.)
│   ├── how-to/            # Installation, configuration, training
│   ├── entities/          # Tools, UIs, integrations
│   └── sources/           # Source document summaries
├── raw/                   # Immutable source documents
└── outputs/               # Reports and exports
```

## Key Facts

- **Stem extraction requires the base model** — not turbo, not sft
- **XL (4B) models** offer higher quality than 2B across all tasks
- **Turbo = 8 steps, Base/SFT = 50 steps** — turbo is faster, base has more features
- **Max duration:** 600 seconds (10 minutes)
- **12 extractable stems:** vocals, backing_vocals, drums, bass, guitar, keyboard, percussion, strings, synth, fx, brass, woodwinds
