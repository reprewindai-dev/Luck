# ClipCraft Studio - Vercel Deployment

## Deploy to Vercel

### Option 1: Vercel CLI (Recommended)
```bash
# Install Vercel CLI
npm i -g vercel

# Navigate to this folder
cd dist/

# Deploy
vercel

# Follow prompts:
# - Project name: clipcraft-studio (or your choice)
# - Framework: Other
# - Build settings: None (already built)
```

### Option 2: Vercel Dashboard
1. Go to https://vercel.com/new
2. Click **"Deploy without Git"**
3. Drag this entire `dist/` folder
4. Click **Deploy**

## Configure Backend URL

After deployment, set the environment variable:

### Via Vercel Dashboard
1. Go to your project → **Settings** → **Environment Variables**
2. Add variable for **All environments** (Production, Preview, Development):
   - **Name:** `EXPO_PUBLIC_BACKEND_URL`
   - **Value:** `https://dry-firefly-f8b4.reprewindai.workers.dev`
3. Click **Save**
4. **Redeploy** (Settings → Deployments → Redeploy)

### Runtime Configuration (No Redeploy)
If Vercel doesn't inject env vars at build time:
1. Open your deployed site
2. Modal appears: "Backend Not Configured"
3. Click **"Set Backend URL"**
4. Enter: `https://dry-firefly-f8b4.reprewindai.workers.dev`
5. URL saves to localStorage and reloads

## Features

✅ **iOS Safari Compatible** - .m4a, .mp3, .wav files work perfectly
✅ **Debug Mode** - Add `?debug=1` to URL for live diagnostics
✅ **Multi-Source Config** - 5 ways to set backend URL
✅ **No Server Required** - 100% static files

## File Structure

```
dist/
├── index.html          # Entry point
├── _expo/static/       # JS and CSS bundles (5.25 MB)
├── assets/             # Images and fonts
└── _redirects          # (ignored by Vercel)
```

## Testing Locally

```bash
cd dist/
npx serve

# Open: http://localhost:3000
```

## Debug Mode

Add `?debug=1` to see:
- Backend URL resolution
- File upload progress  
- Response status codes
- All errors

Example: `https://your-site.vercel.app?debug=1`

## Backend API

Uploads POST to: `${BACKEND_URL}/api/project/upload`

Expected response:
```json
{
  "projectId": "abc123",
  "status": "processing"
}
```

## Troubleshooting

**Backend Not Configured**
→ Use runtime configuration modal OR set env var and redeploy

**CORS Errors**
→ Backend must allow: `https://your-site.vercel.app`

**iOS Upload Fails**
→ Enable debug mode and check console

## Support

- Debug mode: `?debug=1`
- Browser console: F12
- Verify backend is accessible
