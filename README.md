# Videoskill

All-in-one Claude Code Skill for cinematic video production with Remotion.

## What it does

A single comprehensive skill (`/videoskill`) that orchestrates the complete video production pipeline:

- **Remotion Engine** -- Compositions, spring animations, transitions, sequences
- **AI Image Generation** -- nanobanana-pro (Gemini) with style consistency
- **Lottie Animations** -- Search and embed professional animations
- **Premium Typography** -- @remotion/google-fonts with curated selections
- **Shapes & Paths** -- Procedural SVG, path morphing, draw-on effects
- **Motion Blur & Noise** -- Cinematic blur, Perlin noise organic motion
- **TikTok Captions** -- Word-by-word highlighted subtitles
- **Audio Visualization** -- Frequency bars, waveforms
- **Icons** -- Lucide + Iconify (200k+ icons)
- **Stock Assets** -- Pexels, Pixabay, Unsplash, picsum.photos
- **UI Screens** -- Google Stitch for app walkthrough videos
- **Design Systems** -- Premium color palettes, typography rules, anti-patterns
- **Multi-Agent** -- Parallel scene building for faster production
- **Multi-Format** -- YouTube, Instagram Reel, TikTok, Story, Thumbnail

## MCP Servers Used

| MCP | Purpose |
|-----|---------|
| nanobanana-pro | AI image generation (Gemini) |
| lottiefiles | Lottie animation search |
| lucide | Line icon search |
| iconify | 200k+ icons from 150+ sets |
| remotion-docs | Remotion API documentation |
| stitch | Google Stitch UI screen generation |
| magic (21st.dev) | UI component inspiration |
| shadcn | shadcn/ui components |
| magic-ui | Magic UI components |
| aceternity | Aceternity UI components |
| reactbits | ReactBits components |
| gsap | GSAP animation patterns |
| context7 | Library documentation |
| tailwind-docs | Tailwind CSS docs |
| heroui | HeroUI components |

## Installation

Copy `.claude/skills/videoskill/` into your `~/.claude/skills/` directory:

```bash
cp -r .claude/skills/videoskill ~/.claude/skills/
```

## Usage

```
/videoskill 30-second product launch video, dark cinematic theme, 16:9
/videoskill Instagram Reel for a coffee brand, warm tones, 15 seconds
/videoskill TikTok tutorial with TikTok-style captions, tech theme
```
