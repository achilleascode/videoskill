# MCP Server Setup Guide

Complete list of all MCP servers used by the videoskill pipeline.

## Required (Core)

### 1. nanobanana-pro (AI Image Generation)
Generates custom background images via Google Gemini.
```bash
claude mcp add nanobanana-pro -- npx -y @ycse/nanobanana-mcp
```
**Env:** `GOOGLE_AI_API_KEY` (set in MCP env config)

### 2. remotion-docs (Remotion Documentation)
API reference lookup during video development.
```bash
claude mcp add remotion-docs -- npx -y @remotion/mcp@latest
```

### 3. social-media (Publishing — 649 Tools)
All-in-one publishing to YouTube, LinkedIn, Twitter/X, Instagram, Facebook, TikTok.
```bash
claude mcp add social-media -- npx -y @muhammadhamidraza/social-media-mcp-server
```
**Env:** Platform API keys (see `docs/api-keys.md`)

## Required (Assets)

### 4. lottiefiles (Lottie Animations)
Search and download professional After Effects animations.
```bash
claude mcp add lottiefiles -- npx -y mcp-server-lottiefiles
```

### 5. lucide (Line Icons)
Clean, consistent icon library.
```bash
claude mcp add lucide -- npx -y lucide-icons-mcp --stdio
```

### 6. iconify (200,000+ Icons)
Access to 150+ icon sets (Phosphor, Remix, Material, etc.).
```bash
claude mcp add iconify -- npx -y iconify-mcp-server@latest
```

## Required (Voice-Over)

### 7. elevenlabs (Text-to-Speech)
Professional German voiceovers. Python-based (official).
```bash
claude mcp add elevenlabs -- uvx elevenlabs-mcp
```
**Env:** `ELEVENLABS_API_KEY`

Alternative (direct API via curl — no MCP needed, built into SKILL.md).

## Optional (Design Inspiration)

### 8-12. UI Component Libraries
Query for visual patterns and motion inspiration.
```bash
claude mcp add shadcn -- npx -y shadcn@latest mcp
claude mcp add magic-ui -- npx -y @magicuidesign/mcp@latest
claude mcp add aceternity -- npx -y aceternityui-mcp
claude mcp add magic -- npx -y @21st-dev/magic@latest
claude mcp add reactbits -- npx -y reactbits-dev-mcp-server
```

### 13. gsap (Animation Patterns)
Complex animation reference and patterns.
```bash
claude mcp add gsap -- npx -y @vinhnguyen/gsap-mcp
```

## Optional (UI Screen Generation)

### 14. stitch (Google Stitch)
Generate UI screens for app walkthrough videos.
```bash
claude mcp add stitch -- npx -y @_davideast/stitch-mcp proxy
```
Requires: `npx @_davideast/stitch-mcp init` for OAuth setup.

## Optional (Scheduling)

### 15. n8n
Workflow automation for scheduled publishing.
```bash
claude mcp add n8n -- npx -y n8n-mcp-server
```

## Quick Install (All Required)
```bash
claude mcp add nanobanana-pro -- npx -y @ycse/nanobanana-mcp
claude mcp add remotion-docs -- npx -y @remotion/mcp@latest
claude mcp add social-media -- npx -y @muhammadhamidraza/social-media-mcp-server
claude mcp add lottiefiles -- npx -y mcp-server-lottiefiles
claude mcp add lucide -- npx -y lucide-icons-mcp --stdio
claude mcp add iconify -- npx -y iconify-mcp-server@latest
claude mcp add elevenlabs -- uvx elevenlabs-mcp
```
