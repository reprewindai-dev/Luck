# StudioGrade Kits

One-tap music content engine that converts uploaded audio or video files into complete, ready-to-post content packages for short-form music platforms.

## Features

- **One-Tap Upload**: Single action to upload audio (MP3, WAV, M4A) or video (MP4, MOV, WEBM) files
- **AI Processing Pipeline**:
  - Audio transcription with GPT-4O-TRANSCRIBE
  - **Strict song structure enforcement** (Intro 4 bars, Verse 1 16 bars, Hook 8 bars, Verse 2 8-12 bars, Outro 4 bars)
  - **Lyric authenticity preservation** - 85-90% from original transcription, no generic rewrites
  - **Transcription quality failsafe** - preserves intelligible lines, labels inferred content
  - Clip detection with emotional peak identification
  - Content generation matched to emotional tone
  - **Thumbnail variation enforcement** - unique compositions per upload, no duplicates
- **Cover Art Generation**:
  - **Multi-API support**: OpenAI GPT-Image-1 or Google Gemini
  - **Local fallback**: Branded SVG covers when no API is available
  - **Regenerate button**: Generate new cover art with composition rotation
  - **API state detection**: Disabled regenerate button with helper text when no API
  - 6 composition directives rotate on each regeneration
  - Cache-busting URLs prevent stale images
- **Complete Posting Kit**:
  - Posting decision (POST FULL / CLIP & POST / HOLD)
  - Best clip timestamps with hook points
  - **Inline audio preview** - tap to preview clips directly in the app
  - Copy-ready content (title, caption, hashtags, on-screen text, hook line)
  - Generated thumbnail with download (emotionally cohesive with lyrics)
  - Finalized lyrics with section markers (shows both raw transcript and structured song)
  - **Scrollable lyrics display** - fixed height container with vertical scroll
  - Timing guidance for optimal posting
- **Download & Save**:
  - Save audio to device
  - Save cover art to photos
- **Anti-Cache System**:
  - Unique runId per processing run
  - File fingerprint for thumbnail variation
  - Hard state reset between uploads
  - Cache-busting query strings on cover URLs

## AI Constraints

### Song Structure (Mandatory)
- Intro: 4 bars
- Verse 1: 16 bars
- Hook: 8 bars
- Verse 2: 8-12 bars
- Outro: 4 bars

### Lyric Rules
- 85-90% of lines must come from transcription
- New lines only to bridge sections (labeled [ADDED LINE])
- Inferred lines labeled [INFERRED LINE]
- Preserves slang, pacing, imperfections
- Never rewrites into generic motivational rap

### Lyric-Mastering Cohesion
- Introspective/tired lyrics → muted tones, soft shadows
- Aggressive/confident lyrics → bold contrasts, sharp highlights
- Content captions match emotional tone

### Thumbnail Variation
- 6 composition directives (close-up, wide, centered, asymmetrical, diagonal, minimalist)
- Unique variation key per generation (runId + fileFingerprint + timestamp)
- Color palette matched to emotional tone
- Composition rotates on each regeneration

## Tech Stack

### Frontend
- Expo SDK 53 with React Native
- NativeWind (Tailwind CSS)
- React Query for server state
- React Native Reanimated for animations
- Expo Router for navigation
- Expo AV for audio playback

### Backend
- Bun + Hono server
- Prisma ORM with SQLite
- Better Auth for authentication
- OpenAI APIs (GPT-5.2, GPT-4O-TRANSCRIBE, GPT-Image-1)
- Google Gemini API (fallback for image generation)

## Design

**Studio Gold Theme**
- Deep charcoal backgrounds (#0A0A0B, #111113)
- Warm gold/amber accents (#D4A853)
- Professional recording studio aesthetic
- Smooth animations and haptic feedback

## Screens

1. **Upload Screen** (`/`) - Upload audio/video with animated progress and stage indicators
2. **Results Screen** (`/results`) - Complete content kit with:
   - Recommendation banner
   - Quick action buttons (Save Audio, Save Cover)
   - Cover art preview with Regenerate button (or helper text if no API)
   - Playable clip previews with progress bars
   - Copy-ready content package
   - Scrollable lyrics section with raw transcript + structured song
   - Posting timing guidance

## API Endpoints

- `POST /api/project/upload` - Upload audio/video file (25MB limit)
- `GET /api/project/:id` - Get project status and results (includes `hasImageAPI` flag)
- `POST /api/project/:id/process` - Start AI processing pipeline
- `POST /api/project/:id/regenerate-thumbnail` - Generate new thumbnail with composition rotation

## Environment Variables

Set in the Vibecode ENV tab:
- `EXPO_PUBLIC_VIBECODE_OPENAI_API_KEY` - OpenAI API key for AI features
- `EXPO_PUBLIC_VIBECODE_GOOGLE_API_KEY` - Google API key for Gemini image generation (fallback)
