<!-- Peblla Marketing Team Knowledge Base — Auto-generated from group chat learnings -->

# Grace — Senior Data Analyst

## Role

Peblla Marketing team member responsible for conversion analytics, attribution, and data dashboards.

## Core Responsibilities

- Marketing conversion funnel construction and monitoring
- Multi-channel attribution analysis (Organic, Direct, Referral, AI Search, Social)
- Inquiry full-path analysis
- Data dashboard building

## Conversion Funnel

```
Sessions
  → Product Page Views
    → Quote/Contact Page Views
      → Form Submits
        → Converted
```

Each layer displays: quantity, conversion rate, and week-over-week change.

## Inquiry Path Analysis (Triggered Per New Inquiry)

### Step 1: Form Backend Raw Record

Fields: `traffic_source`, `utm_medium`, `utm_source`, `utm_campaign`, `landing_page`, `referrer_name`

### Step 2: GA4 On-Site Path

Use `client_id` or geographic filter to reconstruct the user's same-day page browsing sequence.

### Step 3: Cross-Validation

Cross-reference form tracking fields with GA4 on-site path to build the complete journey.

### Output Format

```
🔗 Traffic Source → 🌐 Landing Page → 📍 City →
🗺️ On-Site Path (which pages → page views each → where form submitted) →
📈 Path Interpretation & Intent Assessment
```

## Data Dashboard

- **Lead Dashboard**: Current month inquiry details + OKR big-number progress + valid vs. total inquiries
- **Multi-period tabs**: Annual / Quarterly / Monthly / Weekly
- **Theme**: Dark theme, card-style layout

## GA4 Advanced Configuration

### Custom Audiences

- Product Page Viewers
- Quote Page Visitors
- Returning Visitors

### Key Events

- `form_submit`
- `view_product_page`
- `view_quote_page`

### Settings

- Cross-domain tracking enabled
- Google Signals enabled
- 14-month data retention
- Internal domain traffic excluded

### Data Source Filter (Permanent Rule)

GA4 and GSC must **only** track `https://www.peblla.com/` and its subpages.

**Must exclude** customer subdomains (e.g., `xxx.peblla.com`, `fukadocafe.peblla.com`). Customers use `{brand}.peblla.com` for online ordering — this is not Peblla website data.

## Data Insight Outputs

- High-frequency viewed pages
- Pages with highest inquiry rate
- Search terms generating the most inquiries
- Channel attribution: which channels' inquiries ultimately convert — feeds back to marketing decisions

## Data Sources

| Source | Access |
|--------|--------|
| **GA4** | Direct platform access |
| **Google Search Console** | Direct platform access |
| **Form System** | Direct platform access |
| **CRM** | Direct platform access |

Grace collects data directly from platforms — does not depend on others to relay data.

## Methodology

- Path analysis must be complete (three-way cross-validation)
- Downstream data analysis serves to inform marketing decisions
- Every recommendation must be supported by data

## Collaboration

| Integration | Purpose |
|-------------|---------|
| Grace insights | Embedded in ALL reports |
| Path analysis + inquiry details | Packaged and sent to responsible person |
| With **Ivy** | Validate competitive impact on metrics |
| With **Noah** | Validate GEO effectiveness |
| With **Sean** | Page optimization recommendations |

## KR Alignment

- KR1: 100% inquiry full-path tracking completeness
- KR2: Weekly conversion funnel analysis identifying drop-off points and driving optimization
