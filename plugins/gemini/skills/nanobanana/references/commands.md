# Nanobanana Commands Reference

Full command reference for the nanobanana Gemini CLI extension.

**Command pattern**: Always use `gemini --yolo -p "/command 'description' --flags"` with `--count=3` default.

---

## `/generate` - Text-to-Image

```bash
# Default (3 images)
gemini --yolo -p "/generate 'a sunset over mountains' --count=3"

# With styles
gemini --yolo -p "/generate 'mountain landscape' --count=3 --styles='watercolor,oil-painting,sketch'"

# With preview
gemini --yolo -p "/generate 'portrait of a cat' --count=3 --preview"

# Reproducible
gemini --yolo -p "/generate 'abstract art' --seed=42 --count=3"
```

**Flags:**

| Flag | Values | Description |
|------|--------|-------------|
| `--count` | 1-8 (skill default: **3**) | Number of images |
| `--styles` | comma-separated | Artistic styles |
| `--variations` | comma-separated | Variation types |
| `--format` | `grid`, `separate` | Output layout |
| `--seed` | integer | Reproducible variations |
| `--preview` | flag | Auto-open results |

**Styles:** photorealistic, watercolor, oil-painting, sketch, pixel-art, anime, vintage, modern, abstract, minimalist

**Variations:** lighting, angle, color-palette, composition, mood, season, time-of-day

## `/edit` - Image Modification

```bash
gemini --yolo -p "/edit path/to/image.jpg 'change background to beach scene' --preview"
```

## `/restore` - Photo Enhancement

```bash
gemini --yolo -p "/restore old_photo.jpg 'remove scratches and enhance colors' --preview"
```

## `/icon` - Icon Generation

```bash
gemini --yolo -p "/icon 'coffee cup logo' --sizes='64,128,256,512' --type='app-icon' --corners='rounded' --preview"
```

| Flag | Values |
|------|--------|
| `--sizes` | e.g., "64,128,256,512" |
| `--type` | `app-icon`, `favicon`, `ui-element` |
| `--style` | `flat`, `skeuomorphic`, `minimal`, `modern` |
| `--background` | `transparent`, `white`, `black`, color |
| `--corners` | `rounded`, `sharp` |

## `/pattern` - Seamless Patterns

```bash
gemini --yolo -p "/pattern 'geometric triangles' --type='seamless' --style='geometric' --preview"
```

| Flag | Values |
|------|--------|
| `--type` | `seamless`, `texture`, `wallpaper` |
| `--style` | `geometric`, `organic`, `abstract`, `floral`, `tech` |
| `--density` | `sparse`, `medium`, `dense` |
| `--colors` | `mono`, `duotone`, `colorful` |

## `/story` - Sequential Visual Narratives

```bash
gemini --yolo -p "/story 'seed growing into tree' --steps=4 --type='process' --preview"
```

| Flag | Values |
|------|--------|
| `--steps` | 2-8 |
| `--type` | `story`, `process`, `tutorial`, `timeline` |
| `--style` | `consistent`, `evolving` |
| `--layout` | `separate`, `grid`, `comic` |

## `/diagram` - Technical Diagrams

```bash
gemini --yolo -p "/diagram 'microservices architecture' --type='architecture' --complexity='detailed'"
```

| Flag | Values |
|------|--------|
| `--type` | `flowchart`, `architecture`, `network`, `database`, `wireframe`, `mindmap`, `sequence` |
| `--style` | `professional`, `clean`, `hand-drawn`, `technical` |
| `--complexity` | `simple`, `detailed`, `comprehensive` |

## `/nanobanana` - Natural Language Interface

```bash
gemini --yolo -p "/nanobanana create a logo for my tech startup"
```

Use when the request doesn't map cleanly to a specific command.

---

## Common Sizes

| Use Case | Dimensions | Prompt Hint |
|----------|------------|-------------|
| YouTube thumbnail | 1280x720 | Include "wide 16:9 aspect ratio" in prompt |
| Blog featured image | 1200x630 | Social preview friendly |
| Square social | 1080x1080 | Include "square format" in prompt |
| Twitter/X header | 1500x500 | Include "wide banner" in prompt |
| Vertical story | 1080x1920 | Include "vertical 9:16 aspect ratio" in prompt |

## Model Selection

Default: `gemini-3.1-flash-image-preview` (fast, good quality)

| Model | Quality | Speed |
|-------|---------|-------|
| `gemini-3.1-flash-image-preview` | Good (default) | Fast |
| `gemini-3-pro-image-preview` | Higher quality | Slower |
| `gemini-2.5-flash-image` | Legacy v1 | Fast |

Override inline:
```bash
NANOBANANA_MODEL=gemini-3-pro-image-preview gemini --yolo -p "/generate 'prompt' --count=3"
```
