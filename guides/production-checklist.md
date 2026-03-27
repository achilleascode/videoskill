# Video Production Checklist

Use this checklist before rendering any video.

## Pre-Production
- [ ] Script written with correct German (Umlaute: ue, oe, ae, ss)
- [ ] Scene breakdown defined (title, voiceover text, background prompt)
- [ ] Voice selected (Markus for ads, Felix for corporate, Laura for customer-facing)
- [ ] Numbers spelled out in voiceover scripts ("zwanzig siebenundzwanzig" not "2027")
- [ ] Brand assets available (logo, colors, fonts)

## Asset Generation
- [ ] Background images generated with photography-style prompts (no AI slop)
- [ ] All backgrounds use same `conversation_id` + `use_image_history: true`
- [ ] Voiceover clips generated per scene (separate MP3 per scene)
- [ ] Audio durations measured with `ffprobe`
- [ ] Background music sourced (royalty-free, pre-cut to video length)

## Scene Development
- [ ] Every element uses blur-to-sharp + translate entrance (no pop-in)
- [ ] Animation delays synced to voiceover timestamps (frame = seconds * 30)
- [ ] No manual `opacity: fadeOut` wrapper on root AbsoluteFill
- [ ] Cards have: top accent bar, inner glow, blur-to-sharp entrance
- [ ] Header: `top: 130`, centered, 60px Outfit bold
- [ ] Ken Burns: subtle zoom (1.04-1.06 max, NOT 1.15)
- [ ] Ambient particles: < 12 count, opacity < 0.25
- [ ] No text in top-right 220x90px zone (logo watermark area)

## Timing & Sync
- [ ] Scene duration = audio duration + transition + 15 frame buffer
- [ ] Total duration math verified: sum(scenes) - sum(transitions) = target
- [ ] Transitions: only `fade()`, max 30 frames (1 second)
- [ ] BGM: constant 2-4% volume, no fade in/out
- [ ] Per-scene Audio component inside each scene (not one combined file)

## Visual Review
- [ ] Preview in Remotion Studio (npm run dev)
- [ ] Check all 5 scenes play through without glitches
- [ ] Verify text is readable (min 28px at 1080p)
- [ ] Verify no overlapping text/elements
- [ ] Logo watermark visible but not obtrusive
- [ ] Transitions smooth (no flash, no double-fade)

## Render & Deliver
- [ ] Render landscape (1920x1080) for YouTube/LinkedIn/Twitter/Facebook
- [ ] Render portrait (1080x1920) for Instagram/TikTok (if needed)
- [ ] Generate thumbnail still
- [ ] Upload to Google Drive for review
- [ ] Get approval before publishing
