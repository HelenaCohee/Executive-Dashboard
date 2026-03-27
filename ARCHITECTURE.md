# Architecture & Design Decisions

## Why This Exists

Enterprise programs generate data across dozens of tools. Leadership doesn't want to log into Airtable, then Workday, then the LMS, then the survey platform to understand program health. They want one view that tells them: are we on track, what changed, and what are we doing about it.

This dashboard consolidates all of that into a single, narrative-driven page.

## Design Principles

### 1. Story First, Data Second
Every section answers a question leadership is actually asking. Not "here's a chart" but "here's what this means for our headcount plan." Data without narrative is noise.

### 2. Zero Friction Deployment
No build step. No server. No database. One HTML file that loads from anywhere — email attachment, Slack link, GitHub Pages, or a shared drive. This was a deliberate constraint to maximize adoption.

### 3. Executive-Optimized Visual Design
Dark theme reduces eye strain in conference rooms. Large KPI numbers are readable from across a table. Charts use a limited color palette so they're distinguishable on projectors.

### 4. Progressive Disclosure
Top-level KPIs are immediately visible. Detailed breakdowns are below the fold. Executives who want the summary get it in 10 seconds. Those who want depth can scroll.

## Data Flow (Production)

```
┌─────────────┐     ┌──────────────┐     ┌───────────────┐
│   Workday   │────▶│              │     │               │
│   (HRIS)    │     │   Airtable   │────▶│   Dashboard   │
├─────────────┤     │  (Central    │     │   (HTML +     │
│     LMS     │────▶│    Hub)      │     │   Chart.js)   │
│  Platform   │     │              │     │               │
├─────────────┤     │  - ETL sync  │     │  - KPI cards  │
│   Survey    │────▶│  - Schema    │     │  - Charts     │
│  Platform   │     │  - Formulas  │     │  - Narrative  │
├─────────────┤     │  - API       │     │  - Trends     │
│  Finance /  │────▶│              │     │               │
│   Budget    │     └──────────────┘     └───────────────┘
└─────────────┘

Sync frequency: Every 4 hours via Airtable automations
Manual refresh: Before quarterly leadership reviews
```

## Data Model

### KPI Metrics
| Metric | Source | Refresh Rate |
|--------|--------|-------------|
| Total Participants | LMS enrollment records | 4 hours |
| Completion Rate | LMS completion flags | 4 hours |
| NPS Score | Survey platform API | Weekly |
| Budget Utilization | Finance Airtable base | Daily |
| Headcount | Workday HRIS export | Daily |
| Facilitator Ratings | Post-session surveys | Per session |

### Chart Data
All chart data is aggregated in Airtable using rollup fields and formula columns, then exported as JSON for the dashboard. In this demo version, representative sample data is hardcoded.

## File Structure

The entire dashboard is a single `index.html` file. This is intentional — it maximizes portability and minimizes deployment complexity. The tradeoff is a larger file size (~30KB), which is acceptable for an internal tool.

External dependencies (loaded via CDN):
- Chart.js 4.x — chart rendering
- Google Fonts (DM Sans) — typography

No other dependencies. No build tools. No framework.
