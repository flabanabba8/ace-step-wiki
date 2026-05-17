---
title: Prompting Guide
type: concept
tldr: "Tags + lyrics structure for quality output"
related:
  - "[[text-to-music]]"
  - "[[task-types]]"
created: 2026-05-17
updated: 2026-05-17
confidence: high
---

# Prompting Guide

## Caption (Tags)

Combine 3-4 dimensions: **genre + instruments + mood + production style + vocal type**

Good: `"aggressive death metal with blast beats, down-tuned guitars, guttural vocals, raw production"`

Bad: `"good music"`, `"a song"`, contradictions like `"fast slow"`

### Critical Rules

1. **Tags and lyrics must align** — conflicting descriptions degrade quality severely
2. **Don't put BPM/key/tempo in caption** — use the dedicated metadata parameters
3. **Be specific about instruments** — named instruments > vague genres
4. **Specify vocal type explicitly** — gender, style, technique
5. **Include ending instructions** — or tracks cut abruptly (e.g., "long decaying reverb outro fading into silence")
6. **Max 512 characters**

## Lyrics

### Section Markers

`[Intro]`, `[Verse]`, `[Pre-Chorus]`, `[Chorus]`, `[Bridge]`, `[Outro]`, `[Instrumental]`, `[Guitar Solo]`, `[Drop]`, `[Breakdown]`, `[Fade Out]`, `[Silence]`

### Vocal Technique

`[raspy vocal]`, `[whispered]`, `[falsetto]`, `[powerful belting]`, `[spoken word]`, `[harmonies]`, `[call and response]`, `[ad-lib]`

### Energy

`[high energy]`, `[building energy]`, `[explosive]`, `[melancholic]`

### Combined (max 2-3 attributes)

`[Chorus - anthemic]`, `[Verse - whispered]`

> [!warning] Don't stack too many tags: `[Chorus - anthemic - stacked - high energy - powerful - epic]` risks the model singing the tags as lyrics.

### Formatting Tips

- **6-10 syllables per line** for best rhythmic alignment
- **UPPERCASE** for emphasis/intensity
- **(Parentheses)** for backing vocals
- **Extended vowels:** "Feeeling" for drawn-out notes
- **`[Instrumental]`** with `...` lines for instrumental sections
- **Max 4096 characters**

## Metadata Parameters

| Parameter | Range | Notes |
|-----------|-------|-------|
| BPM | 30-300 | Extreme values may be unstable (sparse training data) |
| Key | e.g., "C Major", "D minor" | Guidance, not precise — BPM 120 may produce 118-122 |
| Time Signature | 2, 3, 4, or 6 | |
| Duration | 10-600 seconds | Quality may degrade above ~4 minutes |
| Language | 50+ supported | Top quality: en, zh, ru, es, ja, de, fr, pt, it, ko |

## LM Parameters (Advanced)

| Parameter | Default | Effect |
|-----------|---------|--------|
| temperature | 0.85 | 0.7=tight/clinical, 0.9=creative, 0.95=chaotic |
| cfg_scale | 2.0 | Internal LM guidance, higher = stricter adherence |
| top_p | 0.9 | Nucleus sampling |
| thinking | true | Enable chain-of-thought planning |

## Ending Strategy

Tracks tend to end abruptly. Always:
1. Add 10-15 extra seconds beyond content duration
2. Include fade-out language in caption
3. Give `[Outro]` section multiple `...` lines
4. Repeat a fading phrase in lyrics
