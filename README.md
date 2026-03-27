# Executive Program Dashboard

An interactive, single-page executive dashboard built for leadership reporting on enterprise onboarding programs. Visualizes program health, operational metrics, financial performance, and strategic evolution across quarters.

🔗 **[View Live Demo →](https://helenacohee.github.io/executive-dashboard/)**

![Dashboard Preview](preview.png)

---

## Overview

This dashboard was designed to solve a specific problem: leadership wanted a single view of program health that didn't require logging into 5 different tools or waiting for someone to pull a report. Instead of building another slide deck, I built an interactive dashboard that updates in real-time and tells a story.

**Key features:**
- Executive-level KPI cards with trend indicators
- Interactive charts (participation trends, satisfaction scores, budget tracking)
- Quarterly comparison and program evolution timeline
- Strategic narrative sections that frame data as decisions, not just numbers
- Fully responsive — works on desktop, tablet, and mobile
- Zero backend dependencies — runs as a standalone HTML file

## Architecture

```
executive-dashboard/
├── index.html          # Complete dashboard (single-file, self-contained)
├── README.md           # This file
├── preview.png         # Dashboard screenshot
└── docs/
    ├── ARCHITECTURE.md # Design decisions and data flow
    └── DATA_MODEL.md   # How the data sources connect
```

### Design Decisions

**Why a single HTML file?**
The audience is executives who need to open a link and immediately see the data. No build step, no npm install, no server. One file, one URL, zero friction. Chart.js is loaded from CDN.

**Why not Looker/Tableau?**
Those tools are great for analysts, but leadership wanted something that read more like a narrative than a data explorer. This dashboard tells a story: here's where we are, here's what changed, here's what it means, here's what we're doing about it.

**Data flow:**
In production, data feeds from Airtable (program data), Workday (headcount), LMS (completions), and survey platforms (NPS/satisfaction). This demo uses representative sample data.

```
Workday (HRIS) ──┐
LMS Platform ────┤──→ Airtable (central hub) ──→ Dashboard
Survey Tool ─────┤
Finance Data ────┘
```

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Frontend | HTML5, CSS3 (custom properties, grid, flexbox) |
| Charts | Chart.js 4.x via CDN |
| Typography | DM Sans (Google Fonts) |
| Hosting | GitHub Pages (static) |
| Design | Dark theme, executive-optimized, responsive |

## Running Locally

No dependencies. Just open the file:

```bash
# Clone the repo
git clone https://github.com/HelenaCohee/executive-dashboard.git
cd executive-dashboard

# Open in browser
open index.html
```

## Customization

The dashboard uses CSS custom properties for theming. To change colors, update the `:root` variables at the top of the file:

```css
:root {
  --bg: #0a0b0f;
  --surface: #12141a;
  --accent: #6366f1;
  /* ... */
}
```

Data is stored in JavaScript objects at the bottom of the file. To update metrics, modify the data arrays passed to each Chart.js instance.

## Context

Built as part of a quarterly retrospective process for an enterprise onboarding program serving 400–550 new hires per quarter. The dashboard replaced a 15-slide PowerPoint deck and reduced leadership reporting prep time from 4 hours to 30 minutes.

## License

MIT

---

**Built by [Helena Cohee](https://linkedin.com/in/helenabcohee)** — Senior Program Manager | AI Enablement & Operations
