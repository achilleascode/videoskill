---
name: videoskill
description: All-in-one Remotion video production engine. Orchestrates 15+ MCP servers (nanobanana AI images, Lottie, Stitch UI, shadcn, magic-ui, aceternity, GSAP, Lucide, Iconify) with premium design systems (taste/soft/brutalist/minimalist) for cinematic video compositions. Handles the complete pipeline from asset generation through scene composition to final render. Use when creating any video, animation, or motion graphic.
argument-hint: "[video description, style, duration, format]"
---

# VIDEOSKILL — Complete Video Production Engine

Create a cinematic video for: **$ARGUMENTS**

---

# PART A: REMOTION CORE ENGINE

## A1. Project Setup

### Required Dependencies
```bash
# Core
npm i --save-exact remotion@latest @remotion/cli@latest react react-dom

# Premium packages (install as needed)
npm i @remotion/google-fonts        # Load any Google Font
npm i @remotion/transitions         # Slide, fade, wipe, flip between scenes
npm i @remotion/lottie lottie-web   # After Effects animations
npm i @remotion/shapes              # Procedural SVG shapes
npm i @remotion/paths               # SVG path animations & morphing
npm i @remotion/noise               # Perlin noise for organic motion
npm i @remotion/motion-blur         # Cinematic blur on moving elements
npm i @remotion/animated-emoji      # Apple-quality animated emoji
npm i @remotion/media-utils         # Audio visualization
npm i @remotion/captions            # TikTok-style word-by-word captions
npm i @remotion/gif                 # GIF support
npm i @remotion/three @react-three/fiber three  # 3D scenes
```

### Folder Structure
```
src/
  index.ts              # registerRoot entry point
  Root.tsx               # All <Composition> definitions
  compositions/          # Scene components
  components/            # Reusable animated elements
  lib/                   # Fonts, colors, easing configs
  assets/                # Local images, audio, lottie JSON
public/                  # Static assets (staticFile() references)
```

### Entry Point (src/index.ts)
```ts
import { registerRoot } from "remotion";
import { RemotionRoot } from "./Root";
registerRoot(RemotionRoot);
```

## A2. Composition Formats

```tsx
import { Composition } from "remotion";

export const RemotionRoot = () => (
  <>
    <Composition id="YouTube" component={Video} durationInFrames={900} fps={30} width={1920} height={1080}
      defaultProps={{ theme: "dark", accent: "#3B82F6" }} />
    <Composition id="InstagramReel" component={Video} durationInFrames={450} fps={30} width={1080} height={1920}
      defaultProps={{ theme: "dark", accent: "#3B82F6" }} />
    <Composition id="TikTok" component={Video} durationInFrames={450} fps={30} width={1080} height={1920}
      defaultProps={{ theme: "dark", accent: "#3B82F6" }} />
    <Composition id="InstagramPost" component={Video} durationInFrames={450} fps={30} width={1080} height={1080}
      defaultProps={{ theme: "dark", accent: "#3B82F6" }} />
    <Composition id="Story" component={Video} durationInFrames={150} fps={30} width={1080} height={1920}
      defaultProps={{ theme: "dark", accent: "#3B82F6" }} />
    <Composition id="Thumbnail" component={Video} durationInFrames={1} fps={30} width={1280} height={720}
      defaultProps={{ theme: "dark", accent: "#3B82F6" }} />
  </>
);
```

| Format | Width | Height | Aspect | Typical Duration |
|--------|-------|--------|--------|-----------------|
| YouTube/LinkedIn | 1920 | 1080 | 16:9 | 30-120s |
| Instagram Reel/TikTok | 1080 | 1920 | 9:16 | 15-90s |
| Instagram Post | 1080 | 1080 | 1:1 | 15-60s |
| Story | 1080 | 1920 | 9:16 | 5-15s |
| Twitter/X | 1280 | 720 | 16:9 | 15-140s |

## A3. Animation Primitives

### Spring (Primary — Always use over linear)
```tsx
import { spring, useCurrentFrame, useVideoConfig } from "remotion";

const scale = spring({ frame, fps, config: { damping: 200, stiffness: 100, mass: 0.5 } });
// Delayed: spring({ frame: frame - 15, fps, ... })
// Bouncy: config: { damping: 8, stiffness: 120 }
// Smooth: config: { damping: 200 }
```

### Interpolate (Value Mapping)
```tsx
import { interpolate, interpolateColors, Easing } from "remotion";

const opacity = interpolate(frame, [0, 30], [0, 1], { extrapolateRight: "clamp" });
const y = interpolate(frame, [0, 20], [50, 0], { extrapolateRight: "clamp", easing: Easing.bezier(0.25, 0.1, 0.25, 1) });
const color = interpolateColors(frame, [0, 30], ["#0a0a0a", "#3B82F6"]);
```

### Sequence & Series
```tsx
import { Sequence, Series } from "remotion";

// Auto-chaining (no manual frame math)
<Series>
  <Series.Sequence durationInFrames={90}><IntroScene /></Series.Sequence>
  <Series.Sequence durationInFrames={120}><ContentScene /></Series.Sequence>
  <Series.Sequence durationInFrames={60} offset={-10}><OutroScene /></Series.Sequence>
</Series>
```

### TransitionSeries (Smooth scene changes)
**IMPORTANT:** Only use `fade()` for enterprise videos. See D3 for critical rules on audio overlap,
manual fadeOut conflicts, and duration math.
```tsx
import { TransitionSeries, springTiming } from "@remotion/transitions";
import { fade } from "@remotion/transitions/fade";

// Each scene has per-scene <Audio> inside. Scene durations include tail buffer
// so voiceover finishes before transition starts (see D4).
// NEVER put fadeOut opacity wrappers inside scene components.
<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={200}><SceneA /></TransitionSeries.Sequence>
  <TransitionSeries.Transition
    presentation={fade()}
    timing={springTiming({ config: { damping: 200 }, durationInFrames: 25 })}
  />
  <TransitionSeries.Sequence durationInFrames={290}><SceneB /></TransitionSeries.Sequence>
  <TransitionSeries.Transition
    presentation={fade()}
    timing={springTiming({ config: { damping: 200 }, durationInFrames: 25 })}
  />
  <TransitionSeries.Sequence durationInFrames={180}><SceneC /></TransitionSeries.Sequence>
</TransitionSeries>
```

### AbsoluteFill (Layer Stack)
```tsx
import { AbsoluteFill } from "remotion";

<AbsoluteFill style={{ backgroundColor: "#0a0a0a" }}>
  <AbsoluteFill><BackgroundLayer /></AbsoluteFill>
  <AbsoluteFill><ContentLayer /></AbsoluteFill>
  <AbsoluteFill style={{ pointerEvents: "none" }}><OverlayLayer /></AbsoluteFill>
</AbsoluteFill>
```

## A4. Media Components

```tsx
import { Img, OffthreadVideo, Audio, staticFile } from "remotion";

// Images (local or remote)
<Img src={staticFile("hero.png")} style={{ width: "100%", height: "100%", objectFit: "cover" }} />
<Img src="https://picsum.photos/seed/vid1/1920/1080" />

// Video (use OffthreadVideo for long clips — better memory)
<OffthreadVideo src={staticFile("bg.mp4")} startFrom={30} endAt={150} volume={0.3}
  style={{ objectFit: "cover", width: "100%", height: "100%" }} />

// Audio with volume envelope (fade in/out)
<Audio src={staticFile("music.mp3")}
  volume={(f) => interpolate(f, [0, 15, dur - 30, dur], [0, 0.8, 0.8, 0], {
    extrapolateLeft: "clamp", extrapolateRight: "clamp",
  })} />
```

## A5. Premium Packages

### Google Fonts
```tsx
import { loadFont } from "@remotion/google-fonts/Outfit";
const { fontFamily } = loadFont();
// Premium: Outfit, Sora, SpaceGrotesk, PlusJakartaSans, DMSans
// Editorial: PlayfairDisplay, Fraunces, InstrumentSerif
// Mono: JetBrainsMono, FiraCode, SpaceMono
```

### Noise (Organic Motion)
```tsx
import { noise2D } from "@remotion/noise";
const x = noise2D("seedX", frame * 0.02, 0) * 50;   // wobble
const y = noise2D("seedY", 0, frame * 0.02) * 30;
const rotation = noise2D("rot", frame * 0.01, 0.5) * 10;
```

### Shapes
```tsx
import { Circle, Star, Triangle, Polygon, Pie } from "@remotion/shapes";
<Circle radius={100} fill="#3B82F6" />
<Star innerRadius={40} outerRadius={80} points={5} fill="#F59E0B" />
<Pie radius={60} progress={0.75} fill="#10B981" />
```

### Path Animations
```tsx
import { evolvePath, interpolatePath, getPointAtLength } from "@remotion/paths";
const evolved = evolvePath(progress, svgPath);  // Draw-on effect
const morphed = interpolatePath(progress, [0, 1], [pathA, pathB]);  // Morph
const point = getPointAtLength(path, progress * length);  // Follow path
```

### Motion Blur
```tsx
import { CameraMotionBlur, Trail } from "@remotion/motion-blur";
<CameraMotionBlur samples={10} shutterAngle={180}><FastElement /></CameraMotionBlur>
<Trail layers={6} trailOpacity={0.3} lagInFrames={2}><MovingLogo /></Trail>
```

### Lottie Animations
```tsx
import { Lottie } from "@remotion/lottie";
import animationData from "./assets/confetti.json";
<Lottie animationData={animationData} style={{ width: 400, height: 400 }} />
```

### TikTok Captions
```tsx
import { parseSrt, createTikTokStyleCaptions } from "@remotion/captions";
const { pages } = createTikTokStyleCaptions({ captions: parseSrt({ input: srtText }), combineTokensWithinMilliseconds: 800 });
// Render: highlight active word per token.fromMs / token.toMs
```

### Audio Visualization
```tsx
import { useAudioData, visualizeAudio } from "@remotion/media-utils";
const audioData = useAudioData(src);
const bars = visualizeAudio({ fps, frame, audioData, numberOfSamples: 64 });
// bars is number[] — map to bar heights
```

## A6. Quick Reference

| Task | Code |
|------|------|
| Fade in | `opacity: interpolate(frame, [0, 20], [0, 1], { extrapolateRight: "clamp" })` |
| Slide up | `translateY: interpolate(frame, [0, 20], [40, 0], { extrapolateRight: "clamp" })` |
| Scale bounce | `scale: spring({ frame, fps, config: { damping: 12 } })` |
| Smooth scale | `scale: spring({ frame, fps, config: { damping: 200 } })` |
| Delay | `spring({ frame: frame - 15, fps, ... })` |
| Stagger | `spring({ frame: frame - index * 5, fps, ... })` |
| Ken Burns | `scale: interpolate(frame, [0, dur], [1, 1.15])` |
| Typewriter | `text.slice(0, Math.floor(interpolate(frame, [0, dur], [0, text.length])))` |
| Fade out | `opacity: interpolate(frame, [dur-20, dur], [1, 0], { extrapolateLeft: "clamp" })` |
| Pulse | `scale: 1 + Math.sin(frame * 0.1) * 0.05` |
| Color flash | `interpolateColors(frame, [0, 10, 20], ["#0a0a0a", "#3B82F6", "#0a0a0a"])` |
| Noise wobble | `rotation: noise2D("r", frame * 0.02, 0) * 5` |

## A7. Rendering

```bash
npx remotion render src/index.ts YouTube out/video.mp4              # Default H.264
npx remotion render src/index.ts YouTube out/video.mp4 --crf 18     # High quality
npx remotion render src/index.ts InstagramReel out/reel.mp4         # 9:16
npx remotion render src/index.ts YouTube out/video.mov --codec prores  # ProRes
npx remotion render src/index.ts YouTube out/anim.gif --codec gif   # GIF
npx remotion still src/index.ts YouTube out/thumb.png --frame=45    # Thumbnail
```

---

# PART B: MCP ASSET PIPELINE

## B1. AI Image Generation — nanobanana-pro (Gemini)

**This is your primary creative asset pipeline.** Generates custom images for any scene.

### Tools Reference
| Tool | Purpose |
|------|---------|
| `mcp__nanobanana-pro__set_aspect_ratio` | Set ratio before generating (16:9, 9:16, 1:1, etc.) |
| `mcp__nanobanana-pro__set_model` | Switch to `flash` (fast) or `pro` (quality) |
| `mcp__nanobanana-pro__gemini_generate_image` | Generate image from text prompt |
| `mcp__nanobanana-pro__gemini_edit_image` | Edit existing image (supports `last`, `history:N`) |
| `mcp__nanobanana-pro__get_image_history` | List all generated images in session |
| `mcp__nanobanana-pro__clear_conversation` | Reset session |
| `mcp__nanobanana-pro__gemini_chat` | Chat with Gemini (image analysis, captions) |

### Workflow: Generate Video Assets
```
# Step 1: Set aspect ratio matching video format
mcp__nanobanana-pro__set_aspect_ratio({ aspect_ratio: "16:9", conversation_id: "video-1" })

# Step 2: Set quality model
mcp__nanobanana-pro__set_model({ model: "pro", conversation_id: "video-1" })

# Step 3: Generate hero background
mcp__nanobanana-pro__gemini_generate_image({
  prompt: "Cinematic wide shot, abstract digital landscape with flowing data streams,
           deep navy and electric blue tones, volumetric lighting, no text, 8K",
  aspect_ratio: "16:9",
  output_path: "/path/to/project/public/hero-bg.png",
  conversation_id: "video-1"
})

# Step 4: Generate consistent series (style memory)
mcp__nanobanana-pro__gemini_generate_image({
  prompt: "Same style. Close-up of hands typing with holographic UI floating above",
  conversation_id: "video-1",
  use_image_history: true  // Maintains visual consistency!
})

# Step 5: Edit/refine
mcp__nanobanana-pro__gemini_edit_image({
  image_path: "last",
  edit_prompt: "Add subtle lens flare top-right, increase blue tones",
  conversation_id: "video-1"
})

# Step 6: Get AI-generated caption/description
mcp__nanobanana-pro__gemini_chat({
  message: "Describe this image in one cinematic sentence for a video voiceover",
  images: ["last"],
  conversation_id: "video-1"
})
```

### Aspect Ratio Mapping
| Video Format | Nanobanana Ratio | Remotion Size |
|-------------|-----------------|---------------|
| YouTube | `16:9` | 1920x1080 |
| Reel/TikTok | `9:16` | 1080x1920 |
| Instagram Post | `1:1` | 1080x1080 |
| Ultrawide | `21:9` | 2560x1080 |

### Pro Tips
- Use `conversation_id` consistently for visual coherence across all generated images
- `use_image_history: true` ensures the AI remembers previous generations (colors, style)
- `reference_images` array accepts local file paths for style matching
- `enable_google_search: true` for real-world reference grounding (product photos, landmarks)
- Save to `public/` folder for `staticFile()` access in Remotion
- Generate at highest quality, Remotion compresses during render

## B2. Lottie Animations — lottiefiles MCP

Search professional animations for transitions, celebrations, UI elements:
```
mcp__lottiefiles__search_animations({ query: "confetti celebration" })
mcp__lottiefiles__search_animations({ query: "loading spinner minimal" })
mcp__lottiefiles__search_animations({ query: "data chart animate" })
mcp__lottiefiles__search_animations({ query: "logo reveal" })
mcp__lottiefiles__search_animations({ query: "social media icons" })
mcp__lottiefiles__get_popular_animations({})
mcp__lottiefiles__get_animation_details({ id: "animation-id" })
```
Download JSON, save to `src/assets/`, use with `@remotion/lottie`.

## B3. Icons — Lucide + Iconify MCPs

### Lucide (Clean line icons)
```
mcp__lucide__search_icons({ query: "arrow right" })
mcp__lucide__fuzzy_search_icons({ query: "play" })
mcp__lucide__get_icon_usage_examples({ iconName: "arrow-right" })
mcp__lucide__list_all_categories({})
```

### Iconify (200,000+ icons from 150+ sets)
```
mcp__iconify__search_icons({ query: "play button", limit: 10 })
mcp__iconify__get_icon({ prefix: "ph", name: "play-fill" })  // Phosphor
mcp__iconify__get_icon({ prefix: "ri", name: "movie-2-line" })  // Remix
mcp__iconify__get_all_icon_sets({})
```
Render as inline SVG in Remotion for crisp scaling at any resolution.

## B4. Stock Assets (No API Key Needed)

```
// Reliable placeholders
https://picsum.photos/seed/{unique-id}/1920/1080      // Random with seed
https://picsum.photos/id/{number}/1920/1080            // Specific photo
https://picsum.photos/1920/1080?grayscale&blur=2       // With effects
```

**API-based (free keys):**
- **Pexels** (`api.pexels.com`): Free HD/4K photos + videos. 200 req/hr. No attribution needed.
- **Pixabay** (`pixabay.com/api`): 4.5M+ photos + videos. Free commercial use.
- **Unsplash** (`api.unsplash.com`): 3M+ photos. Attribution required.

Use `calculateMetadata` to fetch stock URLs before render:
```tsx
calculateMetadata={async ({ props }) => {
  const res = await fetch(`https://api.pexels.com/v1/search?query=${props.topic}&per_page=5`, {
    headers: { Authorization: process.env.PEXELS_API_KEY }
  });
  const data = await res.json();
  return { props: { ...props, images: data.photos.map(p => p.src.large2x) } };
}}
```

## B5. Google Stitch — UI Screen Generation

Generate polished UI screens to showcase in videos:
```
# Official MCP: stitch (if connected)
# Tools: build_site, get_screen_code, get_screen_image

# Workflow:
# 1. Design screens at stitch.withgoogle.com
# 2. Pull screenshots via MCP
# 3. Use as Img in Remotion with Ken Burns / zoom reveals
# 4. Add device frames, reflections, shadows
```

Official Remotion integration: `npx skills add google-labs-code/stitch-skills --skill remotion --global`

## B6. UI Component Inspiration MCPs

Query for visual patterns to recreate as video scenes:
```
mcp__magic__21st_magic_component_inspiration({ query: "hero section dark theme" })
mcp__shadcn__search_items_in_registries({ query: "card animated" })
mcp__aceternity__search_components({ query: "text reveal animation" })
mcp__reactbits__search_components({ query: "background effects" })
mcp__magic-ui__searchRegistryItems({ query: "globe" })
```

## B7. GSAP Patterns (for complex scroll-like sequences)
```
mcp__gsap__understand_and_create_animation({ prompt: "staggered text reveal with split characters" })
mcp__gsap__create_production_pattern({ prompt: "parallax image zoom on scroll" })
mcp__gsap__get_gsap_api_expert({ prompt: "timeline with labels and callbacks" })
```
Translate GSAP patterns into Remotion's frame-based system.

## B8. Remotion Documentation Lookup
```
mcp__remotion-docs__remotion-documentation({ query: "spring animation config options" })
mcp__remotion-docs__remotion-documentation({ query: "TransitionSeries fade slide" })
mcp__remotion-docs__remotion-documentation({ query: "calculateMetadata async data" })
```

## B9. ElevenLabs Voice-Over (TTS API)

Generate professional voice-overs for any video. Store API key in `.env` as `ELEVENLABS_API_KEY`.

### Generate Voice-Over via curl
```bash
curl -s "https://api.elevenlabs.io/v1/text-to-speech/{voice_id}" \
  -H "xi-api-key: $ELEVENLABS_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Your voiceover script here",
    "model_id": "eleven_multilingual_v2",
    "voice_settings": {
      "stability": 0.6,
      "similarity_boost": 0.85,
      "style": 0.4,
      "use_speaker_boost": true
    }
  }' \
  --output public/assets/voiceover.mp3
```

### Recommended German Voices
| Voice ID | Name | Style | Best for |
|----------|------|-------|----------|
| `CLrOIc4387Te6zgQGxeh` | Markus | confident, advertisement | Product videos, ads |
| `MbbPUteESkJWr4IAaW35` | Felix | professional | Explainer, corporate |
| `zKHQdbB8oaQ7roNTiDTK` | Laura | professional, female | Customer-facing |
| `aTTiK3YzK3dXETpuDE2h` | Ben | confident, young | Modern, startup |
| `6CS8keYmkwxkspesdyA7` | Ramona | professional, female | Customer care |

### Voice Settings Tuning
- `stability`: 0.3 (expressive) to 0.9 (consistent). Default 0.6
- `similarity_boost`: Higher = closer to original voice. Default 0.85
- `style`: 0.0 (neutral) to 1.0 (expressive). Default 0.4
- `model_id`: `eleven_multilingual_v2` for German, `eleven_turbo_v2_5` for fast English

### List All Voices
```bash
curl -s "https://api.elevenlabs.io/v1/voices" \
  -H "xi-api-key: $ELEVENLABS_API_KEY" | python3 -c "
import json, sys
for v in json.load(sys.stdin)['voices']:
    labels = v.get('labels', {})
    if labels.get('language','') == 'de':
        print(f\"{v['voice_id']} | {v['name']} | {labels}\")"
```

### Integration with Remotion
```tsx
// Reference the generated audio file
<Audio src={staticFile("voiceover.mp3")}
  volume={(f) => interpolate(f, [0, 10, dur - 20, dur], [0, 1, 1, 0], {
    extrapolateLeft: "clamp", extrapolateRight: "clamp",
  })} />
```

### Pro Tips
- Generate voiceover FIRST, then match video duration to audio length
- Use `ffprobe` to get exact duration: `ffprobe -v quiet -show_entries format=duration -of csv=p=0 file.mp3`
- Set Remotion `durationInFrames = Math.ceil(audioDuration * fps)`
- Add 0.5s padding at start and end for clean fade
- For scene-synced audio: generate separate MP3 per scene, combine with Sequence

---

# PART C: MOTION GRAPHICS SYSTEM

Every element that appears on screen MUST use the **blur-to-sharp + translate** entrance pattern.
No element may simply "pop" into existence. This is the core of enterprise-grade motion graphics.

## C1. The Entrance Pattern (MANDATORY for all elements)

Every animated element uses this exact pattern:
```tsx
// Spring with slow, weighty feel
const p = spring({
  frame: frame - DELAY,  // synced to voiceover
  fps,
  config: { damping: 22, stiffness: 50, mass: 1.1 },  // slow and smooth
});

// Three simultaneous properties
const translateY = interpolate(p, [0, 1], [40, 0]);    // slides up 40px
const blur = interpolate(p, [0, 1], [8, 0]);            // sharpens from 8px blur
const opacity = p;                                       // fades in naturally

// Apply all three together
<div style={{
  opacity,
  transform: `translateY(${translateY}px)`,
  filter: `blur(${blur}px)`,
}}>
```

### Direction Variants
- **From bottom (default):** `translateY: [40, 0]` — content rises into place
- **From left:** `translateX: [-60, 0]` — sweeps in from left side
- **From right:** `translateX: [60, 0]` — sweeps in from right side
- **Scale up:** Add `transform: scale(${interpolate(p, [0, 1], [0.88, 1])})` — grows into place

### Spring Config Reference
| Feel | Config | Use for |
|------|--------|---------|
| **Slow & elegant** | `{ damping: 22, stiffness: 50, mass: 1.1 }` | Cards, main content |
| **Smooth header** | `{ damping: 22, stiffness: 65, mass: 0.7 }` | Headlines |
| **Title sweep** | `{ damping: 18, stiffness: 80, mass: 0.8 }` | Big intro text |
| **Bouncy accent** | `{ damping: 10, stiffness: 100, mass: 0.6 }` | Dots, nodes, small elements |
| **Instant settle** | `{ damping: 200 }` | Subtle fades, backgrounds |

## C2. Scene Template (Copy this for every scene)

```tsx
import React from "react";
import {
  AbsoluteFill, Audio, Img, interpolate, spring,
  staticFile, useCurrentFrame, useVideoConfig,
} from "remotion";
import { noise2D } from "@remotion/noise";
import { COLORS } from "../lib/colors";
import { outfit } from "../lib/fonts";

export const SceneTemplate: React.FC = () => {
  const frame = useCurrentFrame();
  const { fps, durationInFrames } = useVideoConfig();

  // Layer 1: Ken Burns background (ALWAYS slow zoom 1.04-1.06 → 1.0)
  const bgScale = interpolate(frame, [0, durationInFrames], [1.05, 1.0], {
    extrapolateRight: "clamp",
  });

  // Layer 2: Header entrance
  const headerP = spring({ frame: frame - 5, fps,
    config: { damping: 22, stiffness: 65, mass: 0.7 } });
  const headerY = interpolate(headerP, [0, 1], [-25, 0]);

  return (
    // NEVER wrap in opacity:fadeOut — TransitionSeries handles it
    <AbsoluteFill>
      {/* Per-scene audio */}
      <Audio src={staticFile("assets/vo-sceneN.mp3")} volume={1} />

      {/* Background + Ken Burns */}
      <AbsoluteFill>
        <Img src={staticFile("assets/bg-sceneN.png")}
          style={{ width: "100%", height: "100%", objectFit: "cover",
                   transform: `scale(${bgScale})` }} />
      </AbsoluteFill>

      {/* Dark overlay */}
      <AbsoluteFill style={{ backgroundColor: COLORS.overlay }} />

      {/* Ambient particles (8-12, very subtle) */}
      <AbsoluteFill style={{ pointerEvents: "none" }}>
        {Array.from({ length: 10 }, (_, i) => (
          <div key={i} style={{
            position: "absolute",
            left: 400 + noise2D(`px${i}`, frame * 0.003, i) * 600,
            top: 250 + noise2D(`py${i}`, i, frame * 0.003) * 400,
            width: 3, height: 3, borderRadius: "50%",
            backgroundColor: COLORS.highlight,
            opacity: 0.1 + Math.abs(noise2D(`po${i}`, i, frame * 0.005)) * 0.12,
          }} />
        ))}
      </AbsoluteFill>

      {/* Header (centered top) */}
      <div style={{
        position: "absolute", top: 130, width: "100%", textAlign: "center",
        opacity: headerP, transform: `translateY(${headerY}px)`,
      }}>
        <span style={{
          fontFamily: outfit.fontFamily, fontWeight: 800, fontSize: 60,
          color: COLORS.white, letterSpacing: "-0.02em",
        }}>
          Scene Title Here
        </span>
      </div>

      {/* Content: Cards in flex-row (see C3) */}
      <AbsoluteFill style={{
        display: "flex", flexDirection: "row", alignItems: "center",
        justifyContent: "center", gap: 40, paddingTop: 60,
      }}>
        {/* Cards go here */}
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```

## C3. Glassmorphism Card (Enterprise Standard)

Every info scene uses these cards. NEVER use bullet lists or plain text on background.

```tsx
// Card with entrance animation
const cardP = spring({
  frame: frame - DELAY_SYNCED_TO_VOICE, fps,
  config: { damping: 22, stiffness: 50, mass: 1.1 },
});
const cardY = interpolate(cardP, [0, 1], [40, 0]);
const cardBlur = interpolate(cardP, [0, 1], [8, 0]);

<div style={{
  // Size
  width: 460, padding: "44px",
  // Glassmorphism
  backgroundColor: "rgba(255,255,255,0.04)",
  border: "1px solid rgba(255,255,255,0.08)",
  borderRadius: 20,
  backdropFilter: "blur(8px)",
  // Entrance
  opacity: cardP,
  transform: `translateY(${cardY}px)`,
  filter: `blur(${cardBlur}px)`,
  // For accent bar + glow
  position: "relative", overflow: "hidden",
}}>
  {/* Top accent bar — scales from left */}
  <div style={{
    position: "absolute", top: 0, left: 0, right: 0, height: 3,
    backgroundColor: accentColor,
    transform: `scaleX(${cardP})`, transformOrigin: "left",
    boxShadow: `0 0 12px ${accentColor}40`,
  }} />

  {/* Number badge (optional) */}
  <div style={{
    fontFamily: jetbrainsMono.fontFamily, fontSize: 52, fontWeight: 700,
    color: accentColor, opacity: 0.3, lineHeight: 1,
  }}>01</div>

  {/* Title */}
  <div style={{
    fontFamily: outfit.fontFamily, fontSize: 38, fontWeight: 700,
    color: COLORS.white, lineHeight: 1.15, marginTop: 14,
  }}>Card Title</div>

  {/* Description */}
  <div style={{
    fontFamily: outfit.fontFamily, fontSize: 26, fontWeight: 400,
    color: COLORS.whiteSecondary, marginTop: 10, lineHeight: 1.4,
  }}>Description text here</div>

  {/* Bottom progress bar (optional) */}
  <div style={{
    position: "absolute", bottom: 0, left: 0,
    width: `${interpolate(cardP, [0.5, 1], [0, 100], {
      extrapolateLeft: "clamp", extrapolateRight: "clamp"
    })}%`,
    height: 2, backgroundColor: accentColor, opacity: 0.5,
  }} />

  {/* Inner glow (subtle) */}
  <div style={{
    position: "absolute", bottom: -30, right: -30,
    width: 160, height: 160, borderRadius: "50%",
    background: `radial-gradient(circle, ${accentColor}08 0%, transparent 70%)`,
  }} />
</div>
```

## C4. Accent Effects (Add life to scenes)

### Pulsing Glow (behind key elements)
```tsx
const glowPulse = 0.15 + Math.sin(frame * 0.08) * 0.08;
<div style={{
  position: "absolute", width: 500, height: 200, borderRadius: "50%",
  background: `radial-gradient(ellipse, ${COLORS.teal}${hex(glowPulse)} 0%, transparent 70%)`,
  filter: "blur(40px)",
}} />
```

### Floating Mockup
```tsx
const floatY = Math.sin(frame * 0.04) * 3;  // amplitude 3px, slow
<Img src={...} style={{
  transform: `translateY(${floatY}px)`,
  filter: "drop-shadow(0 24px 48px rgba(0,0,0,0.5))",
}} />
```

### Dot with Pulse Ring (for bullet points)
```tsx
const dotScale = spring({ frame: frame - delay, fps,
  config: { damping: 8, stiffness: 120 } });
<div style={{ position: "relative" }}>
  <div style={{
    width: 14, height: 14, borderRadius: "50%",
    backgroundColor: COLORS.teal,
    transform: `scale(${dotScale})`,
    boxShadow: `0 0 16px ${COLORS.teal}50`,
  }} />
  {/* Expanding ring */}
  <div style={{
    position: "absolute", inset: -6, borderRadius: "50%",
    border: `2px solid ${COLORS.teal}`,
    opacity: interpolate(dotScale, [0.8, 1.3], [0.5, 0], clampBoth),
    transform: `scale(${interpolate(dotScale, [0.8, 1.3], [1, 1.8], clampBoth)})`,
  }} />
</div>
```

### Traveling Light Particle (along lines)
```tsx
{frame > 25 && (
  <div style={{
    position: "absolute", top: lineY - 3,
    left: startX + ((frame - 25) % 90) / 90 * lineWidth,
    width: 10, height: 10, borderRadius: "50%",
    backgroundColor: COLORS.highlight,
    boxShadow: `0 0 16px ${COLORS.highlight}80, 0 0 32px ${COLORS.teal}40`,
    opacity: 0.7,
  }} />
)}
```

### Animated Checkmark (after step completion)
```tsx
{progress > 0.9 && (
  <div style={{
    width: 20, height: 20, borderRadius: "50%",
    backgroundColor: COLORS.teal,
    opacity: interpolate(progress, [0.9, 1], [0, 0.6], clampBoth),
    transform: `scale(${interpolate(progress, [0.9, 1], [0.5, 1], clampBoth)})`,
  }}>
    <svg width="12" height="12" viewBox="0 0 24 24" fill="none"
         stroke="white" strokeWidth="3" strokeLinecap="round">
      <polyline points="20 6 9 17 4 12" />
    </svg>
  </div>
)}
```

## C5. Background Decorative Elements

### Large Watermark Character (e.g. "?" behind question scenes)
```tsx
<div style={{
  position: "absolute", right: 120, top: 140,
  fontFamily: outfit.fontFamily, fontWeight: 900, fontSize: 420,
  color: COLORS.teal,
  opacity: interpolate(frame, [10, 50], [0, 0.04], clampBoth),
  transform: `translateY(${interpolate(frame, [0, dur], [0, -20])}px)`,
  pointerEvents: "none",
}}>?</div>
```

## C6. Review Checklist (Run before rendering)

Before rendering any video, verify:
- [ ] Every element uses blur-to-sharp + translate entrance (no pop-in)
- [ ] Animation delays are synced to voiceover timestamps
- [ ] No manual `opacity: fadeOut` wrapper on root AbsoluteFill
- [ ] Cards have top accent bar + inner glow
- [ ] Header uses `top: 130` centered, 60px Outfit bold
- [ ] Ken Burns is subtle (1.04-1.06 max, not 1.15)
- [ ] Ambient particles are < 12 count, opacity < 0.25
- [ ] No text in top-right 220x90px zone (logo area)
- [ ] Scene duration > audio duration + 40 frames
- [ ] Background images are photography-style, not AI slop

## C7. Google Drive Upload (Review before Publishing)

After rendering, upload videos to Google Drive for approval before publishing.

### Via Google Drive CLI (gdrive)
```bash
# Install gdrive
brew install gdrive

# Upload single video
gdrive files upload out/vid-001-landscape.mp4 --parent FOLDER_ID

# Upload entire batch
for f in out/*.mp4; do
  gdrive files upload "$f" --parent FOLDER_ID
done
```

### Via Google Drive API (Node.js)
```ts
import { google } from "googleapis";

const auth = new google.auth.GoogleAuth({
  keyFile: "service-account.json",
  scopes: ["https://www.googleapis.com/auth/drive.file"],
});
const drive = google.drive({ version: "v3", auth });

async function uploadToDrive(filePath: string, folderId: string) {
  const fs = require("fs");
  const path = require("path");
  const res = await drive.files.create({
    requestBody: {
      name: path.basename(filePath),
      parents: [folderId],
    },
    media: {
      mimeType: "video/mp4",
      body: fs.createReadStream(filePath),
    },
  });
  return `https://drive.google.com/file/d/${res.data.id}/view`;
}
```

### Via Google Drive MCP (if available)
```
mcp__claude_ai_Google_Drive__*  // Check if connected
```

### Workflow: Render → Drive → Approve → Publish
```
1. Render all videos (landscape + portrait)
2. Upload to Google Drive shared folder
3. User reviews in Drive and approves
4. User triggers /video-publish for approved videos
```

---

# PART D: DESIGN SYSTEM FOR VIDEO

## D1. Color Palettes

**Dark Cinematic** (default):
- Background: `#0a0a0a` (NOT pure black — avoids banding)
- Surface: `#141414`
- Text: `#FAFAFA` / Secondary: `rgba(255,255,255,0.6)`
- Accent: `#3B82F6` (Electric Blue) or `#10B981` (Emerald)

**Light Premium:**
- Background: `#F9FAFB` / Surface: `#FFFFFF`
- Text: `#18181B` / Secondary: `#71717A`
- Accent: `#E11D48` (Deep Rose)

**Neon/Tech:**
- Background: `#050510` / Surface: `rgba(255,255,255,0.05)`
- Glow: `#00FF88` or `#6366F1`

## D2. Typography Rules for Video
- **Minimum size:** 32px at 1080p (must read on mobile)
- **Headlines:** 72-120px, tight tracking (-0.03em), weight 700-900
- **Body:** 36-48px, leading 1.4
- **Data/Numbers:** Monospace, tabular-nums
- **Max chars per line:** 40 (viewers read fast)
- **Contrast:** Minimum 7:1 (video gets compressed)
- **Banned fonts:** System defaults. Always load via @remotion/google-fonts
- **Premium picks:** Outfit, Sora, SpaceGrotesk, PlusJakartaSans

## D3. Transitions — CRITICAL RULES

### Only use `fade()` for transitions
- `slide()`, `wipe()`, `flip()` look jarring and unprofessional in enterprise videos
- ALWAYS use `fade()` with `springTiming({ config: { damping: 200 }, durationInFrames: 25 })`
- **Maximum transition duration: 30 frames (1 second)**. Longer = sluggish
- Minimum: 20 frames. Shorter = too abrupt

### Audio overlap during transitions
TransitionSeries overlaps the END of scene N with the START of scene N+1 during the transition.
If both scenes have `<Audio>`, the voiceover clips will overlap and sound garbled.

**Fix: Add tail buffer to scene durations.** Each scene must be longer than its audio by at least
`transition_duration + 15 frames`. This creates silence at the end of each scene so audio finishes
before the transition begins crossfading into the next scene's audio.

```
Scene duration = ceil(audio_duration_seconds * fps) + transition_frames + 15
```

Example: Audio is 5.9s = 177 frames. Transition is 25 frames. Scene = 177 + 25 + 15 = 217 frames.

### NEVER use manual fadeOut inside scenes
TransitionSeries handles the crossfade. Adding `opacity: fadeOut` inside a scene component creates
a DOUBLE fade — the scene fades to transparent AND the transition fades. Result: the background
color/previous scene bleeds through as a flash. **Remove all manual fadeOut wrappers from scene
components.** The outermost `<AbsoluteFill>` in each scene must NOT have an opacity animation.

### Duration math
```
Total video duration = sum(all scene durations) - sum(all transition durations)
```
Always verify this calculation. Off-by-one errors cause audio desync or black frames at the end.

## D4. Audio-Visual Sync — CRITICAL RULES

### Generate voiceover FIRST, then match scene durations
1. Write the script per scene
2. Generate each scene's voiceover as a separate MP3 via ElevenLabs
3. Measure each MP3's exact duration with `ffprobe`
4. Set scene duration = audio duration + tail buffer (see D3)

### Sync animation timing to spoken words
Every animated element (bullet point, card, number) must appear at the EXACT frame when the
voiceover speaks the corresponding word. Calculate frame offsets from the audio:

```
word_spoken_at_seconds = listen to the audio and note timestamps
animation_delay = Math.round(word_spoken_at_seconds * fps)
```

**Common mistake:** Using fixed delays like `delay: 20 + i * 14` without checking what the voice
says at that moment. A bullet reading "Kein PDF" must appear when the speaker says "kein PDF",
not 2 seconds before or after.

### Per-scene audio (not one combined file)
Always place `<Audio src={staticFile("assets/vo-sceneN.mp3")} volume={1} />` INSIDE each scene
component. This ensures perfect sync regardless of scene duration changes. Never use one combined
voiceover file across all scenes — timing breaks when you adjust scene lengths.

### Background music
- Place BGM `<Audio>` in the Root component, NOT in individual scenes
- Volume: constant low value (0.02-0.04), NO fade in/out — sounds inconsistent
- Use royalty-free music. Generate with ElevenLabs Sound Effects API if needed (max 22s per clip)

### ElevenLabs number pronunciation
Years like "2027" may be spoken as "zwei null zwei sieben" or "zwanzig siebenundzwanzig".
To force correct pronunciation, write the number as words in the script:
- "2025" → "zwanzig fünfundzwanzig"
- "2027" → "zwanzig siebenundzwanzig"

## D5. Scene Layout — Enterprise Standard

### Card-based layouts (preferred)
Use glassmorphism cards for all info-heavy scenes (explanations, steps, timelines):
```tsx
<div style={{
  width: 460, padding: "44px",
  backgroundColor: "rgba(255,255,255,0.04)",
  border: "1px solid rgba(255,255,255,0.08)",
  borderRadius: 20,
  backdropFilter: "blur(8px)",
  position: "relative", overflow: "hidden",
}}>
  {/* Top accent bar */}
  <div style={{
    position: "absolute", top: 0, left: 0, right: 0, height: 3,
    backgroundColor: accentColor,
    transform: `scaleX(${progress})`, transformOrigin: "left",
  }} />
  {/* Content */}
</div>
```

### Consistent scene structure
Every scene follows the same visual hierarchy:
1. **Background:** Ken Burns image (`scale 1.04-1.08 → 1.0` over full duration)
2. **Overlay:** `COLORS.overlay` (semi-transparent dark)
3. **Ambient particles:** 8-15 noise-driven dots (optional, subtle)
4. **Header:** Centered top (top: 130), 60px Outfit bold, spring entrance
5. **Content area:** Cards or key content, centered, flex-row layout
6. **Audio:** Per-scene voiceover

### Blur-to-sharp entrance effect
Every element that enters should blur-to-sharp AND translate, not just fade:
```tsx
const p = spring({ frame: frame - delay, fps, config: { damping: 22, stiffness: 50, mass: 1.1 } });
const y = interpolate(p, [0, 1], [40, 0]);
const blur = interpolate(p, [0, 1], [8, 0]);
// Apply: opacity: p, transform: translateY(y), filter: blur(blur)
```

### Spacing rules
- Cards in a row: `gap: 40px`
- Header to content: `paddingTop: 60px` on content container
- No text in top-right 220x90px zone (logo watermark area)

## D6. Background Images — No AI Slop

### Prompt strategy for nanobanana
NEVER use prompts with "holographic", "floating documents", "glowing grids", "sci-fi elements",
or "futuristic UI". These produce cheap AI-looking backgrounds.

**DO use:** Abstract photography-style prompts:
- "Professional dark navy gradient, soft bokeh lights, warm white tones, shot on Hasselblad"
- "Minimal dark background with subtle geometric lines, soft light source upper left"
- "Dark surface with single elegant horizontal light streak, keynote presentation style"
- "Soft diagonal light rays in cool blue, shallow depth of field, luxury brand aesthetic"

All backgrounds in a video must use the same `conversation_id` and `use_image_history: true`
to maintain visual consistency.

## D7. Anti-Patterns (BANNED)

### Visual
- No `#000000` backgrounds — `#0a0a0a` minimum (banding)
- No system fonts — always @remotion/google-fonts
- No text under 28px at 1080p
- No thin weights (100-300) — compression destroys them
- No unmasked image edges — use rounded corners or drop-shadow
- No AI-looking backgrounds (holographic, floating documents, glowing grids)

### Animation
- No linear easing — always spring or bezier
- No manual fadeOut wrappers — TransitionSeries handles it
- No slide/wipe/flip transitions — fade() only for enterprise
- No transition longer than 30 frames (1 second)
- No animations faster than 15 frames — too jarring
- No elements appearing without blur-to-sharp + translate

### Audio
- No single combined voiceover file — use per-scene MP3s
- No BGM volume above 0.04 — voice must dominate
- No BGM fade in/out — constant volume sounds cleaner
- No audio overlap during transitions — add tail buffer to scenes
- No writing years as digits in ElevenLabs scripts — spell them out

### Content
- No Lorem Ipsum
- No generic stock photos — generate with nanobanana or curate
- No watermarked assets
- No em dashes (—) in on-screen text — use middle dots (·) or commas

---

# PART E: COMPLETE VIDEO WORKFLOW

## Step 1: Plan
- Define format (16:9, 9:16, 1:1)
- Outline scenes + timing (e.g., 3s intro, 2s text, 4s images, 2s data, 2s outro)
- List needed assets (backgrounds, product shots, icons, audio)

## Step 2: Generate Assets
1. `mcp__nanobanana-pro__set_aspect_ratio` matching video format
2. `mcp__nanobanana-pro__gemini_generate_image` for backgrounds, scenes (with conversation_id for consistency)
3. `mcp__lottiefiles__search_animations` for accent animations
4. `mcp__lucide__search_icons` / `mcp__iconify__search_icons` for UI elements
5. Download stock footage from Pexels/Pixabay if needed
6. Source audio (royalty-free)

## Step 3: Build Compositions
1. Create scene components using archetypes (Part C)
2. Wire with `TransitionSeries` (Part A3)
3. Add audio with volume envelope
4. Layer backgrounds, content, overlays via `AbsoluteFill`

## Step 4: Preview & Iterate
```bash
npm run dev   # Opens Remotion Studio at localhost:3000
```

## Step 5: Render
```bash
npx remotion render src/index.ts MainVideo out/video.mp4 --crf 18
```

---

# PART F: MULTI-AGENT ORCHESTRATION

## Parallel Production Pipeline
Use Claude Code's `Agent` tool to parallelize video production. Each agent works independently on a scene or asset batch.

### Strategy 1: Parallel Scene Building
Launch one agent per scene. Each agent writes its own component file.

```
Agent 1 (background): "Generate 5 AI images with nanobanana-pro for the video.
  Use conversation_id 'video-prod-1', aspect_ratio 16:9.
  Scene 1: cinematic landscape, dark blue tones
  Scene 2: close-up technology, holographic
  Scene 3: team collaboration, warm office
  Scene 4: data visualization, abstract
  Scene 5: sunrise skyline, orange-gold
  Save all to /path/to/project/public/"

Agent 2 (parallel): "Write src/compositions/IntroScene.tsx — a 3-second cinematic intro
  with spring-animated title, subtitle fade-in, and Ken Burns background zoom.
  Use @remotion/google-fonts Outfit. Dark theme."

Agent 3 (parallel): "Write src/compositions/ContentScene.tsx — a 4-second image
  montage using TransitionSeries with slide transitions between 3 images.
  Each image gets Ken Burns treatment."

Agent 4 (parallel): "Write src/compositions/DataScene.tsx — a 3-second stats
  section with 3 animated counters, staggered entry, monospace numbers."

Agent 5 (parallel): "Write src/compositions/OutroScene.tsx — a 2-second logo
  reveal with scale+blur spring animation and CTA text fade-in."
```

### Strategy 2: Asset Generation + Build Split
```
Agent 1 (assets):     "Search lottiefiles for confetti, logo-reveal, and loading
                       animations. Download JSON files to src/assets/"

Agent 2 (assets):     "Generate 6 product images with nanobanana-pro, consistent
                       style, save to public/. Then generate matching thumbnails."

Agent 3 (code):       "Build the full video composition in src/compositions/MainVideo.tsx
                       using TransitionSeries. 5 scenes, 30 seconds total, 30fps."

Agent 4 (code):       "Build reusable components: AnimatedText.tsx, Background.tsx,
                       ImageFrame.tsx with proper spring animations."
```

### Strategy 3: Multi-Format Render
After building one master composition with props:
```
Agent 1: "Render YouTube 16:9 version: npx remotion render src/index.ts YouTube out/youtube.mp4 --crf 18"
Agent 2: "Render Instagram Reel 9:16: npx remotion render src/index.ts InstagramReel out/reel.mp4"
Agent 3: "Render Instagram Post 1:1: npx remotion render src/index.ts InstagramPost out/post.mp4"
Agent 4: "Generate thumbnail: npx remotion still src/index.ts Thumbnail out/thumb.png --frame=45"
```

### Agent Communication Pattern
```
1. PLAN phase:     Main agent plans all scenes, defines timing, lists assets
2. DISPATCH phase: Launch agents in parallel (use run_in_background: true)
   - Asset agents: Generate images, search Lottie, find icons
   - Code agents: Write scene components (one per agent)
3. ASSEMBLE phase: Main agent wires scenes into Root.tsx with TransitionSeries
4. RENDER phase:   Launch render agents for each format in parallel
```

### Key Rules for Agent Coordination
- Each agent writes to its OWN file (no conflicts)
- Use consistent naming: `Scene1Intro.tsx`, `Scene2Content.tsx`, etc.
- Pass shared config (colors, fonts, fps) as constants in `lib/` files FIRST
- Asset agents save to `public/` with predictable names (`scene1-bg.png`, `scene2-bg.png`)
- Code agents reference assets by the same predictable names
- Main agent does final assembly in Root.tsx after all agents complete

---

# PART G: REMOTION CONFIG

### remotion.config.ts
```ts
import { Config } from "@remotion/cli/config";
Config.setVideoImageFormat("jpeg");
Config.setOverwriteOutput(true);
Config.setConcurrency(4);
Config.setChromiumOpenGlRenderer("angle");
```

### package.json scripts
```json
{
  "scripts": {
    "dev": "remotion studio src/index.ts",
    "build": "remotion render src/index.ts MainVideo out/video.mp4",
    "build:reel": "remotion render src/index.ts InstagramReel out/reel.mp4",
    "build:tiktok": "remotion render src/index.ts TikTok out/tiktok.mp4",
    "thumb": "remotion still src/index.ts Thumbnail out/thumb.png",
    "upgrade": "remotion upgrade"
  }
}
```
