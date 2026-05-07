---
name: elevenlabs-music-finite
description: Generate short songs, jingles, loops, and instrumental music with the Finite-managed ElevenLabs Music API key
version: 1.0.0
metadata:
  hermes:
    tags: [audio, music, elevenlabs, generation, finite]
    related_skills: []
---

# ElevenLabs Music

Use this skill when the user asks to create music, a song, a jingle, a theme,
a loop, a backing track, or an instrumental bed.

Do not use text-to-speech for music requests. Use the ElevenLabs Music endpoint
and save the generated audio as an MP3 file in the current project or a clear
output directory.

## Requirements

- `ELEVENLABS_API_KEY` must be available in the environment or in
  `/home/node/.hermes/.env`.
- `curl` and `jq` should be available.

If the key is missing, explain that music generation is not configured for this
machine.

## Quick Generate

Use this shell pattern. Keep prompts in English for best results.

```bash
set -euo pipefail
set -a
[ -f /home/node/.hermes/.env ] && . /home/node/.hermes/.env
set +a

: "${ELEVENLABS_API_KEY:?ELEVENLABS_API_KEY is not configured}"

prompt='A short upbeat instrumental jingle for an AI agent workshop, warm synths, no vocals'
length_ms=15000
output='elevenlabs-music.mp3'

curl -fsS -X POST "https://api.elevenlabs.io/v1/music" \
  -H "xi-api-key: ${ELEVENLABS_API_KEY}" \
  -H "Content-Type: application/json" \
  -H "Accept: audio/mpeg" \
  --data "$(jq -cn \
    --arg prompt "$prompt" \
    --argjson length_ms "$length_ms" \
    '{prompt: $prompt, music_length_ms: $length_ms, force_instrumental: true}')" \
  -o "$output"

file "$output"
```

## Guidance

- Ask one short clarifying question only if the prompt is too vague to choose a
  useful musical direction.
- Default to `force_instrumental: true` unless the user explicitly asks for
  vocals or lyrics.
- Prefer short first drafts: 10-30 seconds. Longer generations cost more and
  are slower.
- Use a descriptive filename, for example
  `agent-camp-jingle.mp3` or `product-demo-loop.mp3`.
- After generation, tell the user the file path and attach the file if the
  current chat platform supports native attachments.
- If the user wants revisions, generate a new file rather than overwriting the
  previous one unless they ask to replace it.

## Optional Planning

For more structured songs, first create a composition plan:

```bash
curl -fsS -X POST "https://api.elevenlabs.io/v1/music/plan" \
  -H "xi-api-key: ${ELEVENLABS_API_KEY}" \
  -H "Content-Type: application/json" \
  --data "$(jq -cn --arg prompt "$prompt" --argjson length_ms "$length_ms" \
    '{prompt: $prompt, music_length_ms: $length_ms}')" \
  > elevenlabs-music-plan.json
```

Then pass the plan to `/v1/music` as `composition_plan` instead of `prompt`.
