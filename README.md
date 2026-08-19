# 📸 image-to-prompt

**A reverse-engineering skill for Claude Code** — hand it a photo, it reads the shot back like a photographer would (angle, lens, light, grade) and turns that into a ready-to-use AI image-generation prompt. Then you tell it what to swap, and it merges the edit into one final prompt.

## What it does

Paste or attach a photo and Claude:

1. **Reads the shot's own specs off the image** — not just what's in it, but how it was taken
2. **Shows you the breakdown first**, so you can correct anything it misread before a prompt gets built
3. **Asks what you want to change** — an object, the setting, the mood, the lighting, the style — and what tool you're generating for
4. **Merges your edit into one final prompt**, keeping everything you didn't ask to change

No guessing from a text description — the analysis always starts from the actual pixels.

> **Note:** this rebuilds the *style* of a photo — its angle, lens, light, grade, and composition — into a text prompt for a generation model. It's not meant to reproduce the source image pixel-for-pixel, and text-to-image generation won't give you an exact copy even with a very detailed prompt. If you need to preserve an exact scene and only change one element, look at image-to-image / inpainting tools instead — this skill is for capturing the *look and feel*, then letting you swap in something new.

## The dimensions it reads

| Dimension | What it's reading |
|---|---|
| **Subject & composition** | what's in frame, rule-of-thirds vs. centered, foreground/background |
| **Camera angle** | eye-level, low-angle, high-angle, bird's-eye, dutch tilt |
| **Shot size** | extreme close-up through establishing shot |
| **Lens / focal length** | inferred from perspective distortion and background compression |
| **Depth of field** | shallow with bokeh vs. deep focus, plus a suggested f-stop |
| **Lighting** | direction, hard vs. soft, color temperature, implied time of day |
| **Color palette & grading** | dominant colors, saturation, overall mood |
| **Style / genre** | editorial, product, cinematic, documentary, snapshot, plus grain/texture cues |

The visual-cue → terminology mappings behind each dimension live in [references/shot-analysis-cheatsheet.md](references/shot-analysis-cheatsheet.md), so the wording stays consistent from one photo to the next.

## Install

```bash
git clone https://github.com/silent-bucket/image-to-prompt.git
```

Then point your agent at it. In Claude Code, add to your `CLAUDE.md`:

```markdown
#### /image-to-prompt
Read: /path/to/image-to-prompt/SKILL.md
Turn a photo into an editable AI image-generation prompt — reads the shot's
real specs, then lets you swap in whatever you want changed.
```

Or drop the folder straight into `~/.claude/skills/` and Claude Code will pick it up automatically.

## Usage

Just paste or attach a photo and say what you're after:

```
[paste photo] give me a prompt for this, but make the jacket a black dress
[paste photo] what prompt would generate this image?
[paste photo] recreate this but change it to golden hour lighting
```

Claude will walk through the shot breakdown, ask what to change and which tool you're targeting (Midjourney-style with parameters, or a generic natural-language prompt for Flux/DALL-E/Nano Banana/SDXL), then hand back one copy-ready prompt. Ask for further tweaks and it edits that same prompt rather than starting over.

---

Built by [Juliet Aittomäki](https://github.com/silent-bucket) with Claude.
