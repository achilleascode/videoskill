# API Keys & Credentials Guide

## Already Built-In (No Key Needed)
- Remotion (local rendering)
- Lucide, Iconify (public icon APIs)
- Lottiefiles (public search)

## Video Production Keys

### ElevenLabs (Voice-Over)
1. Sign up at https://elevenlabs.io
2. Go to Profile → API Keys
3. Copy your API key
```env
ELEVENLABS_API_KEY=sk_...
```

### Google AI / Gemini (nanobanana Image Generation)
1. Go to https://aistudio.google.com/apikey
2. Create API Key
```env
GOOGLE_AI_API_KEY=AI...
```

## Publishing Keys (per Platform)

### YouTube
Uses Google OAuth (same Google Cloud project as Google Drive).
1. Google Cloud Console → APIs → Enable "YouTube Data API v3"
2. Credentials → Your OAuth Client → Note Client ID + Secret
3. Run OAuth flow to get Refresh Token
```env
YOUTUBE_CLIENT_ID=177287...
YOUTUBE_CLIENT_SECRET=GO...
YOUTUBE_REFRESH_TOKEN=1//...
```

### Twitter/X
1. Go to https://developer.x.com
2. Sign up for Developer Account (Free tier: 1,500 tweets/month)
3. Create a Project → Create an App
4. Generate Consumer Keys + Access Token
```env
TWITTER_API_KEY=...
TWITTER_API_SECRET=...
TWITTER_BEARER_TOKEN=...
TWITTER_ACCESS_TOKEN=...
TWITTER_ACCESS_SECRET=...
```

### LinkedIn
1. Go to https://linkedin.com/developers
2. Create App → Request "Community Management API"
3. Auth tab → Generate Access Token
```env
LINKEDIN_CLIENT_ID=...
LINKEDIN_CLIENT_SECRET=...
LINKEDIN_ACCESS_TOKEN=...
```

### Facebook + Instagram (One App for Both)
1. Go to https://developers.facebook.com
2. Create Business App
3. Add "Facebook Login" + "Instagram Graph API" products
4. Get Page Access Token (long-lived, 60 days)
5. Find Instagram Business Account ID via Graph API Explorer
```env
FACEBOOK_PAGE_ID=...
FACEBOOK_PAGE_ACCESS_TOKEN=EAA...
INSTAGRAM_BUSINESS_ACCOUNT_ID=17841...
```
Note: Instagram requires video to be hosted at a public URL (use Supabase Storage or Google Drive shared link).

### TikTok
1. Go to https://developers.tiktok.com
2. Create App → Request "Content Posting API" scope
3. Note: Approval takes a few days
```env
TIKTOK_CLIENT_KEY=...
TIKTOK_CLIENT_SECRET=...
TIKTOK_ACCESS_TOKEN=...
```

## Google Drive (Review Pipeline)
Already set up via `gdrive` CLI:
```bash
brew install gdrive
gdrive account add
```
Video folder ID: Set in your workflow config.

## Where to Store Keys
Save all keys in your Remotion project `.env` file:
```bash
# /path/to/remotion-project/.env
ELEVENLABS_API_KEY=sk_...
GOOGLE_AI_API_KEY=AI...
# ... etc
```
Make sure `.env` is in `.gitignore`.
