# Nano Banana Generation Guidelines

Adapted from the upstream [GEMINI.md](https://github.com/gemini-cli-extensions/nanobanana/blob/main/GEMINI.md). These guidelines inform how to craft prompts and use the MCP tools effectively.

## Core Principles

### 1. Precise Count Adherence

When the user specifies a count, generate exactly that many images:
- `outputCount: 3` means exactly 3 images
- If no count specified by user, skill default is 3 for `generate_image`
- Never generate fewer images than requested

### 2. Style and Variation Compliance

Always respect user-specified design preferences:
- **styles**: Apply exact artistic styles (watercolor, oil-painting, sketch, photorealistic, etc.)
- **variations**: Implement specific variation types (lighting, angle, color-palette, composition, mood, season, time-of-day)
- Maintain prompt essence while applying stylistic changes
- When multiple styles requested, each image should distinctly represent its assigned style

### 3. Visual Consistency for Stories

When using `generate_story`, maintain strict visual consistency:
- **Color Palette**: Same or similar color schemes across all frames
- **Art Style**: Consistent artistic approach (detail level, shading, line work)
- **Character Design**: Consistent appearances (clothing, proportions, features)
- **Visual Theme**: Same mood and aesthetic throughout
- **Layout**: Similar composition and framing for coherence

### 4. Text in Images

When generating text within images:
- Ensure all text is spelled correctly
- Use proper grammar and punctuation
- Only include text directly related to the prompt
- Make text clearly readable and well-positioned
- Never add unrelated words or content not in the prompt

## Tool-Specific Guidelines

### generate_icon
- Create clean, scalable designs suitable for specified sizes
- Use appropriate platform icon conventions
- Ensure legibility at smaller sizes
- Consider context: app icon vs. favicon vs. UI element

### generate_pattern
- For seamless patterns, ensure perfect tiling without visible seams
- Match requested density (sparse/medium/dense) accurately
- Respect color scheme (mono/duotone/colorful)

### generate_diagram
- Use professional diagramming conventions
- Ensure text labels are clear and properly positioned
- Follow standard symbols for the diagram type (flowchart, architecture, etc.)
- Maintain readability at intended viewing size

### edit_image
- Preserve original image quality and style
- Make only the requested modifications
- Ensure edits look natural and integrated

### restore_image
- Focus on enhancing without altering original intent
- Improve technical quality while preserving historical accuracy
- Remove only specified defects (scratches, tears, etc.)

## Quality Standards

- Generate high-quality images suitable for intended use
- Ensure appropriate resolution and aspect ratios
- Maintain consistent lighting and perspective in multi-image sets
- Balance user specifications with artistic best practices

## File Management

### Output
- Images saved to `./nanobanana-output/` (created automatically)
- Smart filenames derived from prompt text (max 32 chars, underscores)
- Automatic duplicate prevention with counter suffix (`_1`, `_2`, etc.)

### Input File Search Paths (for edit/restore)
1. Current working directory
2. `./images/`
3. `./input/`
4. `./nanobanana-output/`
5. `~/Downloads/`
6. `~/Desktop/`
