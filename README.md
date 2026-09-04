# Jarvis AI Patches

Hot-patch repository for the Jarvis AI Assistant desktop app.

## How It Works

1. The Jarvis desktop app fetches `manifest.json` from this repo on startup
2. If there's a new patch, it downloads the ZIP and extracts it to `%APPDATA%/JarvisAI/patches/`
3. The renderer HTTP server checks the patches directory first before the bundled files
4. The renderer reloads — instant update, no rebuild needed

## Manifest Format

```json
{
  "latestPatch": "001",
  "patches": [
    {
      "id": "001",
      "version": "1.0.847",
      "url": "https://github.com/SmallBoiTv/jarvis-patches/releases/download/patch-001/patch-001.zip",
      "sha256": "abc123...",
      "size": 12345678,
      "description": "Fix coding page terminal AI",
      "date": "2026-09-03",
      "restartRequired": false
    }
  ]
}
```

## Creating a Patch

```bash
# 1. Build only the renderer (2-3 min)
cd apps/desktop && vite build && node post-build.mjs

# 2. Zip the build output
cd apps/desktop
7z a -tzip ../../jarvis-patches/patches/patch-001.zip build/*

# 3. Compute SHA256
certutil -hashfile ../../jarvis-patches/patches/patch-001.zip SHA256

# 4. Update manifest.json with the new patch info

# 5. Create a GitHub release and upload the ZIP
gh release create patch-001 --repo SmallBoiTv/jarvis-patches \
  --title "Patch 001" --notes "Fix coding page terminal AI" \
  patches/patch-001.zip

# 6. Commit and push the updated manifest
git add manifest.json && git commit -m "patch 001" && git push
```
