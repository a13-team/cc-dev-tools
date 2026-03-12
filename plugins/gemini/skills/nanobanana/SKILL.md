---
name: nanobanana
version: 2.1.0
description: This skill should be used when the user wants to generate, create, make, draw, design, edit, or restore any image or visual content. Trigger phrases include "generate an image", "create an illustration", "make an icon", "draw a diagram", "design a pattern", "edit this photo", "restore a photo", "use nanobanana", "nano banana", "make a thumbnail", "featured image", "visual asset", "graphic", "artwork". Covers all image generation and editing requests using the Nano Banana extension for Gemini CLI.
allowed-tools: Bash(gemini:*)
---

# Nano Banana Image Generation

Generate professional images via the Gemini CLI's nanobanana extension.

## When to Use This Skill

ALWAYS use this skill when the user:
- Asks for any image, graphic, illustration, or visual
- Wants a thumbnail, featured image, or banner
- Requests icons, diagrams, or patterns
- Asks to edit, modify, or restore a photo
- Uses words like: generate, create, make, draw, design, visualize

Do NOT attempt to generate images through any other method.

---

## Health Check (Run Before First Use)

```bash
# 1. Check Gemini CLI installed
command -v gemini && echo "OK: gemini found" || echo "FAIL: gemini not found"

# 2. Check nanobanana extension installed with valid config
ls ~/.gemini/extensions/nanobanana/gemini-extension.json >/dev/null 2>&1 && echo "OK: nanobanana installed" || echo "FAIL: nanobanana not installed or config missing"

# 3. Check authentication (API key OR Vertex AI)
AUTH="${NANOBANANA_AUTH_METHOD:-api_key}"
if [ "$AUTH" = "vertex_ai" ]; then
  [ -n "$GCP_PROJECT_ID" ] && [ -n "$GCP_REGION" ] && echo "OK: Vertex AI auth ($GCP_PROJECT_ID / $GCP_REGION)" || echo "FAIL: Vertex AI requires GCP_PROJECT_ID and GCP_REGION"
elif [ "$AUTH" = "auto" ]; then
  [ -n "$NANOBANANA_API_KEY" ] || [ -n "$GEMINI_API_KEY" ] || [ -n "$GCP_PROJECT_ID" ] && echo "OK: auth auto" || echo "FAIL: Set NANOBANANA_API_KEY, GEMINI_API_KEY, or GCP_PROJECT_ID+GCP_REGION"
else
  ([ -n "$NANOBANANA_API_KEY" ] || [ -n "$GEMINI_API_KEY" ]) && echo "OK: API key set" || echo "FAIL: NANOBANANA_API_KEY or GEMINI_API_KEY not set"
fi
echo "AUTH_METHOD=$AUTH"
```

| Check | If FAIL | Fix |
|-------|---------|-----|
| Gemini CLI not found | Install it | `npm install -g @google/gemini-cli` |
| Nanobanana not installed or config missing | Reinstall extension | `rm -rf ~/.gemini/extensions/nanobanana && gemini extensions install https://github.com/gemini-cli-extensions/nanobanana` (interactive — run in terminal) |
| API key not set | Set env var | Get key from [Google AI Studio](https://aistudio.google.com/apikey), then `export GEMINI_API_KEY=your_key` |
| Vertex AI missing vars | Set GCP env vars | `export NANOBANANA_AUTH_METHOD=vertex_ai GCP_PROJECT_ID=your-project GCP_REGION=us-central1` |
| Generative Language API disabled | Enable in GCP console | Visit `https://console.developers.google.com/apis/api/generativelanguage.googleapis.com` and enable for your project |

**If any check fails**, stop and guide the user. Do not run nanobanana commands until all checks pass.

For detailed authentication setup, see `references/troubleshooting.md`.

---

## Command Pattern

**Always use `--yolo -p` flags** for headless execution (auto-approve + non-interactive mode):

```bash
gemini --yolo -p "/command 'description' --flags"
```

**Why both flags?** `--yolo` auto-approves tool actions. `-p` forces non-interactive (headless) mode. Both are required in Claude Code's non-terminal environment.

**Skill default: `--count=3`** — always generate 3 images unless user specifies otherwise.

---

## Command Selection

| User Request | Command |
|--------------|---------|
| "generate an image of..." | `/generate` |
| "create an app icon" | `/icon` |
| "draw a flowchart of..." | `/diagram` |
| "fix this old photo" | `/restore` |
| "edit this image" | `/edit` |
| "create a repeating texture" | `/pattern` |
| "make a comic strip" | `/story` |
| General/unclear request | `/nanobanana` |

For full command flags and options, see `references/commands.md`.

---

## Quick Examples

```bash
# Text-to-image (default 3 images)
gemini --yolo -p "/generate 'a sunset over mountains' --count=3"

# Edit existing image
gemini --yolo -p "/edit path/to/image.jpg 'change background to beach scene'"

# Generate app icons
gemini --yolo -p "/icon 'coffee cup logo' --sizes='64,128,256,512' --type='app-icon'"

# Technical diagram
gemini --yolo -p "/diagram 'microservices architecture' --type='architecture'"

# Natural language (unclear request)
gemini --yolo -p "/nanobanana create a logo for my tech startup"

# Override model for higher quality
NANOBANANA_MODEL=gemini-3-pro-image-preview gemini --yolo -p "/generate 'prompt' --count=3"
```

---

## Output Location

All generated images are saved to `./nanobanana-output/` in the current directory.

## Presenting Results

After generation completes:
1. List contents of `./nanobanana-output/` to find generated files
2. Present the most recent image(s) to the user using the Read tool
3. Offer to regenerate with variations if needed

---

## Refinements and Iterations

When the user asks for changes:
- **"Try again" / "Give me options"**: Regenerate with `--count=3`
- **"Make it more [adjective]"**: Adjust prompt and regenerate
- **"Edit this one"**: Use `gemini --yolo -p "/edit nanobanana-output/filename.png 'adjustment'"`
- **"Different style"**: Add `--styles="requested_style"` to the command

## Prompt Tips

1. **Be specific**: Include style, mood, colors, composition details
2. **Add "no text"**: If you don't want text rendered in the image
3. **Reference styles**: "editorial photography", "flat illustration", "3D render", "watercolor"
4. **Specify aspect ratio**: Include "wide 16:9" or "square" in the prompt description

---

For error troubleshooting, see `references/troubleshooting.md`.
