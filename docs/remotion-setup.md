# Remotion Project Setup

## Create New Project
```bash
mkdir my-video-project && cd my-video-project
npm init -y
npm i --save-exact remotion@latest @remotion/cli@latest react react-dom
```

## Install Premium Packages
```bash
npm i @remotion/google-fonts       # Typography
npm i @remotion/transitions        # Scene transitions
npm i @remotion/noise              # Organic motion
npm i @remotion/shapes             # SVG shapes
npm i @remotion/paths              # Path animations
npm i @remotion/media-utils        # Audio visualization
npm i @remotion/motion-blur        # Cinematic blur
npm i @remotion/lottie lottie-web  # After Effects animations
npm i @remotion/captions           # TikTok-style captions
npm i @remotion/animated-emoji     # Animated emoji
```

## Upgrade All to Same Version
```bash
npx remotion upgrade
```

## Project Structure
```
src/
  index.ts                # registerRoot entry point
  Root.tsx                 # Composition definitions + TransitionSeries
  lib/
    colors.ts             # Color palette constants
    fonts.ts              # Google Font imports
  components/
    LogoWatermark.tsx      # Persistent logo overlay
  compositions/
    Scene1Intro.tsx        # Scene components (one per scene)
    Scene2Content.tsx
    Scene3Timeline.tsx
    Scene4Steps.tsx
    Scene5CTA.tsx
public/
  assets/
    bg-scene1.png          # AI-generated backgrounds
    bg-scene2.png
    vo-scene1.mp3          # Per-scene voiceover clips
    vo-scene2.mp3
    logo.png               # Brand logo
    bgm.mp3                # Background music
    mockup.png             # Product mockup (optional)
```

## package.json Scripts
```json
{
  "scripts": {
    "dev": "remotion studio src/index.ts",
    "render": "remotion render src/index.ts ERechnung out/video.mp4 --crf 18",
    "render:portrait": "remotion render src/index.ts Portrait out/portrait.mp4 --crf 18",
    "thumb": "remotion still src/index.ts Thumbnail out/thumb.png",
    "upgrade": "remotion upgrade"
  }
}
```

## Render Formats
```bash
# H.264 MP4 (default, best compatibility)
npx remotion render src/index.ts ID out/video.mp4 --crf 18

# ProRes MOV (highest quality, for editing)
npx remotion render src/index.ts ID out/video.mov --codec prores

# GIF (social media loops)
npx remotion render src/index.ts ID out/anim.gif --codec gif

# Thumbnail (single frame)
npx remotion still src/index.ts ID out/thumb.png --frame=45
```
