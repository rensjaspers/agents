---
name: optimize-assets
description: Optimizes images and audio for the web using ImageMagick and ffmpeg. Use when optimizing assets, converting images to WebP, converting WAV to MP3, or reducing asset file sizes.
---

# Optimize Assets

Optimize images and audio for web delivery. Use ImageMagick for images, ffmpeg for audio.

## Images

Convert to WebP with ImageMagick. Target: small file size without visible quality loss.

**Command:**
```bash
convert input.jpg -quality 80 -define webp:method=6 -define webp:auto-filter=true output.webp
```

- **Quality:** 80 for photos (balance size/quality). Use 70–75 for thumbnails or decorative images.
- **Method:** 6 for maximum compression (slower but smaller).
- **Resize:** If the image is displayed small in the app (e.g. thumbnail, icon), resize first to the display dimensions before converting. Use `-resize WxH` or `-resize WxH^` to preserve aspect ratio.

**Example with resize:**
```bash
convert input.jpg -resize 400x -quality 80 -define webp:method=6 output.webp
```

## Audio

Convert WAV to MP3 with ffmpeg. Bitrate depends on content type.

**Voice/podcast (speech only):**
```bash
ffmpeg -i input.wav -c:a libmp3lame -b:a 96k -ac 1 output.mp3
```
- 64–96 kbps mono is sufficient for speech.

**Podcast with music:**
```bash
ffmpeg -i input.wav -c:a libmp3lame -b:a 128k output.mp3
```

**Music:**
```bash
ffmpeg -i input.wav -c:a libmp3lame -b:a 192k output.mp3
```
- 192–256 kbps for music. Use 256 kbps when quality matters more than size.

## Workflow

1. Determine content type and display size (for images) or usage (for audio).
2. Choose appropriate quality/bitrate.
3. Convert. For images: resize if needed, then convert to WebP.
4. Verify output looks/sounds acceptable.
