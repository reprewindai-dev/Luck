# ClipCraft Studio - Cloudflare Pages Deployment

## Quick Deploy to Cloudflare Pages

1. Go to https://dash.cloudflare.com/
2. Navigate to: **Workers & Pages** → **Create application** → **Pages** → **Upload assets**
3. **Extract this ZIP** and drag the entire `dist/` folder (or its contents) to the upload area
4. Click **Deploy site**

## Required: Configure Backend URL

After deployment, set the backend environment variable:

### Method 1: Via Cloudflare Dashboard (Recommended)
1. Go to your Pages project → **Settings** → **Environment variables**
2. Add variable:
   - **Name:** `EXPO_PUBLIC_BACKEND_URL`
   - **Value:** `https://dry-firefly-f8b4.reprewindai.workers.dev` (or your backend URL)
3. **Redeploy** for changes to take effect

### Method 2: Runtime Configuration (No Redeploy Needed)
If Cloudflare doesn't inject env vars at runtime:
1. Open your deployed site
2. You'll see "Backend Not Configured" modal
3. Click **"Set Backend URL"**
4. Enter: `https://dry-firefly-f8b4.reprewindai.workers.dev`
5. URL saves to localStorage and app reloads

## Features Included

✅ **iOS Safari Compatible**
- Proper .m4a, .mp3, .wav file handling
- Immediate upload after file selection
- MIME type auto-detection

✅ **Debug Mode**
- Add `?debug=1` to URL
- Shows live diagnostics:
  - Backend URL resolution
  - File upload progress
  - Response status codes
  - Error messages

✅ **Multi-Source Backend Configuration**
Priority order:
1. `window.__BACKEND_URL__` (global variable)
2. `localStorage["BACKEND_URL"]` (browser storage)
3. `EXPO_PUBLIC_BACKEND_URL` (env var)
4. `EXPO_PUBLIC_API_BASE_URL` (env var fallback)
5. `EXPO_PUBLIC_VIBECODE_BACKEND_URL` (legacy fallback)

## File Structure

```
dist/
├── index.html              # App entry point
├── _expo/
│   └── static/
│       ├── css/            # Stylesheets
│       └── js/             # JavaScript bundles (5.25 MB)
├── assets/                 # Images and fonts
└── _redirects              # Netlify redirects (optional, not needed for Cloudflare)
```

## Testing Locally

```bash
# Extract ZIP
unzip ClipCraft-Studio-PAGES-DEPLOY.zip

# Serve locally
cd dist/
python3 -m http.server 8000

# Open: http://localhost:8000
```

**Note:** Use runtime configuration (Method 2) when testing locally.

## Troubleshooting

**Problem:** "Backend Not Configured" error
**Solution:** Use Method 2 (runtime configuration) or set env var and redeploy

**Problem:** CORS errors
**Solution:** Backend must allow requests from your Cloudflare Pages domain

**Problem:** Upload doesn't work on iOS Safari
**Solution:** 
1. Enable debug mode: `?debug=1`
2. Check console logs
3. Verify backend URL is correct

## Backend API Endpoint

The app uploads files to: `${BACKEND_URL}/api/project/upload`

Expected response format:
```json
{
  "projectId": "abc123",
  "status": "processing"
}
```

## Support

- Add `?debug=1` to URL for diagnostics
- Check browser console for detailed logs
- Verify backend is accessible from browser
