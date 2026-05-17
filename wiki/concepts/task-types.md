---
title: Task Types
type: concept
tldr: "6 modes: generate, cover, extract, lego, complete, repaint"
related:
  - "[[model-zoo]]"
  - "[[stem-extraction]]"
  - "[[cover-remix]]"
  - "[[repainting]]"
  - "[[lego-track-addition]]"
  - "[[song-completion]]"
created: 2026-05-17
updated: 2026-05-17
confidence: high
---

# Task Types

ACE-Step 1.5 supports six generation modes. **Not all models support all tasks.**

## Task Compatibility Matrix

| Task | Base | SFT | Turbo | LM Used? |
|------|------|-----|-------|----------|
| text2music | Yes | Yes | Yes | Yes |
| cover | Yes | Yes | Yes | **No** (auto-skipped) |
| repaint | Yes | Yes | Yes | **No** (auto-skipped) |
| extract | **Yes** | No | No | **No** (auto-skipped) |
| lego | **Yes** | No | No | Optional |
| complete | **Yes** | No | No | Optional |

> [!warning] Extract, Lego, and Complete are **base model only** — both 2B and XL variants. Turbo and SFT models cannot do these tasks.

## 1. Text2Music (Default)

Generate music from text descriptions + optional metadata. All parameters available, LM active.

## 2. Cover (Style Transfer)

Transform existing audio while maintaining melodic structure. Requires `src_audio`.

Key parameter: `audio_cover_strength` (0.0-1.0):
- 1.0 = close to original structure
- 0.5-0.7 = moderate genre shift
- 0.2 = loose reinterpretation

Source audio is quantized into semantic codes containing melody, rhythm, chords, orchestration, and partial timbre.

## 3. Extract (Stem Separation) — Base Only

Isolate a specific instrument track from mixed audio. Generative extraction — the model reimagines what each stem sounds like.

Available stems: `vocals`, `backing_vocals`, `drums`, `bass`, `guitar`, `keyboard`, `percussion`, `strings`, `synth`, `fx`, `brass`, `woodwinds`

Set `task_type="extract"`, provide `src_audio`, set `instruction` like:
```
Extract the vocals track from the audio:
```

## 4. Lego (Track Addition) — Base Only

Add a new instrument track to existing audio that coordinates in rhythm and harmony.

Set `task_type="lego"`, provide `src_audio`, set `instruction` like:
```
Generate the guitar track based on the audio context:
```

## 5. Complete (Song Completion) — Base Only

Generate full accompaniment from a single track input (e.g., a cappella → full mix). Also used for Vocal-to-BGM.

Set `task_type="complete"`, provide `src_audio`, set `instruction` like:
```
Complete the input track with drums, bass, guitar:
```

## 6. Repaint (Audio Inpainting)

Regenerate a specific time segment (3-90 seconds) while keeping the rest unchanged.

Parameters: `repainting_start`, `repainting_end` (seconds)

Use cases:
- Fix a bad section without regenerating the whole song
- Intelligent audio stitching between two tracks
- **Infinite duration generation** via chained repaints
