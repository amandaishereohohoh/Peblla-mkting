<!-- Peblla Marketing Team Knowledge Base — Auto-generated from group chat learnings -->

# Dashboard Hosting Architecture

## Overview

All marketing dashboards and reports are hosted on a private infrastructure using Cloudflare R2 (object storage) + Cloudflare Worker (access control).

**Domain**: `mkt-dashboard.peblla.com`

## Architecture

```
Local HTML generation
  → Upload script (boto3)
    → Cloudflare R2 (object storage)
      → Cloudflare Worker (request interceptor)
        → URL path prefix determines access level
          → Serve content or require authentication
```

## Three-Directory Access Model

| Directory | Access Level | Description |
|-----------|-------------|-------------|
| `private/` | OTP email verification (company domain only) | Internal dashboards, reports |
| `partner/` | OTP email verification (company + partner domains) | Shared partner content |
| `public/` | Anyone can access | Public-facing content |

**Default rule**: If unsure which directory to use, **always use `private/`**.

### Mandatory Rule

**All dashboards and reports must be uploaded to the `private/` directory** (with authentication protection).

## OTP Authentication Flow

```
User visits dashboard URL
  → No session cookie found
    → Prompt for email address
      → Validate email domain against whitelist
        → Send 6-digit OTP via email service
          → User enters OTP
            → Verification passed
              → Set session cookie (7-day expiry)
                → Serve content
```

## Security Measures

| Measure | Details |
|---------|---------|
| OTP expiry | 5 minutes |
| Transport | HTTPS only |
| Storage access | R2 is not publicly accessible; only the Worker can proxy requests |
| Analytics isolation | Internal tool domains excluded from GA4 to prevent data pollution |
| Session duration | 7-day cookie |

## Upload Process

```bash
bash upload.sh <local-file> <r2-path>
```

- Automatically detects Content-Type
- Uses boto3 for S3-compatible upload

## Auto-Update Pattern

```
Data changes
  → Script generates HTML
    → upload.sh pushes to R2
      → Team accesses the same URL for latest data
```

Same URL always serves the most current version of the dashboard.
