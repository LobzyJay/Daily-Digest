# Daily Digest

A dark-first, Linear-inspired productivity dashboard built as a single-page application. Designed for job seekers, freelancers, and professionals who want a unified command center for their daily workflow.

![Daily Digest](https://img.shields.io/badge/version-1.0.0-black) ![License](https://img.shields.io/badge/license-MIT-blue) ![Deploy](https://img.shields.io/badge/deploy-GitHub%20Pages-green)

## Overview

Daily Digest consolidates mail, job listings, tasks, news, calendar, networking leads, and cold email drafting into a single bento-grid dashboard with a **Quiet Mode** for focused work.

### Key Features

- **Bento Grid Layout** — 12-column responsive grid with 8 card modules
- **Quiet Mode** — Mask-reveal transition to a minimal focus view with customizable widgets
- **Dark/Light Themes** — Full theming via CSS custom properties
- **Interactive ASCII Canvas** — Dithered particle background with mouse trail
- **Google X-Ray ATS Search** — Cross-board job search across 6 platforms
- **Pomodoro Timer** — Integrated focus timer with break cycles
- **Cold Email Composer** — Template-based outreach with multiple sender accounts
- **FL Studio DAW Buttons** — Beveled hardware-style toggle controls
- **Fully Responsive** — Optimized for desktop, tablet, and mobile with touch targets

### Modules

| Card | Description |
|------|-------------|
| **Mail** | Multi-provider inbox (Gmail, Outlook, iCloud) with smart sort |
| **Jobs** | ATS board aggregator with keyword search and X-Ray |
| **Focus · Tasks** | Pomodoro timer + task list with priority levels |
| **News Radar** | Curated feeds: Design AI, Deep Tech, Sport, World |
| **Leads** | Social media opportunity tracker (X, LinkedIn, Contra) |
| **Calendar** | Monthly view with event indicators |
| **Cold Email** | Template composer with account selector |
| **Activity Log** | Real-time system event stream |

## Quick Start

### Option 1: Open directly

Just open `public/index.html` in any modern browser. No build step required.

### Option 2: Local server

```bash
# Using Python
python3 -m http.server 8080 --directory public

# Using Node
npx serve public

# Using PHP
php -S localhost:8080 -t public
```

### Option 3: GitHub Pages

Push to GitHub and enable Pages from Settings → Pages → Source: `main` branch, `/public` folder. Or use the included workflow:

```bash
git push origin main
# GitHub Actions will deploy automatically
```

## Project Structure

```
daily-digest/
├── public/
│   └── index.html          # Complete single-file application
├── src/
│   └── daily-digest.jsx    # React source (reference)
├── docs/
│   ├── PRODUCT_SPEC.md     # Full product specification
│   ├── DESIGN_SYSTEM.md    # Design tokens, typography, spacing
│   ├── DESIGN_SYSTEM_PROMPT.md  # AI prompt for design consistency
│   ├── BACKEND_ARCHITECTURE.md  # API integration architecture
│   └── TODO_PROTOTYPE_TO_LIVE.md # Roadmap: prototype → production
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Pages deployment
├── package.json
├── LICENSE
└── README.md
```

## Tech Stack

- **React 18** — via CDN (no build tooling required)
- **Babel Standalone** — JSX transform in-browser
- **IBM Plex Mono** — Primary monospace typeface
- **DM Sans** — Secondary sans-serif
- **CSS Custom Properties** — Full dark/light theming
- **Canvas API** — ASCII dither background + mouse trail
- **Pointer Events API** — Unified mouse/touch interactions

## Configuration

Open the **HUB** (settings) from the top-right controls:

### Profile
- Display name, title, timezone
- Hero text customization
- Primary email account

### Layout
- Grid preset selection (Default, Mail Focus, Compact, Intel Focus)
- Card visibility toggles
- Row height customization
- **Quiet Mode widget selection** — Choose which cards appear in quiet mode (1-4 widgets from Calendar, Mail, Focus, Jobs, News, Leads)

### Connections
- Add/remove mail accounts (Gmail, Outlook, iCloud)

## Quiet Mode

Press the **QUIET** button to enter focus mode. The transition uses a CSS `clip-path` mask reveal — the ASCII background expands from the hero area to fill the viewport while the main dashboard fades out.

Quiet mode displays:
- Customizable widget cards (configurable in Settings → Layout)
- Digital clock
- Large hero typography
- Profile photo + music widget

## Browser Support

- Chrome/Edge 88+
- Firefox 78+
- Safari 15+
- Mobile Safari / Chrome for Android

## API Integration

The Jobs card supports live search via the Anthropic API with `web_search` tool. See `docs/BACKEND_ARCHITECTURE.md` for integration details. When the API is unavailable, the dashboard falls back to local generated data.

## Roadmap

See `docs/TODO_PROTOTYPE_TO_LIVE.md` for the full prototype-to-production roadmap including:
- OAuth mail integration
- Real calendar API sync
- Persistent storage
- Push notifications
- PWA support

## License

MIT — see [LICENSE](LICENSE)

## Author

**Adewale Aloba** — Principal Designer
