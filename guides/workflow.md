# End-to-End Workflow

## Daily Content Pipeline

```
Morning:  Prepare 10 scripts (JSON or natural language)
     ↓
Claude:   /videoskill produces each video (parallel agents)
     ↓
Render:   16:9 landscape + 9:16 portrait per video
     ↓
Upload:   gdrive files upload → Google Drive "Video Content Pipeline"
     ↓
Review:   You watch on Drive, approve or request changes
     ↓
Publish:  /video-publish to all platforms (or schedule via n8n)
```

## Single Video Workflow

### Step 1: Write the Script
```
/videoskill 30-second explainer about ZUGFeRD format, dark enterprise theme,
EasyFirma branding, German voiceover with Markus voice
```

### Step 2: Review in Remotion Studio
```bash
cd ~/remotion
npm run dev
# Open http://localhost:3000
```

### Step 3: Render
```bash
npx remotion render src/index.ts ERechnung out/video.mp4 --crf 18
```

### Step 4: Upload to Drive for Review
```bash
gdrive files upload out/video.mp4 --parent 1hh3rDpLSYlg0LLiKkovYBc04pn7LUwOb
```

### Step 5: Publish (after approval)
```
/video-publish out/video.mp4
```

## Batch Workflow

### Step 1: Prepare Scripts
Create `scripts.json` with 10 video definitions (see BATCH.md for format).

### Step 2: Run Batch
```
/video-batch scripts.json
```

### Step 3: Upload All to Drive
```bash
gdrive files upload out/ --parent 1hh3rDpLSYlg0LLiKkovYBc04pn7LUwOb --recursive
```

### Step 4: Review & Publish
Review each video on Drive, then publish approved ones.

## Platform-Specific Tips

### YouTube
- Title: max 100 chars, front-load keywords
- Description: first 2 lines visible without "show more"
- Tags: 10-15 relevant tags
- Thumbnail: custom still render at best frame
- Schedule: Best times DE = 17:00-20:00

### LinkedIn
- Text: max 1300 chars, 3-5 hashtags
- Native video performs 5x better than links
- Best times: Di-Do 08:00-10:00

### Twitter/X
- Text: max 280 chars, be punchy
- Use thread for longer context
- Best times: 12:00-13:00, 17:00-18:00

### Instagram (Reels)
- First 3 seconds must hook
- Add captions (sound often off)
- 20-30 hashtags in first comment
- Best times: 11:00-13:00, 19:00-21:00

### Facebook
- Native upload always (never YouTube links)
- Longer descriptions OK
- Best times: 13:00-16:00

### TikTok
- First frame = hook, no intro fluff
- Trending audio if applicable
- 3-5 hashtags in caption
- Best times: 19:00-22:00
