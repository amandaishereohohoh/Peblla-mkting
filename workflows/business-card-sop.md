<!-- Peblla Marketing Team Knowledge Base — Auto-generated from group chat learnings -->

# Business Card SOP (Mina Lu)

## Process Overview

```
Request submitted
  → Provide information
    → Select paper material & size
      → Create ClickUp task
        → Design & layout
          → Proofreading confirmation
            → Print order
              → Ship to office
                → Archive & close
```

## Companies

- **Peblla**
- **Rosper**

## Paper Specifications

| Type | Size | Printer |
|------|------|---------|
| **Standard** | 3.5 x 2 inches | EGI Printing |
| **Premium** | 3.3 x 2.16 inches | MOO (moo.com) |

## Required Information

| Field | Notes |
|-------|-------|
| Company | Peblla or Rosper |
| Paper size | Standard or Premium |
| Name | Full name |
| Title | Job title |
| Phone | Include country code |
| Email | Business email |
| Shipping address | Office address for delivery |

## ClickUp Task Title Format

```
[Business Card] Company - Name - Paper Version
```

**Example**: `[Business Card] Peblla - David Kang - Premium`

## Proofreading Checklist

Before printing, verify:

- [ ] Name spelling and capitalization
- [ ] Title accuracy
- [ ] Phone number (with country code)
- [ ] Email address
- [ ] Logo placement and quality
- [ ] Correct size version

**Recipient must reply "Approved" before the print order is placed.**

## Technical Execution (Sigma)

### Template

- **Source file**: `mina_businesscard.pdf` (Peblla standard front, original: David Kang)
- **Mandatory**: Every card must be identical to the template — logo, layout, font, color, size — only replace name/title/phone/email

### Font

- **Roboto Medium** (`Roboto-Medium.ttf`, downloaded to session directory)

### Method

pymupdf redact (white fill) on target areas + `insert_text` for new content

### Coordinate Reference

| Element | Coordinates | Font Size |
|---------|-------------|-----------|
| Name Line 1 | (21.49, 42.60) | 28 |
| Name Line 2 | (21.49, 72.59) | 28 |
| Title | (21.49, 130.27) | 10 |
| Contact Info | Right side (166.97, 106-130) | 8 |

### PDF Page Size

259.92 x 151.92 points
