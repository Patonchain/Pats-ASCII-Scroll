# Character Set Catalog

The ASCII renderer maps video luminance to characters. Fewer bright pixels in the character = assigned to darker areas. More bright pixels = assigned to brighter areas. Space is always the darkest (transparent).

## Built-in Character Sets

### Classic
```
 .,:;+*?%S#@
```
**12 characters.** The workhorse. Smooth luminance gradient from whisper-thin dots to dense blocks. Works with everything. Start here.

**Best for:** General use, detailed scenes, gradients

### Blocks
```
 ░▒▓█
```
**5 characters.** Posterized, chunky, retro. Jumps between density levels rather than smooth gradation. Feels like a low-res display.

**Best for:** Retro aesthetics, 8-bit style, bold graphic scenes

### Minimal
```
 .-=+#
```
**6 characters.** Clean, understated. Good separation between levels but not too many. Reads easily at small sizes.

**Best for:** Clean designs, readable text overlays, subtle backgrounds

### Dense
```
 .:-=+*#%@
```
**10 characters.** Similar to Classic but slightly different character weight distribution. A touch more contrast in the mid-tones.

**Best for:** Alternative to Classic when you want punchier mid-range

### Dots
```
 ·:;o*O@
```
**8 characters.** Rounded, organic feel. The dots and circles create a softer texture than angular characters.

**Best for:** Organic scenes (nature, water, clouds), dreamy aesthetics

### Binary
```
 01
```
**3 characters.** Stark. Either transparent, 0, or 1. Maximum digital aesthetic. Creates hard edges and high contrast.

**Best for:** Matrix/hacker aesthetics, digital themes, cyberpunk

### Slashes
```
 /|\-=*#
```
**8 characters.** Directional characters create an illusion of texture and flow. Lines and angles dominate.

**Best for:** Architectural scenes, geometric patterns, rain/movement effects

### Braille
```
 ⠁⠃⠇⡇⣇⣧⣷⣿
```
**9 characters.** Braille dot patterns create extremely fine-grained density control. Each character is a small grid of dots, so the overall texture is uniquely granular.

**Best for:** High-detail scenes, photographic subjects, fine gradients

---

## Creating Custom Character Sets

The renderer accepts any string of characters ordered from lightest (most transparent) to heaviest (most opaque). The first character should always be a space.

### Rules:
1. **Start with space** — space = transparent, the darkest value
2. **Order by visual density** — lightest character first, densest last
3. **Monospace matters** — all characters must be the same width in monospace fonts
4. **3-15 characters** — fewer = posterized, more = smoother gradients
5. **Unicode works** — any character the browser's monospace font can render

### Examples:
```javascript
// Arrows — creates sense of direction
' →↗↑↖←↙↓↘'

// Stars — sparkle effect
' ·✦✧★✴'

// Math — nerdy aesthetic
' ·∴∵∷≡≣'

// Custom blend
' .·:+#@█'
```

To use a custom charset, modify the `CHARSETS` object in the template and add an option to the dropdown.
