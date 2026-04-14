---
name: seedance
description: Generate cinematic motion graphics videos using ByteDance Seedance 2.0 via Fal AI. Covers reference-to-video, text-to-video, liquid glass prompting, App Store screenshot scraping, and the full pipeline from screenshot to finished promo clip. Triggers on seedance, cdance, motion graphics, liquid glass, app promo, video promo.
---

# Seedance — Seedance 2.0 Motion Graphics Skill

Generate cinematic motion graphics videos using ByteDance's Seedance 2.0 model via Fal AI. Turn app screenshots into glossy liquid glass promo videos controlled from Claude Code.

## What This Skill Does

- Generate **reference-to-video** motion graphics from app screenshots
- Generate **text-to-video** motion graphics from pure prompts
- Scrape **App Store screenshots** automatically for any iOS app
- Upload reference images to get **public URLs** for the API
- Apply proven **liquid glass prompting patterns** that produce Apple-keynote-quality output

## Setup

### 1. Get a Fal AI API Key

Sign up at [fal.ai](https://fal.ai) and create an API key from your dashboard.

### 2. Store the Key

Add to your `.env` file:

```
FAL_KEY=your-fal-ai-key-here
```

### 3. (Optional) File Upload Key

Seedance needs **publicly accessible URLs** for reference images. If you have a Kie AI account, add:

```
KIE_API_KEY=your-kie-ai-key-here
```

Alternatives: any file hosting that returns a public URL (litterbox, S3, etc).

> **Want a full Generations tab with gallery, styles panel, and multi-model support (Kling, Veo, GPT Image, Nano Banana)?** Check out the Rubric dashboard at [robonuggets.com](https://robonuggets.com).

---

## Endpoints

Seedance 2.0 has 4 endpoints on Fal AI. **Default to Pro** — it's only ~25% more expensive but noticeably better quality.

| Type | Tier | Endpoint | $/sec (720p) |
|------|------|----------|-------------|
| Reference-to-video | **Pro (default)** | `fal.run/bytedance/seedance-2.0/reference-to-video` | $0.30 |
| Reference-to-video | Fast (testing) | `fal.run/bytedance/seedance-2.0/fast/reference-to-video` | $0.24 |
| Text-to-video | **Pro (default)** | `fal.run/bytedance/seedance-2.0/text-to-video` | $0.30 |
| Text-to-video | Fast (testing) | `fal.run/bytedance/seedance-2.0/fast/text-to-video` | $0.24 |

### When to Use Which

- **Reference-to-video** — you have screenshots, logos, or visual assets to base the video on. This is the primary mode.
- **Text-to-video** — no visual references available (e.g. GitHub repos, docs pages, abstract concepts). Pure prompt-driven.

---

## Parameters

| Param | Required | Options | Default |
|-------|----------|---------|---------|
| `prompt` | Yes | string | — |
| `image_urls` | No | array of public URLs, up to 9 | — |
| `video_urls` | No | array of public URLs, up to 3 | — |
| `audio_urls` | No | array of public URLs, up to 3 | — |
| `resolution` | No | `"480p"`, `"720p"` | `"720p"` |
| `duration` | No | `"auto"`, `"4"` – `"15"` | `"auto"` |
| `aspect_ratio` | No | `"auto"`, `"21:9"`, `"16:9"`, `"4:3"`, `"1:1"`, `"3:4"`, `"9:16"` | `"auto"` |
| `generate_audio` | No | boolean | `true` |
| `seed` | No | integer | random |

- **Total files across all modalities must not exceed 12**
- Reference images in prompts: `@Image1`, `@Image2`, ... `@Image9`
- Reference videos: `@Video1`, `@Video2`, `@Video3`
- Reference audio: `@Audio1`, `@Audio2`, `@Audio3`
- Max resolution is **720p** — there is no 1080p option

### Cost Reference (Pro, 720p)

| Duration | Cost |
|----------|------|
| 4s | $1.21 |
| 5s | $1.51 |
| 8s | $2.42 |
| 10s | $3.03 |
| 12s | $3.63 |
| 15s | $4.54 |

Fast tier is ~20% cheaper. Use for quick tests before committing to Pro.

---

## Sweet Spot (Tested)

These are the proven ratios from extensive testing:

| Refs | Duration | Use Case | Cost (Pro 720p) |
|------|----------|----------|-----------------|
| 1 ref | 4s | Quick test, single screenshot | $1.21 |
| 3 refs | 4–6s | Fast promo test | $1.21–$1.81 |
| **5 refs** | **10s** | **Production promo (best results)** | **$3.03** |
| 3 refs | 4s 480p | Cheapest possible test | ~$0.81 |

**The golden rule: 1 reference image per 2 seconds of video.**

- Fewer refs = model focuses better on each one
- More refs (6-7+) = model blends too loosely, loses clarity
- Always describe each image's role in the prompt

---

## Prompting Guide

### Rule 1: Logo References First (CRITICAL)

**NEVER describe logos or brand elements in the prompt text.** Seedance mangles text rendering — wrong shapes, wrong colors, hallucinated details.

Before writing ANY prompt:
1. Ask for or find the app/brand logo image
2. Save it locally
3. Upload it to get a public URL
4. Pass it as one of the `@Image` references

If you don't have the logo: **STOP and ask.** Never proceed without it.

**Recommended approach:** Generate the motion graphics without text/logos in the video at all, then overlay the logo in post-production (Premiere, After Effects, etc). This produces the cleanest results.

### Rule 2: The Liquid Glass Prompt Template

This is the proven prompt structure that produces Apple-keynote-quality motion graphics:

```
Design a motion-graphics style ad using glossy liquid glass design
language. Pure black background. @Image1 is [describe what it shows].
@Image2 is [describe]. @Image3 is [describe]. The [element] from
Image 1 becomes translucent glass [describe transformation].
[Element] from Image 2 [describe]. [Continue for each image].
Multiple camera angles: close-up on glass reflections, pull back
to reveal the full scene. No text, no logos, no words. Style: liquid
glass morphism, Apple Vision Pro aesthetic, premium 3D depth,
self-luminous forms on absolute black, [accent color] accent lighting.
```

**Key rules:**
- Describe each `@Image` by number and what it contains
- Describe how each element transforms into glass
- End with a style line including the brand's accent color
- `"Multiple camera angles"` keeps it dynamic
- `"No text, no logos, no words"` prevents garbled text
- Keep under ~200 words — the model ignores long prompts

### Rule 3: Device Framing

For product shots, specify the device in the prompt:

**Floating desktop screens (best results):**
```
Show the desktop screens floating in 3D space on a pure black
background, tilted at slight angles like a MacBook product shot.
The UI elements on screen become translucent glass with reflections
and refractions.
```

**iPad:**
```
An iPad Pro floating in empty black space, tilted at a cinematic
angle like an Apple product shot. The iPad has visible black bezels
and a physical frame — only the screen content has the glass effect.
```

**MacBook:**
```
A MacBook Pro floating in empty black space, open at a cinematic
angle. The MacBook screen displays the dashboard. Light catches
the aluminium edges.
```

**Floating in black void** produces better results than placing devices in environments (offices, desks). Keep it pure black.

### Rule 4: Audio

- `generate_audio: true` adds ambient SFX and music
- May trigger **content policy violations** on abstract visuals — if it fails, retry with `generate_audio: false`
- Audio costs the same whether enabled or not
- When it works, the generated audio adds real production value

### Rule 5: Negative Guidance

Add constraints to prevent common issues:
- `"No text, no logos, no words"` — prevents garbled text
- `"No cuts"` — keeps it as one continuous shot
- `"No camera shake"` — keeps it stable
- `"Pure visual motion only"` — reinforces no text

---

## The Full Pipeline

### Step 1: Get Screenshots

**Option A — Screenshot a website:**

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    page = browser.new_page(viewport={"width": 1920, "height": 1080})
    page.goto("https://example.com", wait_until="networkidle", timeout=30000)
    page.wait_for_timeout(2000)
    page.screenshot(path="screenshot.png")
    browser.close()
```

**Option B — Scrape App Store screenshots:**

```python
import json, urllib.request

# Step 1: Find the app ID
search_url = "https://itunes.apple.com/search?term=APP_NAME&entity=software&limit=3"
data = json.loads(urllib.request.urlopen(search_url).read())
app_url = data["results"][0]["trackViewUrl"]

# Step 2: Scrape screenshot URLs from the page using Playwright
# Look for <source> elements with srcset containing "mzstatic.com"
# The URLs use format like /460x996bb.webp — swap to /1290x2796bb.jpg for full res
```

**Option C — Manual screenshots** — take them yourself, save locally.

### Step 2: Upload for Public URL

Seedance needs publicly accessible URLs. Upload via any service that returns a URL.

**Using Kie AI (if you have a key):**

```bash
curl -s -X POST "https://kieai.redpandaai.co/api/file-stream-upload" \
  -H "Authorization: Bearer $KIE_API_KEY" \
  -F "file=@/path/to/screenshot.png" \
  -F "uploadPath=generations" \
  -F "fileName=my-screenshot.png"
```

Returns `downloadUrl` — use that as the public URL. Files expire after 3 days.

**Using litterbox (no account needed):**

```bash
curl -s -F "reqtype=fileupload" -F "time=1h" \
  -F "fileToUpload=@/path/to/screenshot.png" \
  https://litterbox.catbox.moe/resources/internals/api.php
```

Returns a direct URL. Expires after 1 hour.

### Step 3: Generate the Video

**Always use a temp JSON file** to avoid shell escaping issues:

```bash
cat > /tmp/seedance_body.json << 'ENDJSON'
{
  "prompt": "Design a motion-graphics style ad using glossy liquid glass design language. Pure black background. @Image1 is [describe]. @Image2 is [describe]. @Image3 is [describe]. The elements become translucent glass with reflections and refractions. Multiple camera angles. No text, no logos, no words. Style: liquid glass morphism, Apple Vision Pro aesthetic, premium 3D depth, self-luminous forms on absolute black.",
  "image_urls": [
    "https://your-upload-url/screenshot1.png",
    "https://your-upload-url/screenshot2.png",
    "https://your-upload-url/screenshot3.png"
  ],
  "resolution": "720p",
  "duration": "4",
  "aspect_ratio": "16:9",
  "generate_audio": false
}
ENDJSON

curl -s -X POST "https://fal.run/bytedance/seedance-2.0/reference-to-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d @/tmp/seedance_body.json
```

### Step 4: Download the Result

The response contains a video URL:

```json
{
  "video": { "url": "https://v3b.fal.media/files/.../video.mp4" },
  "seed": 42
}
```

**Download immediately** — URLs may expire.

```bash
curl -sL -o "output_video.mp4" "THE_VIDEO_URL"
```

### Step 5: Post-Production (Recommended)

Overlay the logo and any text in your video editor. The motion graphics are the visual base — branding goes on top in post.

---

## Text-to-Video (No References)

For apps/projects where you don't have screenshots (GitHub repos, docs pages, abstract concepts), use text-to-video with the timestamped approach:

```
Ultra-sleek motion design sequence. Pure black background throughout.
Glossy liquid glass design language.

0-1s [FORM]: A translucent glass [object] materializes at center,
light refracting through it.

1-2s [CONNECT]: Glass connections form between elements, each a thin
glass tube filling with luminous light.

2-3s [REVEAL]: The structure rotates revealing depth and glass
reflections.

3-4s [GLOW]: A pulse of warm light passes through the entire glass
structure.

Style: liquid glass morphism, premium 3D depth, self-luminous forms
on absolute black.
```

**Endpoint:** `https://fal.run/bytedance/seedance-2.0/text-to-video` (Pro)

Same parameters minus `image_urls`, `video_urls`, `audio_urls`.

---

## Prompt Patterns Library

### Floating Desktop Screens (Best for SaaS/Apps)

```
Design a motion-graphics style ad based on these images, using
multiple camera angles — close-ups that highlight the glass
reflections, various tap and click interaction animations, and a
focus on this glossy glass UI design language. @Image1 is [describe].
@Image2 is [describe]. @Image3 is [describe]. Show the desktop
screens floating in 3D space on a pure black background, tilted at
slight angles like a MacBook product shot. The UI elements on screen
become translucent glass with reflections and refractions. No text,
no logos, no words. Style: liquid glass morphism, Apple product ad,
premium desktop app showcase, self-luminous forms on absolute black,
[color] accent lighting.
```

### Glass UI Ad (Based on LEPADPHONE Style)

```
Design a motion-graphics style ad based on this image, using multiple
camera angles — close-ups that highlight the glass reflections,
various tap/click interaction animations, and a focus on this glossy
"glass UI" design language. @Image1 is [describe the app/interface].
The interface elements become translucent glass panels with
refractions. No text, no logos. Style: liquid glass morphism, premium
depth, self-luminous forms on absolute black.
```

### Abstract Motion Identity (Based on @oggii_0 Style)

```
Ultra-sleek motion design sequence. Pure black background throughout.
White-on-black minimal aesthetic. No cuts — one single continuous
unbroken visual evolution. Locked-off static center frame, no camera
movement. All motion occurs within the subject. Self-luminous white
forms on absolute black, no grain, no shadows.

0-2s [ORIGIN]: [Starting visual]
2-4s [TRANSFORM]: [Transformation]
4-6s [REVEAL]: [Reveal]
6-8s [CLOSE]: [Closing visual]

Style: [brand] motion identity, mathematical precision.
Mood: calm, inevitable, intelligent, premium.
```

---

## Gotchas & Known Issues

1. **Text rendering is bad** — never ask Seedance to render text. Overlay in post.
2. **Audio content policy** — abstract visuals may trigger false positives. Default to `generate_audio: false`, enable when needed.
3. **People's faces in screenshots** — may trigger "likenesses of real people" content policy. Crop faces out of screenshots.
4. **Shell escaping breaks JSON** — always use temp JSON files with `cat > /tmp/file.json`, never inline JSON in curl commands.
5. **Kie AI upload URLs expire after 3 days** — re-upload if generating later.
6. **Max resolution is 720p** — no 1080p. Upscale in post if needed.
7. **Pro vs Fast** — Pro is only ~25% more expensive but noticeably better. Default to Pro.
8. **7+ reference images** — quality degrades. Stick to 5 or fewer.

---

## Auth Header

Fal AI uses `Authorization: Key` (not Bearer):

```
Authorization: Key YOUR_FAL_KEY
```

---

## Credits

Built by [RoboNuggets](https://robonuggets.com). Tested extensively with Rubric dashboard screenshots, App Store scraping, and liquid glass motion graphics.

Seedance 2.0 is by ByteDance. Fal AI is the API provider.
