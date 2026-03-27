---
name: video-publish
description: Publish rendered videos to all social media platforms (YouTube, LinkedIn, Twitter/X, Instagram, Facebook, TikTok) via the social-media MCP server. Handles multi-format uploads and content scheduling.
argument-hint: "[video path or 'all' to publish batch]"
---

# Video Publishing Pipeline

Publish video: **$ARGUMENTS**

## Platform Matrix

| Platform | Format | Max Duration | Max Size | API Endpoint |
|----------|--------|-------------|----------|-------------|
| YouTube | 16:9 (1920x1080) | 12 hours | 256 GB | YouTube Data API v3 |
| LinkedIn | 16:9 (1920x1080) | 10 min | 5 GB | LinkedIn API v2 |
| Twitter/X | 16:9 (1280x720) | 140s | 512 MB | Twitter API v2 |
| Facebook | 16:9 (1920x1080) | 240 min | 10 GB | Graph API |
| Instagram | 9:16 (1080x1920) | 90s Reel | 1 GB | Graph API (needs public URL) |
| TikTok | 9:16 (1080x1920) | 10 min | 4 GB | Content Posting API |

## Publishing via Social Media MCP

The `social-media` MCP server exposes tools for all 6 platforms. Use these tools directly:

### YouTube
```
mcp__social-media__youtube_upload_video({
  file_path: "/path/to/video-landscape.mp4",
  title: "E-Rechnung wird Pflicht ab 2027",
  description: "Was bedeutet die E-Rechnungspflicht für Ihr Unternehmen? ...",
  tags: ["E-Rechnung", "ZUGFeRD", "XRechnung", "Buchhaltung"],
  privacy_status: "public",  // or "private", "unlisted"
  category_id: "22"  // People & Blogs
})
```

### LinkedIn
```
mcp__social-media__linkedin_create_video_post({
  video_path: "/path/to/video-landscape.mp4",
  text: "Die E-Rechnungspflicht kommt ab 2027. Was Unternehmen jetzt wissen müssen. #ERechnung #Digitalisierung",
  visibility: "PUBLIC"
})
```

### Twitter/X
```
mcp__social-media__twitter_upload_media({
  file_path: "/path/to/video-landscape.mp4"
})
// Then create tweet with media_id
mcp__social-media__twitter_create_tweet({
  text: "E-Rechnung wird ab 2027 Pflicht 📄➡️💻\n\nWas Sie jetzt wissen müssen:\n✅ Empfangspflicht ab 2025\n✅ Sendepflicht ab 2027\n✅ ZUGFeRD oder XRechnung\n\n#ERechnung #Buchhaltung",
  media_ids: ["<media_id>"]
})
```

### Instagram (Reel)
```
// Instagram requires video at a public URL
// Upload to Supabase Storage first, then:
mcp__social-media__instagram_create_reel({
  video_url: "https://your-supabase-url/storage/v1/object/public/videos/video-portrait.mp4",
  caption: "E-Rechnung ab 2027 Pflicht! Was Sie wissen müssen 👇\n\n#ERechnung #Unternehmen #Digitalisierung #Buchhaltung #ZUGFeRD"
})
```

### Facebook
```
mcp__social-media__facebook_upload_video({
  file_path: "/path/to/video-landscape.mp4",
  title: "E-Rechnung wird Pflicht ab 2027",
  description: "Was bedeutet die E-Rechnungspflicht für Ihr Unternehmen?"
})
```

### TikTok
```
mcp__social-media__tiktok_upload_video({
  file_path: "/path/to/video-portrait.mp4",
  title: "E-Rechnung ab 2027 Pflicht! #ERechnung #Business #Buchhaltung"
})
```

## Batch Publishing
For batch-produced videos, iterate over the output folder:
```
For each video in out/:
  1. Read metadata from scripts.json (title, description, tags, schedule)
  2. Upload landscape.mp4 to YouTube, LinkedIn, Twitter, Facebook
  3. Upload portrait.mp4 to Instagram, TikTok
  4. Log success/failure per platform
```

## Content Scheduling via n8n
For scheduled publishing, POST to an n8n webhook:
```bash
curl -X POST https://your-n8n-instance/webhook/video-publish \
  -H "Content-Type: application/json" \
  -d '{
    "video_url": "https://storage.url/video.mp4",
    "title": "E-Rechnung Pflicht 2027",
    "description": "...",
    "platforms": ["youtube", "linkedin", "twitter", "facebook", "instagram", "tiktok"],
    "schedule": "2026-03-28T09:00:00Z"
  }'
```

## Platform-Specific Tips
- **YouTube**: Add end screen, cards, chapters via description timestamps
- **LinkedIn**: Keep text under 1300 chars, use 3-5 hashtags
- **Twitter/X**: Max 280 chars text, thread for longer descriptions
- **Instagram**: First frame = thumbnail, use 20-30 hashtags in first comment
- **Facebook**: Native upload performs better than link shares
- **TikTok**: First 3 seconds must hook, use trending sounds if applicable

## Required API Keys (.env)
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

# Instagram (via Facebook)
FACEBOOK_PAGE_ACCESS_TOKEN=
INSTAGRAM_BUSINESS_ACCOUNT_ID=

# Facebook
FACEBOOK_PAGE_ID=
FACEBOOK_PAGE_ACCESS_TOKEN=

# TikTok
TIKTOK_CLIENT_KEY=
TIKTOK_CLIENT_SECRET=
TIKTOK_ACCESS_TOKEN=
```
