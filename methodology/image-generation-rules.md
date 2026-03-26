<!-- Peblla Marketing Team Knowledge Base — Auto-generated from group chat learnings -->

# Image Generation Rules

## Core Rule: Reference Image Mode (Mandatory)

**All AI image generation must use reference image mode** (also called "pad image" / "cushion image" mode).

This means using `gemini-edit.py` in **Mode B** (image editing with reference), never pure text-to-image generation.

## Workflow

```
1. Find the correct product asset from ./sunmi/ directory
2. Pass it as a reference image to gemini-edit.py
3. Include a scene description prompt
4. Generate the scene image
```

## Why Reference Images Are Required

The purpose is to **guarantee visual consistency** of the main subject (POS terminal, kiosk, MPOS handheld, etc.). Without a reference image, AI will generate hardware devices with incorrect proportions, wrong screen layouts, or entirely different form factors.

## Asset Directory

```
./sunmi/
```

Contains multi-angle product images for each SUNMI model, with SUNMI logos already removed.

## Product-to-Model Mapping

| Peblla Product | SUNMI Model | Priority |
|----------------|-------------|----------|
| POS | D3 Pro | Main |
| Kiosk | Flex 3 / K2 | Main |
| Kiosk | K2 Mini | Alt |
| MPOS | M3 | Main |
| MPOS | L3 | Alt |
| Tablet | Cpad | Main |

See [Hardware Product Mapping](../brand/hardware-product-mapping.md) for complete details.

## Tool

**gemini-edit.py** — Mode B (image editing with reference image)

### Usage Pattern

```python
# Conceptual flow:
# 1. Load reference image from ./sunmi/{model}/{angle}.png
# 2. Provide scene description prompt
# 3. Gemini generates scene with the hardware accurately depicted
```

## Post-Generation Review

All generated images must pass through **Safer review** before publishing:

- Hardware appearance matches the correct SUNMI prototype
- SUNMI logo completely removed
- Peblla brand elements correctly applied
- Image quality, composition, and color meet standards

See [Review Workflow](../team/review-workflow.md) for the full review process.
