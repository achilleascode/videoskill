---
name: video-batch
description: Batch video production pipeline. Takes multiple scripts (JSON/CSV) and produces videos in parallel using the videoskill. Generates both 16:9 (landscape) and 9:16 (portrait) formats per script. Handles voiceover generation, background images, and rendering automatically.
argument-hint: "[path to scripts.json or inline script list]"
---

# Video Batch Production Pipeline

Produce videos in batch for: **$ARGUMENTS**

## Input Format

### scripts.json
```json
[
  {
    "id": "vid-001",
    "title": "E-Rechnung Pflicht 2027",
    "voice": "CLrOIc4387Te6zgQGxeh",
    "scenes": [
      {
        "text_on_screen": "E-Rechnung wird Pflicht",
        "voiceover": "Die E-Rechnung wird ab zwanzig siebenundzwanzig in Deutschland zur Pflicht.",
        "bg_prompt": "Professional dark navy gradient, soft bokeh lights, premium corporate"
      },
      {
        "text_on_screen": "Was ist eine E-Rechnung?",
        "voiceover": "Eine E-Rechnung ist ein strukturiertes XML-Format.",
        "bg_prompt": "Minimal dark background with subtle geometric lines, midnight blue"
      }
    ],
    "schedule": {
      "youtube": "2026-03-28T09:00:00Z",
      "instagram": "2026-03-28T12:00:00Z",
      "tiktok": "2026-03-28T15:00:00Z"
    }
  }
]
```

### Inline (Quick Mode)
```
/video-batch 5 videos about E-Rechnung topics: 1) Pflicht ab 2027, 2) ZUGFeRD erklärt,
3) XRechnung vs ZUGFeRD, 4) E-Rechnung empfangen, 5) Software-Auswahl
```

## Production Pipeline

### Phase 1: Script Processing
1. Parse input (JSON file or natural language)
2. For each script, generate scene breakdown if not provided
3. Validate: all German text has correct Umlaute, factual accuracy

### Phase 2: Asset Generation (Parallel)
Launch parallel agents — one per video:

```
For each video:
  Agent A (assets):
    1. Set nanobanana aspect_ratio to 16:9, conversation_id per video
    2. Generate background images (1 per scene)
    3. Generate voiceover clips per scene via ElevenLabs API
    4. Measure audio durations with ffprobe

  Agent B (code):
    1. Create Remotion composition with scenes
    2. Set scene durations = audio duration + 40 frame buffer
    3. Wire TransitionSeries with fade() transitions (25 frames each)
    4. Add per-scene Audio components
    5. Add LogoWatermark overlay
```

### Phase 3: Rendering (Parallel)
For each video, render both formats:
```bash
# Landscape (YouTube, LinkedIn, Twitter, Facebook)
npx remotion render src/index.ts VideoID-Landscape out/vid-001-landscape.mp4 --crf 18

# Portrait (Instagram Reel, TikTok)
npx remotion render src/index.ts VideoID-Portrait out/vid-001-portrait.mp4 --crf 18
```

### Phase 4: Output
Save all rendered videos to `out/` folder with naming convention:
```
out/
  vid-001-landscape.mp4   (1920x1080)
  vid-001-portrait.mp4    (1080x1920)
  vid-001-thumb.png       (1280x720)
  vid-002-landscape.mp4
  vid-002-portrait.mp4
  vid-002-thumb.png
  ...
```

### Phase 5: Upload to Storage (Optional)
Upload to Supabase Storage for public URLs (needed for Instagram API):
```bash
# Via Supabase CLI or API
supabase storage cp out/vid-001-landscape.mp4 sb://videos/vid-001-landscape.mp4
```

## Critical Rules (from videoskill D3-D7)
- Transitions: fade() only, max 30 frames
- NO manual fadeOut in scene components
- Per-scene voiceover MP3s, NOT one combined file
- Scene duration = audio + transition + 15 frame buffer
- BGM: constant 2-4% volume, no fade
- Backgrounds: photography-style prompts, no AI slop
- ElevenLabs: spell out numbers ("zwanzig siebenundzwanzig" not "2027")

## Parallel Agent Strategy
- Up to 5 video agents in parallel (avoid overloading system)
- Each agent creates its own subfolder: `src/videos/vid-001/`
- Shared assets (logo, BGM) in `public/assets/shared/`
- After all agents complete, batch render sequentially

## Voice Reference (German)
| Voice ID | Name | Style |
|----------|------|-------|
| `CLrOIc4387Te6zgQGxeh` | Markus | confident, advertisement |
| `MbbPUteESkJWr4IAaW35` | Felix | professional, corporate |
| `zKHQdbB8oaQ7roNTiDTK` | Laura | professional, female |
| `aTTiK3YzK3dXETpuDE2h` | Ben | confident, young |
