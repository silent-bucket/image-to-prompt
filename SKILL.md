---
name: image-to-prompt
description: Analyzes a pasted or attached photo in detail — camera angle, shot framing (close-up/medium/wide/etc.), lens/focal length, depth of field, lighting, color grading, and composition — then builds a ready-to-use AI image-generation prompt from it. Asks what the user wants changed (swap an object, change setting/mood/lighting/style) and merges those edits into the final prompt. Use when the user pastes or references a photo and wants an image-gen prompt for Midjourney, Flux, DALL-E, Nano Banana, Stable Diffusion, etc., asks to "recreate this photo but change X," wants the shot's specs reverse-engineered, or asks "what prompt would generate this image."
---

# Image to Prompt

Turn a real photo into an editable AI image-generation prompt: read the shot's own
photographic specs off the image, then let the user override just the parts they
want changed.

## Workflow

### 1. Get the image
If the user hasn't already pasted or attached an image in this conversation, ask
for one before doing anything else. Don't guess at analysis from a text
description alone.

### 2. Analyze the shot in detail
Look at the image directly (not just its subject matter) and work through each
of these dimensions. Use `references/shot-analysis-cheatsheet.md` for the
visual-cue → terminology mappings so wording stays consistent across runs.

- **Subject & composition** — what's in frame, arrangement, rule-of-thirds vs.
  centered, foreground/background relationship
- **Camera angle** — eye-level, low-angle, high-angle, bird's-eye, dutch tilt
- **Shot size** — extreme close-up, close-up, medium, medium-wide, wide,
  establishing
- **Lens / focal length estimate** — inferred from perspective distortion,
  background compression, and framing (e.g. wide ~24mm, standard ~35–50mm,
  portrait ~85mm, telephoto 135mm+)
- **Depth of field** — shallow with bokeh vs. deep focus, plus a suggested
  f-stop range
- **Lighting** — direction, hard vs. soft, color temperature, natural vs.
  artificial, implied time of day
- **Color palette & grading** — dominant colors, saturation, overall mood
- **Style/genre** — editorial, product, cinematic, documentary, snapshot, etc.,
  plus any film grain / digital texture cues

### 3. Present the analysis first
Show this as a labeled breakdown before producing any prompt, so the user can
correct anything you misread (e.g. "that's actually a 35mm, not 85mm look").
Keep it concise — short labeled bullets, not paragraphs.

### 4. Ask what to change and how to format it
Ask the user (via `AskUserQuestion` when it's not already clear from their
message):
- **What to change** — object swaps, setting, mood, lighting, style, or "keep
  it as-is, just give me the prompt"
- **Target format** — generic natural-language prompt (works across
  Flux/DALL-E/Nano Banana/SDXL/Midjourney-without-params), Midjourney-style
  with parameters (`--ar`, `--style raw`, `--v`, `::` weights), or another
  named tool. Always ask this — don't assume a default.

### 5. Merge into one final prompt
Keep every analyzed element the user didn't ask to change; override only the
parts they did. Output exactly one copy-ready prompt in a single code block,
formatted for the chosen target. Don't pad it with hedging or alternates
unless asked.

### 6. Iterate in place
If the user asks for further tweaks, edit the same prompt rather than
re-running the full analysis from scratch.
