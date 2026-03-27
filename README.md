# Videoskill — All-in-One Content Production & Publishing Pipeline

Complete Claude Code skill for automated video production, multi-platform publishing, and content scheduling.

## What's Inside

```
.claude/skills/videoskill/
  SKILL.md      # Core video production engine (Remotion + 15 MCPs)
  BATCH.md      # Batch pipeline: 10 scripts → 20 videos (16:9 + 9:16)
  PUBLISH.md    # Multi-platform publishing (6 platforms)
```

## Pipeline Overview

```
Scripts (JSON) → /videoskill (Remotion) → Render 16:9 + 9:16 → Publish to 6 Platforms
```

| Step | Tool | What it does |
|------|------|-------------|
| 1. Script Input | JSON/CSV or natural language | Define scenes, voiceover text, style |
| 2. Asset Generation | nanobanana-pro MCP (Gemini) | AI background images per scene |
| 3. Voiceover | ElevenLabs API | German TTS with scene-synced timing |
| 4. Video Build | Remotion + @remotion/* packages | Enterprise motion graphics |
| 5. Render | Remotion CLI | 16:9 landscape + 9:16 portrait |
| 6. Publish | social-media MCP | YouTube, LinkedIn, Twitter, Instagram, Facebook, TikTok |
| 7. Schedule | n8n Webhook | Content calendar with cron scheduling |

## MCP Servers Required

### Core (Video Production)
```bash
claude mcp add nanobanana-pro -- npx -y @ycse/nanobanana-mcp
claude mcp add remotion-docs -- npx -y @remotion/mcp@latest
claude mcp add lottiefiles -- npx -y mcp-server-lottiefiles
claude mcp add lucide -- npx -y lucide-icons-mcp --stdio
claude mcp add iconify -- npx -y iconify-mcp-server@latest
```

### Publishing (Social Media)
```bash
claude mcp add social-media -- npx -y @muhammadhamidraza/social-media-mcp-server
```

### Optional (Design Inspiration)
```bash
claude mcp add shadcn -- npx -y shadcn@latest mcp
claude mcp add magic-ui -- npx -y @magicuidesign/mcp@latest
claude mcp add aceternity -- npx -y aceternityui-mcp
claude mcp add magic -- npx -y @21st-dev/magic@latest
claude mcp add gsap -- npx -y @vinhnguyen/gsap-mcp
```

### Optional (UI Screen Generation)
```bash
claude mcp add stitch -- npx -y @_davideast/stitch-mcp proxy
```

### Optional (Scheduling via n8n)
```bash
claude mcp add n8n -- npx -y n8n-mcp-server
```

## API Keys Required

### .env (Remotion Project)
```env
# ElevenLabs (Voiceover)
ELEVENLABS_API_KEY=sk_...

# Nanobanana (AI Images) — set via MCP env
GOOGLE_AI_API_KEY=...
```

### Social Media Platform Credentials
Set these as environment variables for the social-media MCP server:

```env
# YouTube
YOUTUBE_CLIENT_ID=
YOUTUBE_CLIENT_SECRET=
YOUTUBE_REFRESH_TOKEN=

# LinkedIn
LINKEDIN_CLIENT_ID=
LINKEDIN_CLIENT_SECRET=
LINKEDIN_ACCESS_TOKEN=

# Twitter/X
TWITTER_API_KEY=
TWITTER_API_SECRET=
TWITTER_BEARER_TOKEN=
TWITTER_ACCESS_TOKEN=
TWITTER_ACCESS_SECRET=

# Instagram (via Facebook Business)
FACEBOOK_PAGE_ACCESS_TOKEN=
INSTAGRAM_BUSINESS_ACCOUNT_ID=

# Facebook
FACEBOOK_PAGE_ID=

# TikTok
TIKTOK_CLIENT_KEY=
TIKTOK_CLIENT_SECRET=
TIKTOK_ACCESS_TOKEN=
```

## Remotion Project Setup

```bash
mkdir my-video-project && cd my-video-project
npm init -y
npm i --save-exact remotion@latest @remotion/cli@latest react react-dom
npm i @remotion/google-fonts @remotion/transitions @remotion/noise @remotion/shapes @remotion/paths @remotion/media-utils
npx remotion upgrade
```

## Usage

### Single Video
```
/videoskill 30-second explainer about E-Rechnung, dark theme, enterprise style
```

### Batch Production
```
/video-batch scripts.json
```

### Publish to All Platforms
```
/video-publish out/vid-001-landscape.mp4
```

## Key Production Rules

1. **Transitions:** Only `fade()`, max 30 frames (1 second)
2. **Audio:** Per-scene MP3 files, NOT one combined voiceover
3. **Scene Duration:** Audio length + transition + 15 frame buffer
4. **No manual fadeOut** in scene components (TransitionSeries handles it)
5. **BGM:** Constant 2-4% volume, no fade in/out
6. **Backgrounds:** Photography-style prompts, no AI slop
7. **ElevenLabs:** Spell out numbers in German scripts
8. **Layout:** Glassmorphism cards, blur-to-sharp entrances, consistent hierarchy

## Installation

```bash
# Clone and install skill
gh repo clone achilleascode/videoskill
cp -r videoskill/.claude/skills/videoskill ~/.claude/skills/

# Install MCP servers (run each line)
claude mcp add nanobanana-pro -- npx -y @ycse/nanobanana-mcp
claude mcp add remotion-docs -- npx -y @remotion/mcp@latest
claude mcp add social-media -- npx -y @muhammadhamidraza/social-media-mcp-server
claude mcp add lottiefiles -- npx -y mcp-server-lottiefiles
claude mcp add lucide -- npx -y lucide-icons-mcp --stdio
claude mcp add iconify -- npx -y iconify-mcp-server@latest
```

## License

MIT
