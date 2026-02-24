# Prompt Engineering for ASCII-Friendly Output

## The Golden Rule

**Black background, high contrast, bold shapes.** Everything else follows from this.

The WebGL2 ASCII renderer maps luminance (brightness) to characters. Dark pixels become transparent. Bright pixels become dense characters. The wider the luminance range in your scene, the richer the ASCII output.

---

## Image Prompts (Nanobanana)

### Required elements — include in EVERY prompt:
- `"solid black background, pure #000000 black"`
- `"high contrast, dramatic lighting"`
- `"strong silhouettes, bold shapes"`
- `"no text, no watermarks, no fine detail"`

### Composition by layout:

**Two-pillar (portrait 9:16):**
- `"portrait composition, tall and narrow"`
- `"centered vertical subject"` or `"symmetrical left-right composition"`
- Works with: columns, trees, vines, towers, waterfalls, totems, figures

**Full-width (landscape 16:9):**
- `"wide panoramic composition"`
- `"horizontal spread across frame"`
- Works with: skylines, landscapes, horizons, abstract waves, mountain ranges

### Color direction:
- Saturated primaries pop: `"deep emerald green"`, `"electric blue"`, `"rich gold"`, `"vivid magenta"`
- Avoid pastels — the 2.5x saturation boost makes them look washed
- Monochrome with one accent color reads beautifully: `"monochrome with golden accents"`
- Warm highlights on dark scenes: `"warm amber glow against darkness"`

### Style keywords that work well:
- `"ethereal glow"` — creates luminance gradients, beautiful in ASCII
- `"bioluminescent"` — bright organic forms on black, perfect for transparency
- `"dramatic chiaroscuro"` — extreme light/dark contrast
- `"neon against darkness"` — saturated light, pure black
- `"silhouette art"` — clean shapes, great readability
- `"art nouveau ornamental"` — decorative curves and organic forms

### Style keywords to AVOID:
- `"photorealistic"` — too much detail, looks muddy as ASCII
- `"soft lighting"` — low contrast, poor character variety
- `"pastel colors"` — wash out under saturation boost
- `"busy background"` — competes with subject, never fully transparent
- `"fine detail"`, `"intricate"` — lost at character-cell resolution
- `"gradient background"` — creates unwanted ASCII noise

### Example prompts:

**Stone pillars with vines:**
```
Two ancient stone pillars covered in climbing vines and blooming flowers,
solid black background pure #000000, high contrast dramatic lighting,
deep greens and vibrant magentas, strong silhouettes bold shapes,
portrait composition tall and narrow, ethereal glow on the flowers,
no text no watermarks no fine detail
```

**Mystical tree:**
```
A single ancient tree with spreading branches and glowing leaves,
solid black background pure #000000, dramatic lighting from within,
bioluminescent golden leaves against darkness, strong silhouette,
portrait composition centered, no text no watermarks no fine detail
```

**City skyline:**
```
Futuristic city skyline at night with neon lights reflecting,
solid black background pure #000000, high contrast neon against darkness,
electric blue and warm amber lights, wide panoramic composition,
bold geometric shapes strong silhouettes, no text no watermarks no fine detail
```

---

## Video Prompts (Veo 3.1)

### Required elements:
- `"maintain solid black background throughout"`
- `"slow cinematic continuous motion"`
- `"no camera shake, no cuts, no fast transitions"`

### Animation types that work:

**Growth (best for scroll):**
- `"vines gradually grow upward"`, `"flowers bloom one by one"`
- `"branches extend slowly"`, `"crystals form and grow"`
- Maps naturally to scroll — scroll down = growth progresses

**Reveal:**
- `"fog slowly clears to reveal"`, `"light gradually illuminates"`
- `"elements appear from darkness one by one"`
- Good for storytelling — scroll reveals the scene

**Transformation:**
- `"season changes from winter to spring"`, `"day transitions to night"`
- `"decay transforms into bloom"`, `"stone erodes to reveal gems"`
- Creates narrative arc across the scroll

### Animation types to AVOID:
- Fast motion (particles flying, explosions, rapid movement)
- Camera movement (dolly, pan, zoom — the user IS the camera)
- Scene cuts or transitions (must be one continuous shot)
- Looping motion (the video plays once, controlled by scroll position)

### Duration:
Always request `8 seconds`. This gives the most scroll content. The video plays at whatever speed the user scrolls.

### Example prompts:

**Growth animation:**
```
Slow cinematic animation: vines gradually grow upward along stone columns,
small flowers bloom one by one in vivid magenta and gold, leaves unfurl gently.
Maintain solid black background throughout. Smooth continuous motion,
no camera movement, no cuts, no fast transitions.
```

**Reveal animation:**
```
Slow cinematic reveal: a tree gradually becomes visible as golden light
spreads from its center outward, illuminating branches and leaves one by one.
Maintain solid black background throughout. Smooth continuous motion,
no camera shake, no cuts, no fast transitions.
```

---

## Common Mistakes

| Mistake | Result | Fix |
|---------|--------|-----|
| Non-black background | Scene doesn't float on page, gets messy border | Always specify pure #000000 black |
| Too much detail | Muddy ASCII, no clear shapes | Simplify subject, emphasize silhouettes |
| Pastel colors | Washed out after saturation boost | Use deep, saturated primary colors |
| Fast animation | Jerky on scroll, hard to control | Always request slow, continuous motion |
| Camera movement | Disorienting when scroll-driven | Request static camera, no movement |
| Busy composition | No clear focal point in ASCII | Single strong subject, lots of negative space |
