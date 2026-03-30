# PROMPT: Build the Daily Digest Design System One-Pager

## Context

You have a comprehensive design system markdown document (`DESIGN_SYSTEM.md`) that documents every token, component, animation, icon, and visual effect for a Linear-inspired dark-first productivity dashboard called **Daily Digest**. Your task is to build a **single-page interactive website** that serves as both the documentation AND the living demonstration of this design system — so any designer or developer on the team can see, copy, and reference everything in one place.

## Task

Create a single `.jsx` React artifact that IS the design system one-pager website. This is NOT a generic docs page — it must **look and feel like the Daily Digest dashboard itself**, using the exact same tokens, fonts, components, and aesthetics documented in the MD. The design system page should be its own best example.

## Requirements

### Structure & Sections

Build the page as a vertical scroll with these sections, each inside cards that use the same `.card` / `.ci` / `.ch` styling from the design system:

1. **Hero** — "DAILY DIGEST — DESIGN SYSTEM" title using the same IBM Plex Mono hero typography (clamp, weight 800, -.05em tracking). Include the animated dither canvas background from the MD. Add version number and date.

2. **Color Tokens** — Render every color as a visual swatch grid. Dark mode tokens on the left, light mode on the right. Each swatch shows the CSS variable name, the actual value, and a filled rectangle of that color. Include the surface hierarchy diagram visually (nested boxes showing --bg → --sf → --rs layering).

3. **Typography** — Show every step of the type scale as actual rendered text at that size/weight/tracking. Include the font stack, the `Mo` component API, and live examples of each size from 7px to the hero clamp.

4. **Spacing & Sizing** — Visual ruler/scale showing every spacing value (1px through 20px) as actual bars. Show the card internal padding map as a live annotated card wireframe with measurements.

5. **Border & Radius** — Render a row of boxes at each radius value (3, 4, 6, 8, 10, 12px) labeled with their usage. Show border styles as actual bordered elements.

6. **Icons** — Render ALL 34 icons in a grid. Each icon shows: the SVG rendered at 24px, the key name below it (e.g., `mail`, `jobs`), and on hover, show the full `d` path in a tooltip or expandable. Make it easy to visually scan and find icons. Use the exact `I` component from the MD with `viewBox="0 0 24 24"`, `strokeWidth:2`, `strokeLinecap:round`, `strokeLinejoin:round`, `fill:none`.

7. **Components** — Live rendered examples of EVERY component documented in the MD:
   - **Card** with Header, scrollable body with sample rows, and footer
   - **Tag** (default + inverted variants)
   - **Dot** (static + pulse)
   - **Button** (primary + secondary, show hover/press states)
   - **Chip** (default + active)
   - **Tab** (default + active)
   - **Input**, **Select**, **Textarea** (show focus states)
   - **Accordion** (working open/close with chevron rotation)
   - **Checkbox** (unchecked + checked with animation)
   - **Task row** with remove button
   - **Stat tile** (2×2 grid with sample data)
   - **Calendar grid** (small working month view with today/done/future states)
   - **Icon box** `.ri`
   - **Timer chip** `.csm` (default + active)
   - **Send button** (default + sending shimmer state)

8. **Grid System** — Visual diagram of the 12-column bento layout. Show all 4 layout presets as small thumbnail grids with colored blocks for each card area. Include the responsive breakpoint table.

9. **Animation System** — Interactive demo area:
   - Buttons that trigger each entrance animation (fadeUp, scaleIn, slideR, slideL, cI, logSlide)
   - Buttons that trigger interaction animations (pop, countPop, shake, sendFly)
   - Live continuous animations (pulse dot, shimmer, breathe, equalizer bars)
   - Press state demos — clickable elements showing the scale values
   - Stagger demo — a row of items that re-animate with staggered delays on click

10. **Visual Effects** — 
    - Embed a small live dither canvas demo (the HeroDither algorithm)
    - Show the ASCII album art block rendered in monospace
    - Show the card glow pseudo-element effect on a hoverable demo card
    - Show the bento spotlight dim/brighten on a mini grid of cards

11. **Music Widget** — Render both states (disconnected + connected/playing) side by side as live components with the EQ bars animating.

12. **Data Shapes** — Code blocks showing each TypeScript-style data shape (Todo, Social Lead, Job, News Item, Email Template, Log Entry) with example values.

13. **Configuration** — Show the default config object with all fields annotated.

### Design Rules for the One-Pager

- **Use the EXACT same CSS variables, fonts, and values** from the design system — this page must be indistinguishable in visual language from the dashboard itself
- **Dark mode by default** with a working light/dark toggle in the top bar
- **IBM Plex Mono for all headings, labels, code, values** — DM Sans for body descriptions
- **Card-based layout** — each section lives inside a card with the standard `.ch` header
- **`1px solid var(--bd)` borders everywhere**, no shadows except where documented
- **All animations must use `cubic-bezier(.4, 0, .2, 1)`**
- **Sticky navigation** — a sidebar or top nav that lets you jump to any section
- **Copy-to-clipboard** on code blocks (CSS variables, icon paths, component code)
- **Responsive** — works down to mobile single-column
- **Section headers** use the same `Mo` component pattern: `s={11} w={700} ls="0.12em"` uppercase with an icon

### What NOT to Do

- Do NOT make it look like a generic Storybook or markdown renderer
- Do NOT use Tailwind classes — use inline styles or a `<style>` block with the exact CSS from the MD
- Do NOT add shadows to cards
- Do NOT use any colors outside the token system
- Do NOT use any font besides IBM Plex Mono and DM Sans
- Do NOT skip any icon — all 34 must be rendered
- Do NOT skip any component — every one documented must have a live example
- Do NOT make it feel like a docs site — it should feel like a product

### Technical

- Single `.jsx` file (React artifact)
- Import only: `useState`, `useRef`, `useEffect`, `useCallback` from React
- Use a `<style>` block for all CSS (same approach as the dashboard)
- All icons defined inline as SVG paths (copy from the MD icon table)
- Include the dither canvas algorithm for the hero background
- Make the light/dark toggle functional

## Reference

The full design system specification is in `DESIGN_SYSTEM.md`. Every token, path, value, and algorithm you need is documented there. Read it completely before writing any code. Follow it exactly — this one-pager IS the design system, so any deviation from the spec is a bug.
