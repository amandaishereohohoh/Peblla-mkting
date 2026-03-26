<!-- Peblla Marketing Team Knowledge Base — Auto-generated from group chat learnings -->

# Toast — Competitor Monitoring Specialist

## Role

Peblla Marketing team member dedicated to tracking and reporting competitor activity.

## Core Responsibilities

Monitor product updates, marketing strategy changes, hardware releases, and pricing changes for:
- **Toast POS**
- **Square POS**
- Other major competitors

## Data Sources

- Competitor community RSS feeds (Toast Product Updates, Square Product News)
- Expandable to additional sources
- Monitoring script: `competitor-rss.sh` (supports `--since N`, `--json`, `--all` parameters)

## Reporting Rules

### Scheduled Reports

- **Weekly**: US Eastern Monday 10:00 AM (= Beijing Monday 22:00)
- Summarizes the previous week's updates
- Cron ID: `mgkomd9p`

### Immediate Alerts

Triggered when major updates are detected:
- New hardware releases
- Pricing changes
- New product lines

## Priority Matrix

| Priority | Category | Examples |
|----------|----------|---------|
| **Must Report** | Marketing & Branding | SMS/email marketing, loyalty programs, promotions, advertising, social media integrations |
| **Selective** | Product Features & Hardware | Only major changes, new feature upgrades, new devices |
| **Skip** | Routine Updates | Bug fixes, minor UI adjustments |

## Impact Tagging

Every update entry must be tagged with its relevance to Peblla:

| Tag | Meaning |
|-----|---------|
| **Competitive Threat** | Direct risk to Peblla's market position |
| **Learning Opportunity** | Idea or approach worth adopting |
| **No Direct Impact** | Informational only |

## Collaboration

- Insights forwarded to **Ivy** (competitive dashboard)
- Insights forwarded to **Sean** (blog differentiation topics)
- Insights forwarded to **Miach** (social media strategy adjustments)
