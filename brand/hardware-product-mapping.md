<!-- Peblla Marketing Team Knowledge Base — Auto-generated from group chat learnings -->

# Peblla Hardware Product — SUNMI Mapping

## Overview

All Peblla hardware products use SUNMI devices as their base. When producing any marketing images, videos, or UI mockups featuring Peblla hardware, the corresponding SUNMI model must be used as the visual reference.

**SUNMI Official Material Library**: https://www.sunmi.com/en/materials/

## Product Mapping Table

| Peblla Product | SUNMI Model | Priority | Use Case |
|----------------|-------------|----------|----------|
| **POS** (Desktop Terminal) | SUNMI D3 Pro | Main | Restaurant front counter checkout |
| **Kiosk** (Self-Service) | SUNMI Flex 3 | Main | Self-service ordering/checkout |
| **Kiosk** (Self-Service) | SUNMI K2 | Main | Self-service ordering/checkout |
| **Kiosk** (Self-Service) | SUNMI K2 Mini | Alt | Compact self-service |
| **MPOS** (Handheld POS) | SUNMI M3 | Main | Server handheld ordering/scanning |
| **MPOS** (Handheld POS) | SUNMI L3 | Alt | Server handheld ordering |
| **Tablet** (Tablet Ordering) | SUNMI Cpad | Main | Tabletop ordering/display |

## Local Asset Directory

All product images (multi-angle, SUNMI logo removed) are stored in:

```
./sunmi/
```

## Mandatory Rules

### 1. SUNMI Logo Removal

**All output must have the SUNMI logo completely removed** and replaced with Peblla brand identity. This is a non-negotiable requirement.

### 2. Visual Consistency

When generating product images:
- Use the correct SUNMI model for the corresponding Peblla product
- Maintain accurate proportions, form factor, and screen layout
- Reference multi-angle shots from the `./sunmi/` directory

### 3. Safer Review Required

**Every image or video must pass Safer review before publishing.** The review checks:
- Hardware appearance matches the correct SUNMI prototype (shape, proportions, screen)
- SUNMI logo is completely removed
- Peblla brand elements are correctly applied
- Image quality, composition, and color meet standards
- No inappropriate content or copyright risks

### 4. Reference Image Mode

When generating product scene images with AI:
- **Always use reference image mode** (gemini-edit.py, mode B)
- Pull the base product image from `./sunmi/` directory
- Pass it as a reference image with a scene description prompt
- This ensures the hardware device appearance stays consistent

See [Image Generation Rules](../methodology/image-generation-rules.md) for details.
