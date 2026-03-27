# Nice-to-Have Features & Advanced Techniques

## Captions (TikTok-Style)
Add word-by-word highlighted subtitles for accessibility and engagement.
```bash
npm i @remotion/captions
```
- Generate SRT file from voiceover (ElevenLabs supports this)
- Use `createTikTokStyleCaptions()` for word-level highlighting
- Great for Instagram Reels and TikTok where sound is often off

## Audio Visualization
Add reactive frequency bars or waveforms synced to music/voice.
```bash
npm i @remotion/media-utils
```
- `visualizeAudio()` returns frequency spectrum array
- Map to bar heights, circle radii, or particle sizes
- Works with both voiceover and background music

## 3D Elements
Add Three.js 3D scenes directly in video compositions.
```bash
npm i @remotion/three @react-three/fiber three
```
- Rotating logos, 3D text, particle systems
- Use for product showcases or tech demos

## Lottie Animations
Embed professional After Effects animations.
```bash
npm i @remotion/lottie lottie-web
```
- Search via `mcp__lottiefiles__search_animations`
- Great for: confetti, loading spinners, icon animations, transitions

## Motion Blur
Add cinematic blur to fast-moving elements.
```bash
npm i @remotion/motion-blur
```
- `<CameraMotionBlur>` for natural camera blur
- `<Trail>` for echo/trail effects on moving elements

## Path Morphing
Animate SVG paths — draw-on effects, shape morphing.
```bash
npm i @remotion/paths
```
- `evolvePath()` — line drawing animation
- `interpolatePath()` — morph between two shapes

## Google Stitch Integration
Generate polished UI screens and turn them into video walkthroughs.
- Design at stitch.withgoogle.com
- Pull screenshots via Stitch MCP
- Animate with Ken Burns, zoom reveals, device frames
- Official Remotion skill: `npx skills add google-labs-code/stitch-skills --skill remotion --global`

## Multi-Language Support
- ElevenLabs supports 30+ languages
- Generate voiceovers in EN, DE, FR, ES, etc.
- Same video composition, different audio tracks
- Create `Composition` variants per language

## Automated Thumbnails
- Register a `<Still>` composition for each video
- Render with `npx remotion still` at the most visually striking frame
- Usually frame 60-90 of the CTA/final scene

## Content Repurposing
From one 30s landscape video, automatically create:
- 9:16 portrait version (Reels/TikTok)
- 1:1 square version (Instagram feed)
- 5s Story snippet (highlight the key message)
- Static thumbnail (for blog posts)
- GIF version (for email/web embeds)

## Performance Tips
- Use `OffthreadVideo` instead of `Video` for long clips
- Compress images before embedding (max 2MB each)
- Set `concurrency: 4` in remotion.config.ts for parallel rendering
- Use `--crf 18` for quality/size balance (lower = bigger file, better quality)

## CI/CD Pipeline (GitHub Actions)
Automate video rendering on push:
```yaml
name: Render Videos
on: push
jobs:
  render:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npx remotion render src/index.ts MainVideo out/video.mp4
      - uses: actions/upload-artifact@v4
        with:
          name: rendered-videos
          path: out/
```
