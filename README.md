# Videoskill

All-in-one Claude Code skill for automated video production, multi-platform publishing, and content scheduling.

## Repository Structure

```
.claude/skills/videoskill/
  SKILL.md              Core video engine (Remotion, MCPs, motion graphics, sync rules)
  BATCH.md              Batch pipeline: 10 scripts -> 20 videos parallel
  PUBLISH.md            Multi-platform publishing (6 platforms)

docs/
  mcp-setup.md          MCP server installation guide (15 servers)
  api-keys.md           Where to get every API key
  remotion-setup.md     Remotion project from scratch

guides/
  workflow.md           End-to-end daily workflow (single + batch)
  production-checklist.md  Pre-render QA checklist
  nice-to-haves.md      Advanced features (captions, 3D, CI/CD, repurposing)

templates/
  script-template.json  Example batch script with 5 scenes
  colors-template.ts    Brand color palette starter
```

## Pipeline Overview

```
10 Scripts -> /videoskill (Remotion) -> Render 16:9 + 9:16 -> Drive Review -> Publish 6 Platforms
```

| Step | Tool | Output |
|------|------|--------|
| Script Input | JSON or natural language | Scene breakdown |
| AI Backgrounds | nanobanana-pro (Gemini) | Premium images per scene |
| Voice-Over | ElevenLabs API | Per-scene MP3 clips |
| Video Build | Remotion + motion graphics | Enterprise compositions |
| Render | Remotion CLI | MP4 landscape + portrait |
| Review | Google Drive upload | Approval workflow |
| Publish | social-media MCP (649 tools) | YouTube, LinkedIn, X, IG, FB, TikTok |

## Quick Start

### 1. Install Skill
```bash
gh repo clone achilleascode/videoskill
cp -r videoskill/.claude/skills/videoskill ~/.claude/skills/
```

### 2. Install Required MCP Servers
```bash
claude mcp add nanobanana-pro -- npx -y @ycse/nanobanana-mcp
claude mcp add remotion-docs -- npx -y @remotion/mcp@latest
claude mcp add social-media -- npx -y @muhammadhamidraza/social-media-mcp-server
claude mcp add lottiefiles -- npx -y mcp-server-lottiefiles
claude mcp add lucide -- npx -y lucide-icons-mcp --stdio
claude mcp add iconify -- npx -y iconify-mcp-server@latest
```

### 3. Setup Remotion Project
```bash
mkdir my-videos && cd my-videos
npm init -y
npm i --save-exact remotion@latest @remotion/cli@latest react react-dom
npm i @remotion/google-fonts @remotion/transitions @remotion/noise @remotion/shapes @remotion/paths @remotion/media-utils
npx remotion upgrade
```

### 4. Set API Keys
```bash
echo "ELEVENLABS_API_KEY=sk_..." >> .env
```

### 5. Create Your First Video
```
/videoskill 30-second explainer about your topic, dark theme, enterprise style
```

### 6. Review & Publish
```bash
# Render
npx remotion render src/index.ts MainVideo out/video.mp4 --crf 18

# Upload to Google Drive for review
gdrive files upload out/video.mp4 --parent YOUR_FOLDER_ID

# Publish after approval
/video-publish out/video.mp4
```

## Key Production Rules

1. **Transitions:** Only `fade()`, max 30 frames (1 second)
2. **Audio:** Per-scene MP3s, NOT one combined voiceover
3. **Scene Duration:** Audio length + transition + 15 frame buffer
4. **No manual fadeOut** in scene components
5. **BGM:** Constant 2-4% volume, no fade
6. **Backgrounds:** Photography-style prompts, no AI slop
7. **Motion Graphics:** Every element uses blur-to-sharp + translate entrance
8. **ElevenLabs:** Spell out numbers ("zwanzig siebenundzwanzig")
9. **Layout:** Glassmorphism cards, consistent scene hierarchy
10. **Review:** Always upload to Drive before publishing

## Supported Platforms

| Platform | Format | Via |
|----------|--------|-----|
| YouTube | 16:9 (1920x1080) | social-media MCP |
| LinkedIn | 16:9 (1920x1080) | social-media MCP |
| Twitter/X | 16:9 (1280x720) | social-media MCP |
| Facebook | 16:9 (1920x1080) | social-media MCP |
| Instagram Reels | 9:16 (1080x1920) | social-media MCP |
| TikTok | 9:16 (1080x1920) | social-media MCP |

## License

MIT
