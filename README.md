# Pats-ASCII-Scroll

Turn any scene description into a scroll-driven ASCII video parallax effect.

Describe a scene. The agent generates an image (Nanobanana), converts it to video (Veo 3.1), and renders it as real-time WebGL2 ASCII art tied to your scroll position. Dark pixels become transparent. The result floats on your page like a living ASCII illustration.

## Install

```bash
npx skills.sh install Patonchain/Pats-ASCII-Scroll
```

Or clone manually:
```bash
git clone https://github.com/Patonchain/Pats-ASCII-Scroll.git ~/.claude/skills/Pats-ASCII-Scroll
```

## Requirements

- **GEMINI_API_KEY** — for automated image + video generation. Get one at [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
- Works without the key in manual mode (provides optimized prompts for you to generate externally)

## Usage

```
/Pats-ASCII-Scroll mystical stone pillars with climbing vines and blooming flowers
```

The agent will:
1. Design the scene (composition, color, animation arc)
2. Generate an image via Nanobanana (Gemini 2.5 Flash Image)
3. Convert to video via Veo 3.1 (image-to-video)
4. Build a complete HTML file with WebGL2 ASCII rendering
5. Serve it locally for preview

## Layouts

**Two-pillar** — left/right ASCII borders flanking center content. Best for columns, trees, vines, architectural elements.

**Full-width** — ASCII art covers the entire viewport as a background. Best for landscapes, skies, abstract patterns.

## How It Works

The renderer uses a WebGL2 fragment shader that:
- Divides the canvas into a character grid
- Samples the video texture at each cell position
- Maps luminance to a character from an atlas texture
- Boosts color saturation for vivid output
- Renders dark pixels as fully transparent

Video playback is tied to scroll position with lerp smoothing — scroll down to play forward, scroll up to reverse.

## Character Sets

Switch between 8 built-in character sets via the dropdown:

| Set | Characters | Feel |
|-----|-----------|------|
| Classic | `.,:;+*?%S#@` | Smooth, detailed |
| Blocks | `░▒▓█` | Retro, chunky |
| Minimal | `.-=+#` | Clean, understated |
| Dense | `.:-=+*#%@` | Punchy mid-range |
| Dots | `·:;o*O@` | Organic, soft |
| Binary | `01` | Digital, stark |
| Slashes | `/|\-=*#` | Textured, directional |
| Braille | `⠁⠃⠇⡇⣇⣧⣷⣿` | Fine-grained, photographic |

## Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| `SCROLL_HEIGHT` | `400vh` | Total scroll distance |
| `LERP_FACTOR` | `0.08` | Scroll smoothness (lower = more lag) |
| `DARK_THRESHOLD` | `0.04` | Luminance below this = transparent |
| `SATURATION` | `2.5` | Color saturation multiplier |
| `BRIGHTNESS` | `1.4` | Brightness multiplier |
| `FONT_SIZE` | `10` | Character cell height in px |
| `PILLAR_WIDTH` | `28vw` | Width of each pillar (two-pillar only) |

## License

MIT
