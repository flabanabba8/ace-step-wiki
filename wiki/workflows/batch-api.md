---
title: REST API Batch Automation
type: workflow
tldr: "Automate generation via REST API"
related:
  - "[[rest-api]]"
  - "[[task-types]]"
created: 2026-05-17
updated: 2026-05-17
confidence: high
---

# REST API Batch Automation

ACE-Step runs a REST API server on port 8001 for programmatic generation.

## Start the API Server

```bash
uv run acestep-api
```

## Key Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `POST /release_task` | POST | Submit generation task, returns task_id |
| `POST /query_result` | POST | Batch query task status |
| `GET /v1/audio?path=...` | GET | Download generated audio |
| `POST /format_input` | POST | LLM-enhanced caption/lyrics formatting |
| `GET /v1/models` | GET | List available DiT models |
| `POST /v1/init` | POST | Initialize/switch models (up to 3 slots) |
| `GET /v1/stats` | GET | Server runtime stats |
| `GET /health` | GET | Health check |

## Task Status Codes

| Code | Meaning |
|------|---------|
| 0 | Queued |
| 1 | Success |
| 2 | Failed |

## Example: Batch Generation

```python
import requests, json

API = "http://localhost:8001"

def generate(caption, lyrics, duration=120, bpm=120, key="D minor"):
    resp = requests.post(f"{API}/release_task", json={
        "task_type": "text2music",
        "caption": caption,
        "lyrics": lyrics,
        "duration": duration,
        "bpm": bpm,
        "keyscale": key,
        "thinking": True,
    })
    return resp.json()["task_id"]

def check(task_ids):
    resp = requests.post(f"{API}/query_result", json={"task_ids": task_ids})
    return resp.json()

# Queue 5 variations
ids = []
for i in range(5):
    tid = generate(f"Industrial jingle variation {i}", "[Instrumental]\n...\n...")
    ids.append(tid)

# Poll until done
import time
while True:
    results = check(ids)
    done = all(r["status"] == 1 for r in results["results"])
    if done:
        break
    time.sleep(5)
```

## Authentication

Optional. Set `ACESTEP_API_KEY` in `.env`, then pass as:
- Request body: `"ai_token": "your-key"`
- Header: `Authorization: Bearer your-key`

## Multi-Model Support

Load up to 3 DiT models simultaneously:

```bash
ACESTEP_CONFIG_PATH=acestep-v15-xl-turbo
ACESTEP_CONFIG_PATH2=acestep-v15-xl-base
```

Select per-request with `"model": "acestep-v15-xl-base"`.

## Output Formats

`flac`, `mp3`, `opus`, `aac`, `wav`, `wav32`
