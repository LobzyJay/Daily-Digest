# Daily Digest

> Linear-inspired, dark-first productivity dashboard.

![Daily Digest](https://img.shields.io/badge/version-1.0-blue) ![License](https://img.shields.io/badge/license-MIT-green) ![React](https://img.shields.io/badge/React-18-61dafb)

## Overview

Daily Digest is a bento-grid productivity dashboard with a monospace-driven, dark-first aesthetic inspired by Linear. It features a 12-column responsive grid, 8 card components, 34 custom SVG icons, 23 keyframe animations, a real-time dither canvas background, and full dark/light mode theming via CSS custom properties.

## Live Demo

**[→ View Live](https://yourusername.github.io/daily-digest/)**

## Features

- **Bento Grid Layout** — 12-column CSS grid with 4 switchable presets (Default, Mail Focus, Compact, Intel Focus)
- **8 Card Components** — Mail, Jobs, Todo/Pomodoro, News, Leads, Calendar, Cold Email, Activity Log
- **Dark/Light Mode** — Full theme inversion via CSS custom properties, toggle in hero bar
- **Dither Canvas** — Animated ordered-dither background using a Bayer 4×4 threshold matrix
- **Mouse Trail** — Particle system with per-pixel dithering that follows cursor movement
- **Pomodoro Timer** — 96px SVG ring timer with configurable durations (25/30/35/40m)
- **Streak Calendar** — Monthly tap-to-toggle calendar with streak tracking
- **Music Widget** — Frosted glass widget with ASCII album art and animated equalizer bars
- **Profile Photo** — Upload with client-side 1-bit dithering effect
- **34 SVG Icons** — Stroke-based icon system at 24×24 viewBox
- **23 Animations** — Entrance, interaction, and continuous keyframes with consistent easing
- **Settings Modal** — Profile, layout presets, and connection management
- **Responsive** — 6 breakpoints from full desktop (>1399px) down to mobile (≤639px)

## Quick Start

### Option 1: Just open it

Download `index.html` and open it in any browser. That's it — no build step, no dependencies, no install.

### Option 2: GitHub Pages

1. Fork this repository
2. Go to **Settings → Pages**
3. Set source to **Deploy from a branch** → `main` / `root`
4. Your site will be live at `https://yourusername.github.io/daily-digest/`

### Option 3: Any static host

Upload `index.html` to Vercel, Netlify, Cloudflare Pages, or any static hosting. Single file, zero config.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | React 18 (CDN) |
| JSX Transform | Babel Standalone (CDN) |
| Typography | IBM Plex Mono + DM Sans (Google Fonts) |
| Styling | CSS Custom Properties, inline `<style>` block |
| Icons | 34 inline SVGs, stroke-based |
| Canvas | 2D context for dither + particle effects |
| Build | None — single HTML file |

## Design System

The complete design system documentation is available in [`DESIGN_SYSTEM.md`](./DESIGN_SYSTEM.md), covering:

- Color tokens (dark + light)
- Typography scale & `Mo` component
- Spacing system (2px base grid)
- Border & radius scale
- Grid system & layout presets
- All 34 icon SVG paths
- 23 animation keyframes
- Component specifications
- Canvas rendering algorithms (dither, trail, photo processing)
- Visual effects (card glow, bento spotlight)

## Project Structure

```
daily-digest/
├── index.html           # Complete app — single file, zero dependencies
├── DESIGN_SYSTEM.md     # Full design system documentation (1,258 lines)
├── README.md            # This file
└── LICENSE              # MIT License
```

## Browser Support

| Browser | Supported |
|---------|-----------|
| Chrome 90+ | ✅ |
| Firefox 90+ | ✅ |
| Safari 15+ | ✅ |
| Edge 90+ | ✅ |

Requires: CSS Custom Properties, CSS Grid, Canvas 2D, `backdrop-filter`, `clamp()`.

## License

MIT — see [LICENSE](./LICENSE) for details.

---

*Built by Layout Studio*
