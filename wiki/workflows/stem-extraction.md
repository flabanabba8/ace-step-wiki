---
title: Stem Extraction Workflow
type: workflow
tldr: "Isolate instrument stems from mixed audio"
related:
  - "[[task-types]]"
  - "[[model-zoo]]"
  - "[[how-to-extract-stems]]"
created: 2026-05-17
updated: 2026-05-17
confidence: high
---

# Stem Extraction

Isolate individual instrument tracks from a mixed audio file. **Requires the base model** (`acestep-v15-base` or `acestep-v15-xl-base`).

## Available Stems

`vocals`, `backing_vocals`, `drums`, `bass`, `guitar`, `keyboard`, `percussion`, `strings`, `synth`, `fx`, `brass`, `woodwinds`

## How It Works

This is **generative extraction** — the model reimagines what each stem sounds like based on its understanding of the mix. Different from DSP-based separation (like Demucs) which mathematically isolates signals.

| | ACE-Step Extract | Demucs |
|---|---|---|
| Method | Generative (reimagines) | Signal processing (isolates) |
| Stems | 12 instrument categories | 4-6 categories |
| Artifacts | May add/change things | Bleed/leakage but faithful |
| Best for | Creative, unusual instruments | Accurate mixing, remixing |

## Gradio UI

1. Select the **base** model from the model dropdown
2. Set task type to **Extract**
3. Upload source audio
4. Set instruction: `Extract the vocals track from the audio:`
5. Generate

Repeat for each stem you need.

## REST API

```json
{
  "task_type": "extract",
  "src_audio": "/path/to/song.mp3",
  "instruction": "Extract the drums track from the audio:",
  "thinking": false
}
```

Submit to `POST /release_task`, poll `POST /query_result`.

## Tips

- LM is auto-skipped for extraction regardless of settings
- Run extraction once per stem — each call isolates one instrument
- Results are generative, not exact — compare with Demucs for critical work
- XL base model produces higher quality extractions than 2B base
