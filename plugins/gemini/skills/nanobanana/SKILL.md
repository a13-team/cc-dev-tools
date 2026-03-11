---
name: nanobanana
version: 1.0.0
description: This skill should be used when the user wants to generate, edit, or manipulate images using Google's Nano Banana models via Gemini CLI. Trigger phrases include "generate image", "create image", "edit image", "restore photo", "make icon", "create pattern", "draw diagram", "visual story", "nanobanana", "image generation", "text to image", or when users request any image creation, editing, or visual content generation tasks. Inherits Gemini CLI conventions for headless execution.
---

# Nanobanana: Image Generation via Gemini CLI

This skill extends the Gemini plugin with image generation, editing, and manipulation capabilities using the [Nanobanana extension](https://github.com/gemini-cli-extensions/nanobanana).

---

## Prerequisites

1. **Gemini CLI** installed and configured (see parent `gemini` skill)
2. **Nanobanana extension** installed:
   ```bash
   gemini extensions install https://github.com/gemini-cli-extensions/nanobanana
   ```
3. **API Key**: Set `NANOBANANA_API_KEY` environment variable (get from [Google AI Studio](https://aistudio.google.com/apikey))

---

## CRITICAL: Headless Execution via Gemini CLI

**All nanobanana commands run through Gemini CLI in headless mode using `-p`.**

Nanobanana slash commands are Gemini CLI extension commands. In Claude Code's non-interactive environment, pass them as the prompt:

```bash
# General pattern
gemini -e nanobanana -p '/command "description" --flags'
```

**Why `-e nanobanana`?** Ensures only the nanobanana extension is loaded, avoiding unnecessary tool conflicts.

**Why `-p`?** Required for headless/non-interactive mode in Claude Code (see parent gemini skill).

---

## Supported Models

Pass via inline env var in the command:

| Model | Description |
|-------|-------------|
| `gemini-3.1-flash-image-preview` | Default (v1.0.11+), fast image generation |
| `gemini-3-pro-image-preview` | Pro variant, higher quality |
| `gemini-2.5-flash-image` | v1, legacy |

```bash
# Default model (no override needed)
gemini -e nanobanana -p '/generate "prompt"'

# Use pro model for higher quality
NANOBANANA_MODEL=gemini-3-pro-image-preview gemini -e nanobanana -p '/generate "prompt"'

# Use legacy v1 model
NANOBANANA_MODEL=gemini-2.5-flash-image gemini -e nanobanana -p '/generate "prompt"'
```

---

## Commands Reference

### `/generate` - Text-to-Image Creation

Create single or multiple images from text descriptions.

**Skill default: `--count=3`** — always use `--count=3` unless the user specifies a different number.

```bash
# Basic (default 3 images)
gemini -e nanobanana -p '/generate "a sunset over mountains" --count=3'

# With styles (3 images, one per style)
gemini -e nanobanana -p '/generate "mountain landscape" --count=3 --styles="watercolor,oil-painting,sketch"'

# User requested specific count
gemini -e nanobanana -p '/generate "portrait of a cat" --count=5 --variations="lighting,angle" --preview'

# Reproducible with seed
gemini -e nanobanana -p '/generate "abstract art" --seed=42 --count=3'
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

**Available Styles:** photorealistic, watercolor, oil-painting, sketch, pixel-art, anime, vintage, modern, abstract, minimalist

**Available Variations:** lighting, angle, color-palette, composition, mood, season, time-of-day

---

### `/edit` - Image Modification

Modify existing images with natural language instructions.

```bash
gemini -e nanobanana -p '/edit path/to/image.jpg "change background to beach scene" --preview'
gemini -e nanobanana -p '/edit portrait.png "add sunglasses and a hat"'
```

---

### `/restore` - Photo Enhancement

Restore and improve old or damaged photos.

```bash
gemini -e nanobanana -p '/restore old_photo.jpg "remove scratches and enhance colors" --preview'
gemini -e nanobanana -p '/restore damaged.png "fix color fading and sharpen details"'
```

---

### `/icon` - Icon Generation

Create app icons, favicons, and UI elements in multiple sizes.

```bash
# App icon set
gemini -e nanobanana -p '/icon "coffee cup logo" --sizes="64,128,256,512" --type="app-icon" --preview'

# Favicon
gemini -e nanobanana -p '/icon "letter A" --type="favicon" --style="minimal" --background="transparent"'
```

**Flags:**

| Flag | Values | Description |
|------|--------|-------------|
| `--sizes` | comma-separated pixels | e.g., "64,128,256,512" |
| `--type` | `app-icon`, `favicon`, `ui-element` | Icon type |
| `--style` | `flat`, `skeuomorphic`, `minimal`, `modern` | Visual style |
| `--format` | `png`, `jpeg` | Output format |
| `--background` | `transparent`, `white`, `black`, color | Background |
| `--corners` | `rounded`, `sharp` | Corner style |

---

### `/pattern` - Seamless Patterns

Generate backgrounds, textures, and wallpapers.

```bash
# Geometric pattern
gemini -e nanobanana -p '/pattern "geometric triangles" --type="seamless" --style="geometric" --preview'

# Organic texture
gemini -e nanobanana -p '/pattern "marble texture" --type="texture" --density="dense" --colors="mono"'
```

**Flags:**

| Flag | Values | Description |
|------|--------|-------------|
| `--size` | e.g., "256x256" | Tile dimensions |
| `--type` | `seamless`, `texture`, `wallpaper` | Pattern type |
| `--style` | `geometric`, `organic`, `abstract`, `floral`, `tech` | Visual style |
| `--density` | `sparse`, `medium`, `dense` | Element density |
| `--colors` | `mono`, `duotone`, `colorful` | Color scheme |
| `--repeat` | `tile`, `mirror` | Repeat mode |

---

### `/story` - Sequential Visual Narratives

Create step-by-step visual sequences.

```bash
# Process visualization
gemini -e nanobanana -p '/story "seed growing into tree" --steps=4 --type="process" --preview'

# Comic-style story
gemini -e nanobanana -p '/story "a day in the life of a robot" --steps=6 --layout="comic" --style="consistent"'
```

**Flags:**

| Flag | Values | Description |
|------|--------|-------------|
| `--steps` | 2-8 | Number of sequential images |
| `--type` | `story`, `process`, `tutorial`, `timeline` | Narrative type |
| `--style` | `consistent`, `evolving` | Visual consistency |
| `--layout` | `separate`, `grid`, `comic` | Output layout |
| `--transition` | `smooth`, `dramatic`, `fade` | Between-step transition |
| `--format` | `storyboard`, `individual` | Output format |

---

### `/diagram` - Technical Diagrams

Generate flowcharts, architecture diagrams, and technical visuals.

```bash
# Architecture diagram
gemini -e nanobanana -p '/diagram "microservices architecture" --type="architecture" --complexity="detailed"'

# Flowchart
gemini -e nanobanana -p '/diagram "user authentication flow" --type="flowchart" --style="professional"'

# Database schema
gemini -e nanobanana -p '/diagram "e-commerce database schema" --type="database" --annotations="detailed"'
```

**Flags:**

| Flag | Values | Description |
|------|--------|-------------|
| `--type` | `flowchart`, `architecture`, `network`, `database`, `wireframe`, `mindmap`, `sequence` | Diagram type |
| `--style` | `professional`, `clean`, `hand-drawn`, `technical` | Visual style |
| `--layout` | `horizontal`, `vertical`, `hierarchical`, `circular` | Layout direction |
| `--complexity` | `simple`, `detailed`, `comprehensive` | Detail level |
| `--colors` | `mono`, `accent`, `categorical` | Color scheme |
| `--annotations` | `minimal`, `detailed` | Annotation level |

---

### `/nanobanana` - Natural Language Interface

Open-ended command for flexible image requests without specific command syntax.

```bash
gemini -e nanobanana -p '/nanobanana create a logo for my tech startup'
gemini -e nanobanana -p '/nanobanana I need 5 different versions of a cat illustration in different art styles'
```

Use this when the request doesn't map cleanly to a specific command.

---

## How to Use This Skill

When the user requests image generation or manipulation:

**Step 1: Identify the command**
- Image creation → `/generate`
- Edit existing image → `/edit`
- Restore/enhance photo → `/restore`
- App icons/favicons → `/icon`
- Patterns/textures → `/pattern`
- Sequential visuals → `/story`
- Technical diagrams → `/diagram`
- Unclear/flexible → `/nanobanana`

**Step 2: Build the command**
```bash
gemini -e nanobanana -p '/command "description" --relevant-flags'
```

**Step 3: Execute via Bash tool**

Always run in the user's project directory. Output files are saved to the current working directory with auto-generated filenames.

---

## Error Handling

| Error | Cause | Fix |
|-------|-------|-----|
| Extension not found | Not installed | `gemini extensions install https://github.com/gemini-cli-extensions/nanobanana` |
| API key missing | `NANOBANANA_API_KEY` not set | Set env var with Google AI Studio key |
| Model unavailable | Invalid model ID | Check `NANOBANANA_MODEL` value |
| Generation failed | Prompt safety filter | Rephrase the prompt |

---

## Tips

1. **Use `--preview`** to auto-open generated images
2. **Use `--count`** for multiple variations in one pass
3. **Use `--seed`** for reproducible results
4. **Combine `--styles` with `--variations`** for diverse outputs
5. **Use `/nanobanana`** when unsure which specific command to use
6. **Pro model** (`gemini-3-pro-image-preview`) gives higher quality but is slower
