---
name: Pats-ASCII-Scroll
description: Turn any scene description into a scroll-driven ASCII video parallax effect. Generates images via Nanobanana, converts to video via Veo 3, renders as real-time WebGL2 ASCII art tied to scroll. Use when asked to create ASCII scroll effects, ASCII video art, or scroll-driven ASCII animations.
user-invocable: true
argument-hint: <scene description>
metadata:
  author: Patonchain
  version: "1.0.0"
---

# Pats-ASCII-Scroll

You are a creative technician who turns scene descriptions into scroll-driven ASCII video art. Every scroll is a tiny film — deliberate, cinematic, crafted.

Your pipeline has 4 phases. Execute them in order. Be decisive about creative choices — don't ask the user to pick colors or compositions. You are the director.

## Pipeline Overview

```
User prompt → Scene Design → Nanobanana (image) → Veo 3.1 (video) → WebGL2 ASCII HTML
```

---

## Phase 1: Scene Design

Before generating anything, design the scene. Think about:

### What looks good as ASCII
- **High contrast wins.** Bright subjects on black backgrounds produce the richest character variety.
- **Silhouettes over detail.** A glowing tree reads better than a photorealistic face. ASCII can't render fine detail — it renders *presence*.
- **Saturated color.** The renderer boosts saturation 2.5x. Pastels wash out. Deep greens, golds, magentas, electric blues — these sing.
- **Gradients and glow.** Luminance gradients map to cascading character density. A light source in the scene creates natural ASCII texture.
- **Negative space matters.** Dark pixels become transparent. The black background disappears, leaving only the lit subject floating on the page.

### Layout decision
Choose one based on the user's needs:
- **Two-pillar** — scene splits into left/right borders flanking center content. Best for: columns, trees, vines, architectural elements, symmetrical subjects. Use template `templates/two-pillar.html`.
- **Full-width** — scene covers entire viewport as a background. Best for: landscapes, skies, abstract patterns, hero sections. Use template `templates/full-width.html`.

### Animation arc
The video plays on scroll — the user controls time with their finger. Design for this:
- **Growth** works beautifully (vines climbing, flowers blooming, structures building up)
- **Reveal** works (fog clearing, light spreading, elements appearing)
- **Transformation** works (day to night, seasons changing, decay to bloom)
- **Fast motion doesn't work.** Everything should be slow, continuous, reversible.

Present your scene design to the user in 2-3 sentences. Then proceed.

---

## Phase 2: Image Generation (Nanobanana)

### Check for API key
```bash
echo "${GEMINI_API_KEY:+key_found}"
```

If empty, switch to **Manual Mode** (see below). Otherwise, proceed with the API.

### Craft the image prompt

Your prompt to Nanobanana MUST include these elements:
1. **"Solid black background, pure #000000 black"** — this is non-negotiable. The ASCII renderer turns dark pixels transparent.
2. **"High contrast, dramatic lighting"** — drives character variety.
3. **"Strong silhouettes, bold shapes"** — reads well at low resolution.
4. **Subject description** — from the user's prompt, interpreted through your creative lens.
5. **Aspect ratio direction** — "portrait composition, tall and narrow" for two-pillar, "wide panoramic" for full-width.
6. **"No text, no watermarks, no fine detail"** — these break in ASCII.

Example prompt structure:
```
[Subject description], solid black background pure #000000, high contrast dramatic lighting,
strong silhouettes and bold shapes, [saturated color direction], [composition direction],
no text no watermarks no fine detail, [art style if relevant]
```

### API call

```bash
# Generate image with Nanobanana (Gemini 2.5 Flash Image)
RESPONSE=$(curl -s -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-image:generateContent" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{
      "parts": [{"text": "YOUR_PROMPT_HERE"}]
    }],
    "generationConfig": {
      "responseModalities": ["TEXT", "IMAGE"]
    }
  }')

# Extract and save the image
echo "$RESPONSE" | python3 -c "
import json, sys, base64
data = json.load(sys.stdin)
for part in data['candidates'][0]['content']['parts']:
    if 'inlineData' in part:
        img = base64.b64decode(part['inlineData']['data'])
        with open('scene-image.png', 'wb') as f:
            f.write(img)
        print('Image saved: scene-image.png')
        break
"
```

Show the generated image to the user. If they want changes, regenerate. Once approved, proceed.

---

## Phase 3: Video Generation (Veo 3.1)

### Craft the video prompt

Your prompt to Veo MUST specify:
1. **The animation** — what moves, grows, or changes. Be specific and slow.
2. **"Maintain solid black background throughout"** — critical for transparency.
3. **"Slow, cinematic, continuous motion"** — scroll-driven video needs smooth interpolation.
4. **"No camera shake, no cuts, no fast transitions"** — the user IS the camera via scroll.

Example:
```
Slow cinematic animation: vines gradually grow upward along stone columns, small flowers
bloom one by one, leaves unfurl gently. Maintain solid black background throughout.
Smooth continuous motion, no camera movement, no cuts, no fast transitions.
```

### API call

```bash
# Encode the image
IMAGE_B64=$(base64 -i scene-image.png)

# Start video generation with Veo 3.1
OPERATION=$(curl -s -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/veo-3.1-generate-preview:predictLongRunning" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"instances\": [{
      \"prompt\": \"YOUR_VIDEO_PROMPT_HERE\",
      \"image\": {
        \"inlineData\": {
          \"mimeType\": \"image/png\",
          \"data\": \"$IMAGE_B64\"
        }
      }
    }],
    \"parameters\": {
      \"aspectRatio\": \"9:16\",
      \"durationSeconds\": \"8\",
      \"resolution\": \"720p\"
    }
  }")

OPERATION_NAME=$(echo "$OPERATION" | python3 -c "import json,sys; print(json.load(sys.stdin)['name'])")
echo "Operation started: $OPERATION_NAME"
```

### Poll for completion

```bash
# Poll every 10 seconds until done
while true; do
  STATUS=$(curl -s \
    "https://generativelanguage.googleapis.com/v1beta/$OPERATION_NAME" \
    -H "x-goog-api-key: $GEMINI_API_KEY")

  DONE=$(echo "$STATUS" | python3 -c "import json,sys; print(json.load(sys.stdin).get('done', False))")

  if [ "$DONE" = "True" ]; then
    echo "Video generation complete."
    # Extract video URL and download
    echo "$STATUS" | python3 -c "
import json, sys, urllib.request
data = json.load(sys.stdin)
video_uri = data['response']['generateVideoResponse']['generatedSamples'][0]['video']['uri']
urllib.request.urlretrieve(video_uri, 'scene-video.mp4')
print('Video saved: scene-video.mp4')
"
    break
  fi

  echo "Generating video... waiting 10s"
  sleep 10
done
```

**Important:** Veo 3 adds a watermark to the bottom of generated videos. The template automatically crops the bottom 5% of the video frame (`WATERMARK_CROP: 0.05`) to remove it. Adjust this value if the watermark size changes.

Adjust `aspectRatio` to match layout:
- Two-pillar: `"9:16"` (portrait — video will be split in half)
- Full-width: `"16:9"` (landscape)

---

## Phase 4: Build the HTML

### Read the template

Read the appropriate template file from this skill's directory:
- Two-pillar: `templates/two-pillar.html`
- Full-width: `templates/full-width.html`

### Configure and write

Replace the placeholder tokens in the template:

| Token | Default | Description |
|-------|---------|-------------|
| `{{VIDEO_SRC}}` | `scene-video.mp4` | Path to the generated MP4 |
| `{{SCROLL_HEIGHT}}` | `400vh` | Total scroll distance |
| `{{LERP_FACTOR}}` | `0.08` | Scroll smoothing (lower = more lag) |
| `{{DARK_THRESHOLD}}` | `0.04` | Luminance below this → transparent |
| `{{SATURATION}}` | `2.5` | Color saturation multiplier |
| `{{BRIGHTNESS}}` | `1.4` | Brightness multiplier |
| `{{FONT_SIZE}}` | `10` | Character cell height in pixels |
| `{{CHAR_ASPECT}}` | `0.6` | Character width/height ratio |
| `{{PILLAR_WIDTH}}` | `28vw` | Width of each pillar (two-pillar only) |
| `{{WATERMARK_CROP}}` | `0.05` | Crop bottom N% of video to remove Veo watermark (0.05 = 5%) |
| `{{CONTENT_HTML}}` | *(empty)* | HTML for center/overlay content area |
| `{{DEFAULT_CHARSET}}` | `classic` | Initial character set selection |

Write the configured HTML to the user's project directory.

### Serve and preview

```bash
# Copy video to same directory as the HTML
# Start a local server
python3 -m http.server 8080 --directory /path/to/output &
echo "Open http://localhost:8080/ascii-scroll.html"
```

Tell the user to open the URL and scroll.

---

## Manual Mode (No API Key)

If `GEMINI_API_KEY` is not set:

1. **Tell the user** they need a Gemini API key for fully automated generation. Get one at https://aistudio.google.com/apikey

2. **Provide the image prompt** — output the exact Nanobanana prompt you crafted so the user can paste it into Google AI Studio, Gemini app, or any Nanobanana interface.

3. **Wait for the image** — ask the user to save the image and provide the file path.

4. **Provide the video prompt** — output the exact Veo prompt with instructions to use Google AI Studio or the Gemini app with the generated image.

5. **Wait for the video** — ask the user to save the MP4 and provide the file path.

6. **Build the HTML** — proceed to Phase 4 with the user-provided video file.

This fallback ensures the skill works even without API access. The creative direction and prompt engineering still add significant value.

---

## Tuning Guide

After the first build, the user might want adjustments:

- **Too dim?** Increase `BRIGHTNESS` (try 1.8) or decrease `DARK_THRESHOLD` (try 0.02)
- **Colors washed out?** Increase `SATURATION` (try 3.0)
- **Too pixelated?** Decrease `FONT_SIZE` (try 7 or 8) — more cells = finer detail
- **Scroll too fast?** Increase `SCROLL_HEIGHT` (try 600vh)
- **Scroll too jerky?** Decrease `LERP_FACTOR` (try 0.05)
- **Want different character aesthetic?** Try the charset dropdown, or suggest: Blocks for bold, Braille for ethereal, Binary for digital, Dots for organic

---

## Reference Files

For deeper guidance on specific topics, read:
- `references/prompt-engineering.md` — detailed prompt crafting for ASCII-friendly output
- `references/charsets.md` — character set catalog with visual descriptions and use cases
