# Daily Digest — Design System

> Linear-inspired · Dark-first · Monospace-driven  
> v1.0 — February 2026

---

## 1 · Philosophy

Restrained, utilitarian, dense. Inspired by Linear. Every element earns its space through function. No decoration, no gradients on surfaces, no drop shadows on cards. Clarity, rhythm, and density are the three pillars.

**Core rules:**

- Dark-first design with full light-mode inversion via CSS custom properties
- Borders for depth, never box-shadows (one exception: the modal overlay)
- IBM Plex Mono carries all structured text — headers, labels, values, buttons, inputs
- One easing function governs 95% of all motion: `cubic-bezier(.4, 0, .2, 1)`
- Every interactive element must have cursor, hover state, press scale, and transition
- Each card component owns its own state — no global store

---

## 2 · Color Tokens

All colors are CSS custom properties on `.root` (dark) and `.root.light`.

### 2.1 Dark Mode (Default)

```css
.root {
  --bg:      #0a0a0b;                  /* Page background */
  --sf:      #111113;                  /* Card / surface */
  --rs:      #18181b;                  /* Recessed — inputs, tiles, icon boxes */
  --bd:      rgba(255,255,255, .06);   /* Border default */
  --bd-hi:   rgba(255,255,255, .12);   /* Border hover / focus */
  --tx-lo:   rgba(255,255,255, .35);   /* Tertiary — labels, timestamps */
  --tx-md:   rgba(255,255,255, .55);   /* Secondary — body, descriptions */
  --tx-hi:   rgba(255,255,255, .9);    /* Primary — headings, stat values */
  --inv-bg:  #fff;                     /* Inverted fill — primary CTA, active tab, today cell */
  --inv-tx:  #111;                     /* Text on inverted fill */
  --glow:    rgba(255,255,255, .03);   /* Row / card hover highlight */
}
```

### 2.2 Light Mode

```css
.root.light {
  --bg:      #f7f7f5;
  --sf:      #ffffff;
  --rs:      #f2f1ee;
  --bd:      rgba(0,0,0, .06);
  --bd-hi:   rgba(0,0,0, .12);
  --tx-lo:   rgba(0,0,0, .35);
  --tx-md:   rgba(0,0,0, .55);
  --tx-hi:   rgba(0,0,0, .88);
  --inv-bg:  #1a1a1e;
  --inv-tx:  #f5f5f5;
  --glow:    rgba(0,0,0, .02);
}
```

### 2.3 Surface Hierarchy

```
--bg  (#0a0a0b)   Page body
 └─ --sf  (#111113)   Card surface
      └─ --rs  (#18181b)   Input / Tile / Icon box
```

### 2.4 Rules

- Never use raw hex — always `var(--token)`
- 3-tier text: `--tx-hi` headings/values → `--tx-md` body → `--tx-lo` meta/labels
- Inverted pair (`--inv-bg` + `--inv-tx`) is reserved for primary buttons, active tabs, and today calendar cell
- Done calendar cells: `rgba(255,255,255,.04)` dark / `rgba(0,0,0,.04)` light

---

## 3 · Typography

### 3.1 Font Stack

| Font | CDN Weights | Role |
|------|------------|------|
| **IBM Plex Mono** | 400, 500, 600, 700, 800 | All structured text — headers, labels, values, buttons, inputs, timestamps |
| **DM Sans** | 400, 500, 600, 700, 800 | Body / inherited fallback via `font-family: inherit` |

```html
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600;700;800&family=DM+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
```

### 3.2 Type Scale

| px | Weight | Tracking | Usage |
|----|--------|----------|-------|
| 7 | 600 | — | Timer micro-status |
| 8 | 600 | `.1em` | Chip labels, stat labels, duration pills, calendar counter |
| 9 | 600 | `.04em` | Tags, tab buttons, row meta, button text, day labels |
| 10 | 600 | `.06em` | Accordion headers, action buttons, log entries |
| 11 | 600–700 | `.12em` | Card header labels, lead names |
| 12 | 400–500 | — | Task text, descriptions, body |
| 15–20 | 800 | — | Pomodoro timer display |
| 20 | 800 | `-.04em` | Stat values (`.sn`) |
| 22 | 800 | `-.04em` | Mail category numbers (`.mn2`) |
| clamp(48, 8vw, 100) | 800 | `-.05em` | Hero title (`.hero-t`) |

### 3.3 `Mo` Component (Monospace Span)

Used everywhere for labels and values:

```jsx
<Mo s={11} c="var(--tx-hi)" w={700} ls="0.12em">LABEL</Mo>
```

| Prop | CSS Property | Default |
|------|-------------|---------|
| `s` | `fontSize` (px) | required |
| `c` | `color` | required |
| `w` | `fontWeight` | 500 |
| `ls` | `letterSpacing` | 0 |
| `st` | style object override | `{}` |

Convention: uppercase labels → `ls="0.12em"`, `w={700}`, `s={11}`. Counters/meta → `s={8–9}`, `c="var(--tx-lo)"`.

---

## 4 · Spacing

### 4.1 Base Grid

All spacing sits on a **2px base**. The scale:

| px | Usage |
|----|-------|
| 1 | Bento grid gap (desktop) |
| 2 | Calendar gap, micro gaps |
| 3 | Chip groups, stat grid gap, tag gap |
| 4 | Button groups, icon-label gap |
| 5 | Dot sizes |
| 6 | Compact padding — stat tiles, input row |
| 8 | Accordion header vertical, pomodoro gap |
| 10 | Row vertical padding, header/footer vertical |
| 12 | Standard horizontal inner |
| 14 | Accordion horizontal, calendar grid horizontal |
| 16 | Card header/footer horizontal, row horizontal |
| 20 | Modal padding, hero outer |

### 4.2 Card Internal Padding

```
Header  (.ch)  →  10px 16px
Row     (.lr)  →  10px 16px,  gap: 10px
Footer  (.cf)  →  10px 16px
Accordion (.aH) →  8px 14px
Input row (.ti) →  6px 10px
Stat grid (.sg) →  6px,  gap: 3px
Cal grid        →  0 14px 10px,  gap: 2px
Cal days        →  0 14px
```

### 4.3 Element Sizes

| Element | Size |
|---------|------|
| Checkbox `.ck` | 14 × 14px |
| Icon box `.ri` | 30 × 30px |
| Priority / status dot | 5px diameter |
| Profile photo `.pfp` | 72 × 72px |
| Pomodoro ring SVG | 96 × 96px, `r=42`, `strokeWidth=2` |
| Music widget | `min-width: 240px` |

---

## 5 · Borders & Radius

### 5.1 Border Styles

| Pattern | Where |
|---------|-------|
| `1px solid var(--bd)` | Cards, rows, inputs, tiles, headers, footers |
| `1px solid var(--bd-hi)` | Hover / focus state |
| `1px solid transparent` | Primary buttons (size parity with secondary) |
| `1.5px solid` | Checkbox (color from task priority) |

### 5.2 Radius Scale

| px | Where |
|----|-------|
| 3 | Checkbox, tag, calendar cell, task-remove btn |
| 4 | Chip, tab (`.ntab`), filter pill |
| 6 | Button (`.ba`), input, stat tile, icon box, mail tile |
| 8 | Card (`.card`), card wrapper (`.aw`) |
| 10 | Profile photo, music widget, hero buttons |
| 12 | Modal (`.mbox`) |

**Rule**: Inner element radius is always smaller than parent radius.

### 5.3 No Shadows

Only permitted `box-shadow` values in the entire system:

| Context | Value |
|---------|-------|
| Modal `.mbox` | `0 24px 48px rgba(0,0,0,.25)` |
| Input `:focus` | `0 0 0 2px var(--glow)` |
| Pulse dots (animated only) | `0 0 2–10px rgba(...)` |

Everything else uses **borders only** for depth.

---

## 6 · Grid System

### 6.1 Bento Layout

```css
.bento {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: 380px 340px 360px 48px;
  gap: 1px;
}
```

### 6.2 Default Desktop (>1399px)

```
┌───── 4 col ─────┬───── 4 col ─────┬───── 4 col ─────┐
│      MAIL        │      JOBS        │      TODO        │  380px
├───── 4 col ─────┼───── 4 col ─────┼───── 4 col ─────┤
│      NEWS        │     LEADS        │    CALENDAR      │  340px
├─────────────────────── 12 col ────────────────────────┤
│                     COLD EMAIL                         │  360px
├─────────────────────── 12 col ────────────────────────┤
│                       LOG BAR                          │  48px
└───────────────────────────────────────────────────────┘
```

```css
grid-template-areas:
  "mail mail mail mail  jobs jobs jobs jobs  todo todo todo todo"
  "news news news news  social social social social  calendar calendar calendar calendar"
  "email email email email  email email email email  email email email email"
  "log log log log  log log log log  log log log log";
```

### 6.3 Internal Grids

| Context | Columns | Gap |
|---------|---------|-----|
| Mail category tiles `.mg2` | `1fr 1fr 1fr 1fr` | 3px |
| Stat tiles `.sg` | `1fr 1fr` | 3px |
| Calendar `.cal-grid` | `repeat(7, 1fr)` | 2px |

---

## 7 · Layout Presets

Four switchable presets (Settings → Layout tab). Row heights are also user-customizable.

### Default

```
Row 1 (380px):  mail ×4  ·  jobs ×4  ·  todo ×4
Row 2 (340px):  news ×4  ·  social ×4  ·  calendar ×4
Row 3 (360px):  email ×12
Row 4 (48px):   log ×12
```

### Mail Focus

```
Row 1 (340px):  mail ×6  ·  jobs ×3  ·  todo ×3
Row 2 (340px):  news ×4  ·  social ×4  ·  calendar ×4
Row 3 (360px):  email ×12
Row 4 (48px):   log ×12
```

### Compact

```
Row 1 (320px):  todo ×4  ·  mail ×4  ·  jobs ×4
Row 2 (340px):  news ×4  ·  social ×4  ·  calendar ×4
Row 3 (340px):  email ×12
Row 4 (48px):   log ×12
```

### Intel Focus

```
Row 1 (380px):  mail ×4  ·  jobs ×4  ·  todo ×4
Row 2 (340px):  news ×4  ·  social ×4  ·  calendar ×4
Row 3 (360px):  email ×12
Row 4 (48px):   log ×12
```

---

## 8 · Responsive Breakpoints

| Breakpoint | Columns | Rows | Notes |
|------------|---------|------|-------|
| >1399px | `repeat(12, 1fr)` | `380 340 360 48` | Full desktop, preset-driven |
| ≤1399px | 12-col | `360 320 340 48` | Reduced heights |
| ≤1199px | `repeat(12, 1fr)` | `340 320 340 48` | Tighter |
| ≤1023px | `repeat(6, 1fr)` | `320 300 300 340 48` | 5-row: mail+jobs → todo → news+social → cal+email → log |
| ≤767px | `1fr 1fr` | `340 360 280 280 280 340 48` | 7-row mobile, `gap:3px`, news+social side-by-side |
| ≤639px | `1fr` | `repeat(7, auto) 48px` | Single column, `min-height:280px` per card |

---

## 9 · Components

### 9.1 Card

```
┌─ .card ──────────────────────────────────────┐
│ ┌─ .ci ────────────────────────────────────┐ │
│ │  .ch  (Header)     10px 16px             │ │
│ │  .cs  (Scrollable body)  flex:1          │ │
│ │    .lr (Row)        10px 16px            │ │
│ │    .lr (Row)        10px 16px            │ │
│ │  .cf  (Footer)     10px 16px             │ │
│ └──────────────────────────────────────────┘ │
└──────────────────────────────────────────────┘
```

```jsx
<Card area="mail">
  <Head left={...} right={...} />
  <div className="cs">{/* scrollable rows */}</div>
  <div className="cf">{/* footer actions */}</div>
</Card>
```

| Class | Role | Key CSS |
|-------|------|---------|
| `.card` | Outer shell | `border-radius:8px; border:1px solid var(--bd); overflow:hidden` |
| `.ci` | Inner flex | `flex-direction:column; height:100%; overflow:hidden` |
| `.aw` | Alt inner (Todo/Calendar) | Same + own border for double-border effect |
| `.ch` | Header | `padding:10px 16px; border-bottom:1px solid var(--bd)` |
| `.cs` | Scroll body | `flex:1; overflow-y:auto` |
| `.cf` | Footer | `padding:10px 16px; border-top:1px solid var(--bd)` |
| `.lr` | List row | `padding:10px 16px; gap:10px; hover:background var(--glow)` |

Card hover: `border-color:var(--bd-hi)` + gradient glow pseudo fades in.
Bento hover: all cards dim (`contrast(.94)`), hovered card brightens.

### 9.2 Head

```jsx
<Head
  left={<>{Ic.mail({s:13,c:"var(--tx-hi)"})}<Mo s={11} c="var(--tx-hi)" w={700} ls="0.12em">MAIL</Mo></>}
  right={<Tag inv>3 NEW</Tag>}
/>
```

### 9.3 Tag

| Variant | bg | border | text |
|---------|-----|--------|------|
| Default | `var(--rs)` | `1px solid var(--bd)` | `var(--tx-md)` |
| Inverted `.tag.inv` | `var(--inv-bg)` | `transparent` | `var(--inv-tx)` |

`padding:2px 7px · font-size:9px · weight:600 · border-radius:3px · IBM Plex Mono`

### 9.4 Dot

```jsx
<Dot color="var(--tx-hi)" size={5} pulse />
```

Pulse: `box-shadow` glow oscillation via `gl` / `gl2` keyframes.

### 9.5 Button `.ba`

| Variant | bg | border | text |
|---------|-----|--------|------|
| `.ba.pri` | `var(--inv-bg)` | `transparent` | `var(--inv-tx)` |
| `.ba.sec` | `transparent` | `var(--bd)` | `var(--tx-lo)` |

`padding:7px 18px · font-size:10px · weight:600 · radius:6px · tracking:.06em`
Compact: `padding:4-5px 12-16px · font-size:9px`
Hover: `brightness(1.08)` · Press: `scale(.95)`

### 9.6 Chip `.chip`

`padding:4px 10px · 9px · 600 · radius:4px · no border · transparent bg`
Active `.on`: `color:var(--tx-hi); background:var(--glow)`

### 9.7 Tab `.ntab`

`padding:4px 10px · 8px · 600 · radius:4px · 1px solid var(--bd)`
Active `.on`: `bg:var(--inv-bg); color:var(--inv-tx); border:transparent`

### 9.8 Input `.inp`

`bg:var(--rs) · border:1px solid var(--bd) · radius:6px · padding:6px 10px · 11px`
Focus: `border:var(--bd-hi); box-shadow:0 0 0 2px var(--glow)`

### 9.9 Select `.sel`

Same as `.inp` with `padding:7px 12px · cursor:pointer · IBM Plex Mono`

### 9.10 Textarea `.eta`

`padding:10px 12px · min-height:80px · resize:vertical · line-height:1.5 · IBM Plex Mono`

### 9.11 Send Button `.send-btn`

`bg:var(--inv-bg) · color:var(--inv-tx) · border:transparent · padding:8px 20px · radius:6px`
Sending: shimmer gradient. Disabled: `opacity:.4`. Press: `scale(.96)`

### 9.12 Accordion `.aH` / `.aB`

Header `.aH`: `padding:8px 14px · no border · transparent bg · width:100%`
Hover: `bg:var(--glow)`. Open: `border-bottom:1px solid var(--bd)`

Body `.aB`:
- Closed: `flex:0 0 0; overflow:hidden`
- Open: `flex:0 1 auto; overflow:auto`
- Transition: `flex .3s cubic-bezier(.4,0,.2,1)`

### 9.13 Checkbox `.ck`

`14×14 · border:1.5px solid · radius:3px`
Done: `border:var(--tx-lo); bg:var(--glow); pop animation`
Press: `scale(.85)`

### 9.14 Task Remove `.task-rm`

`padding:4px · radius:4px · opacity:.3 · hover:opacity 1 + bg:var(--glow)`
Always visible. Far right of row. Icon: `Ic.x({s:10})`

### 9.15 Stat Tile `.sc`

Grid `.sg`: `1fr 1fr · gap:3px · padding:6px`
Tile `.sc`: `bg:var(--rs) · border:1px solid var(--bd) · padding:10px 8px · radius:6px`
Value `.sn`: `20px · 800 · var(--tx-hi)`
Label: `8px · 600 · var(--tx-lo) · tracking:.1em`
Entrance: `scaleIn .3s` with `.05s` stagger.

### 9.16 Calendar Grid

Day labels `.cal-dlbl`: `9px · 600 · var(--tx-lo)`
Grid `.cal-grid`: `repeat(7,1fr) · gap:2px · flex:1 · align-content:stretch`
Cell `.cal-c`: `radius:3px · centered`
Number `.cal-n`: `11px · 500 · var(--tx-lo)`
Done `.dn`: `bg:rgba(255,255,255,.04) · text:var(--tx-hi) · pop animation`
Today `.td`: `bg:var(--inv-bg) · text:var(--inv-tx) · weight:700`
Future `.fu`: `opacity:.2 · cursor:default`
Press: `scale(.88)`

### 9.17 Icon Box `.ri`

`30×30 · bg:var(--rs) · border:1px solid var(--bd) · radius:6px`
Row hover: `scale(1.08); border-color:var(--bd-hi)`

### 9.18 Timer Chip `.csm`

`padding:3px 9px · 9px · 600 · radius:4px · transparent bg`
Active `.on`: `bg:var(--inv-bg); color:var(--inv-tx)`
Running state: `opacity:.35`

---

## 10 · Iconography

All icons: inline SVG, 24×24 viewBox, stroke-based, `strokeWidth:2`, `linecap:round`, `linejoin:round`, `fill:none`.

### 10.1 Usage

```jsx
{Ic.mail({s: 13, c: "var(--tx-hi)"})}
// s = pixel size, c = stroke color
```

Standard sizes: 10–13px (UI), 16px (modal close).

### 10.2 Full Icon Reference

| Key | SVG `d` Path |
|-----|-------------|
| `mail` | `M3 7l9 6 9-6M3 7v10a2 2 0 002 2h14a2 2 0 002-2V7M3 7a2 2 0 012-2h14a2 2 0 012 2` |
| `inbox` | `M22 12h-6l-2 3H10l-2-3H2M5.45 5.11L2 12v6a2 2 0 002 2h16a2 2 0 002-2v-6l-3.45-6.89A2 2 0 0016.76 4H7.24a2 2 0 00-1.79 1.11z` |
| `send` | `M22 2L11 13M22 2l-7 20-4-9-9-4z` |
| `jobs` | `M20 7H4a2 2 0 00-2 2v10a2 2 0 002 2h16a2 2 0 002-2V9a2 2 0 00-2-2zM16 21V5a2 2 0 00-2-2h-4a2 2 0 00-2 2v16` |
| `news` | `M4 22h16a2 2 0 002-2V4a2 2 0 00-2-2H8a2 2 0 00-2 2v16a2 2 0 01-2 2zm0 0a2 2 0 01-2-2v-9c0-1.1.9-2 2-2h2M10 6h8M10 10h4` |
| `timer` | `M12 22a9 9 0 100-18 9 9 0 000 18zM12 6v6l4 2M9 1h6M12 1v3` |
| `cal` | `M4 8h16M4 4h16a2 2 0 012 2v14a2 2 0 01-2 2H4a2 2 0 01-2-2V6a2 2 0 012-2zM8 2v4M16 2v4M9 12h2M13 12h2M9 16h2M13 16h2` |
| `tasks` | `M9 11l3 3L22 4M21 12v7a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2h11` |
| `stats` | `M18 20V10M12 20V4M6 20v-6` |
| `social` | `M17 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2M9 11a4 4 0 100-8 4 4 0 000 8zM23 21v-2a4 4 0 00-3-3.87M16 3.13a4 4 0 010 7.75` |
| `log` | `M13 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V9zM13 2v7h7` |
| `settings` | `M12 15a3 3 0 100-6 3 3 0 000 6z` + gear teeth path (see codebase) |
| `music` | `M9 18V5l12-2v13M9 18a3 3 0 11-6 0 3 3 0 016 0zM21 16a3 3 0 11-6 0 3 3 0 016 0z` |
| `play` | `M5 3l14 9-14 9z` |
| `pause` | `M6 4h4v16H6zM14 4h4v16h-4z` |
| `star` | `M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01z` |
| `upload` | `M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4M17 8l-5-5-5 5M12 3v12` |
| `x` | `M18 6L6 18M6 6l12 12` |
| `plus` | `M12 5v14M5 12h14` |
| `back` | `M19 12H5M12 19l-7-7 7-7` |
| `link` | `M10 13a5 5 0 007.54.54l3-3a5 5 0 00-7.07-7.07l-1.72 1.71M14 11a5 5 0 00-7.54-.54l-3 3a5 5 0 007.07 7.07l1.71-1.71` |
| `design` | `M12 19l7-7 3 3-7 7zM18 13l-1.5-7.5L2 2l3.5 14.5L13 18zM2 2l7.586 7.586M11 13a2 2 0 110-4 2 2 0 010 4z` |
| `globe` | `M12 22a10 10 0 100-20 10 10 0 000 20zM2 12h20M12 2a15.3 15.3 0 014 10 15.3 15.3 0 01-4 10 15.3 15.3 0 01-4-10 15.3 15.3 0 014-10z` |
| `zap` | `M13 2L3 14h9l-1 8 10-12h-9z` |
| `sport` | `M12 22a10 10 0 100-20 10 10 0 000 20zM2.2 10.3l5.1 1.8 3.1-4.3M12.5 2.1L9.9 8.4 4.3 14M22 12l-5.5-1L12 22M20 5.6L16.5 11l1 6.5` |
| `eye` | `M1 12s4-8 11-8 11 8 11 8-4 8-11 8S1 12 1 12zM12 15a3 3 0 100-6 3 3 0 000 6z` |
| `shield` | `M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z` |
| `chevDown` | `M6 9l6 6 6-6` |
| `chevRight` | `M9 18l6-6-6-6` |
| `sun` | `M12 17a5 5 0 100-10 5 5 0 000 10zM12 1v2M12 21v2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42M1 12h2M21 12h2M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42` |
| `moon` | `M21 12.79A9 9 0 1111.21 3 7 7 0 0021 12.79z` |
| `check` | `M20 6L9 17l-5-5` |
| `closeSm` | `M18 6L6 18M6 6l12 12` |
| `cloud` | `M18 10h-1.26A8 8 0 109 20h9a5 5 0 000-10z` |

---

## 11 · Animation System

### 11.1 Easing

**Primary**: `cubic-bezier(.4, 0, .2, 1)` — used on ~95% of transitions. Snappy start, gentle decel.
**Dot ping**: `cubic-bezier(0, 0, .2, 1)` — slower start for radiating glow.

### 11.2 Transition Durations

| ms | Usage |
|----|-------|
| 150 | Micro — checkbox, calendar cell hover, border color |
| 200 | Standard — button hover, icon scale, opacity, border |
| 250 | Medium — profile photo hover, music widget |
| 300 | Content — row entrance, accordion expand, stat card |
| 400 | Card entrance (staggered) |
| 600 | Hero text entrance, bento filter |
| 800 | Pomodoro stroke transition |

### 11.3 Entrance Keyframes

| Name | From | To | Usage |
|------|------|----|-------|
| `fadeUp` | `opacity:0; translateY(6px)` | `opacity:1; translateY(0)` | Rows, fields, news |
| `fadeIn` | `opacity:0` | `opacity:1` | General |
| `scaleIn` | `opacity:0; scale(.92)` | `opacity:1; scale(1)` | Stat tiles, mail tiles |
| `slideR` | `opacity:0; translateX(-8px)` | `opacity:1; translateX(0)` | Slide from left |
| `slideL` | `opacity:0; translateX(8px)` | `opacity:1; translateX(0)` | Slide from right |
| `cI` | `opacity:0; translateY(8px)` | `opacity:1; translateY(0)` | Card entrance |
| `logSlide` | `opacity:0; translateY(10px); blur(2px)` | `opacity:1; none; blur(0)` | Log entries |
| `mdIn` | `opacity:0` | `opacity:1` | Modal overlay |
| `mdBox` | `opacity:0; scale(.97) translateY(8px)` | `opacity:1; scale(1)` | Modal box |

### 11.4 Interaction Keyframes

| Name | Animation | Usage |
|------|-----------|-------|
| `pop` | `scale(1) → 1.15 → 1` | Checkbox done, calendar check |
| `countPop` | `scale(1) → 1.25 → 1` | Tag count update |
| `shake` | `translateX ±2px` oscillation | Error states |
| `check` | `stroke-dashoffset: 24 → 0` | Check SVG draw |
| `sendFly` | `translateX(12) translateY(-8) scale(.7) → opacity:0` | Send button |
| `strike` | `width: 0 → 100%` | Strikethrough |

### 11.5 Continuous Keyframes

| Name | Animation | Usage |
|------|-----------|-------|
| `gl` / `gl2` | `box-shadow` glow pulse | Dot indicator (dark/light) |
| `pulse3` | `opacity: 1 → .5 → 1` | Loading |
| `shimmer` | `background-position: -200% → 200%` | Send button loading |
| `breathe` | `opacity: .35 → .55` | Idle pulse |
| `eq` | `height` oscillation | Equalizer bars |
| `dotPing` | `scale(1) → 2.5; opacity .7 → 0` | Unread dot |
| `timerDone` | `box-shadow: 0 → 20px glow → 0` | Pomodoro complete |

### 11.6 Stagger Pattern

Child elements get incremental `animation-delay`:

```css
.cs > .lr:nth-child(1) { animation-delay: 0s; }
.cs > .lr:nth-child(2) { animation-delay: .03s; }
.cs > .lr:nth-child(3) { animation-delay: .06s; }
/* +.03s per child, up to 10 */
```

| Context | Increment |
|---------|-----------|
| Card rows `.lr` | +.03s |
| Bento cards | +.03s |
| Stat tiles `.sc` | +.05s |
| Mail tiles `.mt2` | +.04s |
| Settings fields | +.04s |
| Mail feed items | +.03s |

### 11.7 Press States

| Element | Scale |
|---------|-------|
| Buttons `.ba`, `.send-btn` | `scale(.95)` / `.scale(.96)` |
| Tabs `.ntab`, chips `.chip` | `scale(.94)` – `scale(.95)` |
| Checkbox `.ck` | `scale(.85)` |
| Calendar cell `.cal-c` | `scale(.88)` |
| Hero buttons | `scale(.94)` |
| Settings toggle | `scale(.9)` |
| Add button `.ab2` | `scale(.95)` |

---

## 12 · Card Catalog

### 12.1 Mail Card · `area="mail"`

Category tiles (4-col grid: inbox/sent/drafts/all) + scrollable email feed with filter chips. Each feed item: provider icon, from, subject, unread dot.

### 12.2 Jobs Card · `area="jobs"`

Job listings with star toggle, company tags, salary, match percentage, application status pill. Smart sort by age/match.

### 12.3 Todo Card · `area="todo"`

Three sections stacked vertically:
1. **Pomodoro hero** — 96px SVG ring, time + status inside, START/RESET buttons, duration chips (25/30/35/40m), session counter. `flex:1` fills available space.
2. **Tasks accordion** — checkbox list with priority dots (urgent/important/normal), add input, remove (✕) button on each row. Empty state: "No tasks yet".
3. **Stats accordion** — 2×2 metric grid (Cold Emails, Apps Sent, Focus, Streak).

### 12.4 News Card · `area="news"`

Category tabs (Design AI, Deep Tech, Sport, OSINT) with icon-per-tab. Feed rows with icon thumbnail, title, source, time, HOT tag. Footer: "FULL ROUNDUP →".

### 12.5 Leads Card · `area="social"`

Social lead rows: platform tag (X, LI, CO, DC), author name, quality dot (high=`--tx-hi`, low=`--tx-lo`), time, description text. Footer: "ALL →".

### 12.6 Calendar Card · `area="calendar"`

Monthly streak tracker. Head: calendar icon + "CALENDAR" + streak badge + progress (checked/total). Month label, 7-col day grid, tap-to-toggle past days, today inverted, future dimmed.

### 12.7 Cold Email Card · `area="email"`

Template picker (Job Application, Freelance, Networking + custom save). Compose: recipient input, tone selector, textarea body, send button with fly animation. Token replacements: [Name], [Company], [Role].

### 12.8 Log Bar · `area="log"`

Auto-scrolling activity feed. Auto-appends new entries every 2.8s. Last entry highlighted. Monospace timestamps.

---

## 13 · Hero Section

```
┌──────────────────────────────────────────────────────────┐
│ ● 64/73 SOURCES                  02:54 WAT  ◐ DARK  ⚙ HUB │
│                                                            │
│ DAILY           ← clamp(48px, 8vw, 100px), weight 800     │
│ DIGEST              tracking: -.05em, line-height: .86     │
│                                                            │
│ SATURDAY, FEBRUARY 21          [PHOTO 72×72] [♪ Connect]   │
│ ADEWALE ALOBA                                              │
└──────────────────────────────────────────────────────────┘
```

- Dithered background image via `<HeroDither>` canvas (pixelated, contrast 1.1)
- Trail overlay via `<HeroTrail>` canvas (opacity .45, pixelated)
- Dim overlay: `rgba(0,0,0,.35)` (dark) / `rgba(255,255,255,.45)` (light)
- Music widget: frosted glass (`backdrop-filter:blur(16px)`), `min-width:240px`, `radius:10px`
- Profile photo: `72×72`, `radius:10px`, upload-on-click, hover `scale(1.03)`
- Title entrance: `fadeUp .6s` with .1s / .2s / .3s stagger

---

## 14 · Modal System

Settings modal — 3 tabs: Profile, Layout, Connections.

| Property | Value |
|----------|-------|
| Overlay `.mbg` | `rgba(0,0,0,.6); backdrop-filter:blur(12px)` |
| Box `.mbox` | `max-width:520px; max-height:80vh; radius:12px` |
| Shadow | `0 24px 48px rgba(0,0,0,.25)` (only permitted shadow) |
| Entrance | Overlay: `fadeIn .2s`. Box: `scale(.97) translateY(8px) → normal .25s` |

### Settings Tabs

| Key | Label | Icon |
|-----|-------|------|
| `profile` | PROFILE | `social` |
| `layout` | LAYOUT | `stats` |
| `connect` | CONNECTIONS | `link` |

### News Filter Tabs (inside News card)

| Key | Label | Icon |
|-----|-------|------|
| `design` | DESIGN AI | `design` |
| `deeptech` | DEEP TECH | `zap` |
| `sport` | SPORT | `sport` |
| `osint` | OSINT | `shield` |

### Visible Cards Toggle

Cards that can be shown/hidden via Settings → Layout:

| Key | Label |
|-----|-------|
| `mail` | Mail |
| `jobs` | Jobs |
| `todo` | Focus · Tasks · Stats |
| `news` | News Radar |
| `social` | Leads |
| `calendar` | Calendar |
| `email` | Cold Email |
| `log` | Activity Log |

---

## 15 · Data Shapes

### Todo Item

```ts
{ id: number, text: string, done: boolean, pri: "urgent" | "important" | "normal" }
```

Priority colors: `urgent → var(--tx-hi)`, `important → var(--tx-md)`, `normal → var(--tx-lo)`

### Social Lead

```ts
{ plat: "X"|"LI"|"CO"|"DC", author: string, txt: string, t: string, q: "high"|"low" }
```

### Job

```ts
{ id: number, title: string, co: string, loc: string, salary: string,
  age: string, match: number, tags: string[], saved: boolean }
```

### News Item

```ts
{ title: string, src: string, t: string, hot?: boolean }
```

Grouped by category: `design`, `deeptech`, `sport`, `osint`.

### Email Template

```ts
{ name: string, body: string }
```

### Log Entry

Plain strings: `"SCAN Greenhouse → 3 new"`, `"MAIL Gmail sync — 47"`, etc.

---

## 16 · Configuration

Default config state:

```js
{
  name: "ADEWALE ALOBA",
  title: "Principal Designer",
  tz: "WAT",
  heroLine1: "DAILY",
  heroLine2: "DIGEST",
  primaryEmail: "adewale@gmail.com",
  syncInterval: 60,
  layout: "default",        // "default" | "mailFirst" | "compact" | "newsHeavy"
  hiddenCards: [],           // array of card keys to hide
  rowHeights: [380, 340, 360]  // customizable row heights in px
}
```

---

## 17 · Rules & Contracts

### Every Interactive Element Must Have:

1. `cursor: pointer`
2. A hover state (border-color, bg glow, or opacity shift)
3. A press state (`transform: scale(...)`)
4. A `transition` on the changing property using `cubic-bezier(.4,0,.2,1)`

### Focus States

Inputs/selects: `border-color:var(--bd-hi)` + `box-shadow:0 0 0 2px var(--glow)`

### Text Rendering

- Never `text-decoration` except strikethrough on completed tasks
- All caps for labels only — body text is sentence case
- All `letter-spacing` values are only on uppercase text

### Color Usage

- Never raw hex — always `var(--token)`
- Never opacity on text elements for hierarchy — use the `--tx-lo/md/hi` scale instead
- Never `rgba` inline — define as a variable if reused

### Animation Rules

- One easing for everything: `cubic-bezier(.4,0,.2,1)`
- Entrance animations use `both` fill mode
- Stagger delay increments: `.03s` for most, `.04–.05s` for tiles
- No animation on first paint for above-the-fold content (hero excepted)

### Border Rules

- All borders use `var(--bd)` or `var(--bd-hi)` — never raw rgba
- Inner border-radius < outer border-radius
- No double borders except `.aw` wrapper (intentional double-border effect)

### Shadow Rules

- No `box-shadow` on any card, row, tile, or button
- Only on: modal overlay, input focus ring, animated pulse dots

---

## 18 · Visual Effects & Canvas Rendering

### 18.1 Dither Background (`HeroDither`)

Animated ordered-dither canvas covering the hero section. Renders at **1/3 resolution** for the pixelated aesthetic (`image-rendering: pixelated`).

**Bayer 4×4 threshold matrix:**

```
B = [
  [ 0,  8,  2, 10],
  [12,  4, 14,  6],
  [ 3, 11,  1,  9],
  [15,  7, 13,  5]
]
```

**Algorithm** (runs every frame via `requestAnimationFrame`):

```
For each pixel (x, y):
  nx = x / width * 3
  ny = y / height * 3
  
  v = sin(nx*2.5 + T*1.1) * cos(ny*2.5 - T*0.7)
    + sin((nx+ny)*1.8 + T*0.5) * 0.5
    + sin(sqrt(nx² + ny²)*3 - T*1.3) * 0.3
    + sin(nx*1.2 - T*0.4) * cos(ny*1.5 + T*0.6) * 0.4

  normalized = (v + 2.2) / 4.4
  on = normalized > B[y%4][x%4] / 16

  Dark mode:  pixel = on ? rgba(255,255,255, 0.165) : transparent
  Light mode: pixel = on ? rgba(lv, lv, lv, 0.31) : transparent
              where lv = floor(180 + normalized * 55)
```

**Time increment**: `T += 0.008` per frame (very slow drift).

**CSS:**

```css
.hero-bg {
  position: absolute; inset: 0;
  image-rendering: pixelated;
  filter: contrast(1.1) brightness(1.1);
  z-index: 0;
}
```

### 18.2 Mouse Trail (`HeroTrail`)

Particle system canvas that spawns dithered particles following the mouse cursor. Renders at **1/5 resolution** (`image-rendering: pixelated`).

**Particle spawn** (on mousemove, throttled to 10ms):

```
For each move event, spawn 3 particles:
  x = mouse.x + random(-2..2)
  y = mouse.y + random(-2..2)
  vx = -dx * 0.03 + random(-0.075..0.075)
  vy = -dy * 0.03 + random(-0.075..0.075)
  life = 1.0
  decay = 0.012 + random(0..0.008)
  size = random(2..5)
  bright = min(speed * 0.15 + 0.3, 1.0)
```

**Render** (each frame):

```
1. Fade canvas: fillRect with rgba(0,0,0, 0.07)
2. For each particle:
   - Update position: x += vx, y += vy
   - Decay life: life -= decay
   - For each pixel in radius (floor(size * life)):
     - intensity = (1 - dist/radius) * life * bright
     - Apply Bayer dither: if intensity > B[y%4][x%4]/16 → draw pixel
     - Pixel color: rgba(255,255,255, min(intensity * life * 0.4, 0.25))
3. Cap at 500 particles max
```

**CSS:**

```css
.hero-trail {
  position: absolute; inset: 0;
  pointer-events: none;
  z-index: 10;
  image-rendering: pixelated;
  opacity: 0.45;
}
```

### 18.3 Dim Overlay

Simple full-cover div between dither canvas and content:

```css
.hero-dim {
  position: absolute; inset: 0;
  background: rgba(0,0,0, 0.35);  /* dark */
  z-index: 1;
  pointer-events: none;
}

/* Light mode */
.root.light .hero-dim { background: rgba(247,247,245, 0.1); }
```

### 18.4 Profile Photo Dithering

Uploaded photos are processed client-side with the same Bayer 4×4 matrix into 1-bit dithered images:

```
1. Draw photo to 96×96 offscreen canvas
2. For each pixel:
   grayscale = R*0.3 + G*0.59 + B*0.11
   normalized = grayscale / 255
   on = normalized > B[y%4][x%4] / 16
   pixel = on ? 255 : 0
3. Export as data URL
```

**Toggle**: Click cycles between dithered and original. CSS for dithered state:

```css
.pfp-img.dith {
  image-rendering: pixelated;
  filter: contrast(1.3);
}
```

---

## 19 · ASCII Art & Text Patterns

### 19.1 Album Art (Music Widget)

When music is connected, a 5-line ASCII block art replaces the album cover:

```
░▓█▓░
▓███▓
█████
▓███▓
░▓█▓░
```

Rendered in IBM Plex Mono at `6px`, `line-height:1`, color `#666`, within a `36px` wide container.

### 19.2 Equalizer Bars

8 bars animated when playing:

```css
.eq {
  display: flex; gap: 2px;
  align-items: flex-end; height: 12px;
}
.eq-bar {
  width: 2px; height: 3px; background: #ccc;
}
.eq-bar.on {
  animation: eq .6s ease-in-out infinite alternate;
}
@keyframes eq {
  0%   { height: 3px; }
  50%  { height: 11px; }
  100% { height: 4px; }
}
```

Each bar gets staggered delay: `animationDelay: i * 0.09s` (0s to 0.63s across 8 bars).

### 19.3 Progress Bar (Music)

Minimal 2px track:

```css
.mw-bar { height: 2px; background: rgba(255,255,255,.12); width: 100%; }
.mw-fill { height: 100%; background: #ccc; transition: width .3s; }
```

### 19.4 Source Counter Pulse

Top-left hero indicator: `● 64/73 SOURCES` with animated dot:

```jsx
<Dot color="#fff" size={7} pulse={pulse} />
```

Pulse toggles every 1400ms via `setInterval`.

---

## 20 · Card Glow & Hover Effects

### 20.1 Card Pseudo-Elements

Two layered pseudo-elements create a subtle light effect on hover:

**`::before`** — Top-down gradient glow:

```css
.card::before {
  content: '';
  position: absolute; inset: 0;
  border-radius: 8px;
  background: linear-gradient(180deg, var(--glow) 0%, transparent 40%);
  pointer-events: none; z-index: 1;
  opacity: 0; transition: opacity .3s;
}
.card:hover::before { opacity: 1; }
```

**`::after`** — Dual radial ambient glow:

```css
.card::after {
  content: '';
  position: absolute; inset: -2px;
  background:
    radial-gradient(ellipse at 30% 0%, rgba(128,128,128,0.05), transparent 50%),
    radial-gradient(ellipse at 70% 100%, rgba(128,128,128,0.03), transparent 50%);
  pointer-events: none; z-index: 0;
  opacity: 0; transition: opacity .4s;
  filter: blur(8px);
}
.card:hover::after { opacity: 1; }
```

### 20.2 Bento Spotlight Effect

When the cursor enters the bento grid, all cards dim and only the hovered card stands out:

```css
.bento.bh {
  filter: contrast(0.94) brightness(0.96);
}
.bento.bh .card:hover {
  filter: contrast(1.08) brightness(1.05);
}

/* Light mode */
.root.light .bento.bh {
  filter: contrast(0.96) brightness(1.01);
}
.root.light .bento.bh .card:hover {
  filter: contrast(1.05) brightness(0.98);
}
```

### 20.3 Card Entrance

All cards enter with staggered `cI` animation:

```css
.card { animation: cI .4s ease both; }

@keyframes cI {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: none; }
}

.bento > :nth-child(1) { animation-delay: .03s; }
.bento > :nth-child(2) { animation-delay: .06s; }
.bento > :nth-child(3) { animation-delay: .09s; }
.bento > :nth-child(4) { animation-delay: .12s; }
.bento > :nth-child(5) { animation-delay: .15s; }
.bento > :nth-child(6) { animation-delay: .18s; }
.bento > :nth-child(7) { animation-delay: .21s; }
.bento > :nth-child(8) { animation-delay: .24s; }
```

---

## 21 · Hero Light Mode Overrides

The hero requires special overrides in light mode since it uses raw colors for the background effects:

```css
.root.light .hero              { background: #F0F0F0; }
.root.light .hero-bg           { filter: none; opacity: 1; }
.root.light .hero-dim          { background: rgba(247,247,245, 0.1); }
.root.light .hero-trail        { opacity: 0.3; }
.root.light .hero-t            { color: rgba(0,0,0, .88); }
.root.light .hero-d            { color: rgba(0,0,0, .4); }
.root.light .hero-name         { color: rgba(0,0,0, .7); }
.root.light .hero .dot         { background: rgba(0,0,0, .7); }
.root.light .hero-top-l span,
.root.light .hero-top-r span   { color: rgba(0,0,0, .4) !important; }
.root.light .hub-btn,
.root.light .lt-btn {
  border-color: rgba(0,0,0, .08);
  color: rgba(0,0,0, .45);
}
.root.light .hub-btn:hover,
.root.light .lt-btn:hover {
  background: rgba(0,0,0, .88);
  color: #fff;
  border-color: transparent;
}
```

---

## 22 · Music Widget

### 22.1 Disconnected State

```
┌───────────────────────────────────────────┐
│  ♪   Connect Music         [Spotify][Apple] │
│       OAuth · read-only                     │
└───────────────────────────────────────────┘
```

### 22.2 Connected/Playing State

```
┌───────────────────────────────────────────┐
│ ░▓█▓░  Untitled (Black Is)            ✕   │
│ ▓███▓  Sault                               │
│ █████  ▬▬▬▬▬▬▬▬▬▬▬▬▬▬▬░░░░░░             │
│ ▓███▓  2:41                    4:03        │
│ ░▓█▓░  SPOTIFY      ⏸ ▎▌▎▐▎▌▎            │
└───────────────────────────────────────────┘
```

### 22.3 CSS

```css
.mw {
  border: 1px solid rgba(255,255,255,.06);
  padding: 10px 14px;
  display: flex; gap: 10px; align-items: center;
  background: rgba(0,0,0,.3);
  backdrop-filter: blur(16px);
  min-width: 240px;
  border-radius: 10px;
}
.mw-svc {
  background: rgba(255,255,255,.05);
  border: 1px solid rgba(255,255,255,.08);
  color: rgba(255,255,255,.6);
  padding: 5px 10px;
  font-size: 9px; font-weight: 600;
  border-radius: 6px;
}
.mw-svc:hover {
  background: rgba(255,255,255,.9);
  color: #111;
  border-color: transparent;
}
```

---

## 23 · Utility Functions

### 23.1 Timer Formatter

```js
const fmt = s => `${String(Math.floor(s/60)).padStart(2,"0")}:${String(s%60).padStart(2,"0")}`;
// fmt(1500) → "25:00"
// fmt(67)   → "01:07"
```

### 23.2 Date Label

```js
const dateLabel = new Date()
  .toLocaleDateString("en-US", { weekday: "long", month: "long", day: "numeric" })
  .toUpperCase();
// → "SATURDAY, FEBRUARY 21"
```

### 23.3 Legacy Color Constants

Used only in the Hero for non-themed elements:

```js
const P = {
  black: "#000", bg: "#080808", surface: "#0f0f0f",
  raised: "#161616", border: "#222", borderHi: "#333",
  dim: "#444", muted: "#777", text: "#bbb",
  bright: "#e0e0e0", white: "#fff"
};
```

---

## 24 · Design Files Reference

### 24.1 Project Files

| File | Description |
|------|-------------|
| `daily-digest.jsx` | Single-file React app — all components, styles, data, and logic (≈960 lines) |
| `DESIGN_SYSTEM.md` | This document |
| `BACKEND_ARCHITECTURE.md` | API integration spec for live data sources |
| `TODO_PROTOTYPE_TO_LIVE.md` | Roadmap from prototype to production |

### 24.2 Architecture

Single `.jsx` file containing:

```
Lines 1–30     → Icon system (Ic object, 28 SVG paths)
Lines 31–60    → Data constants (todos, social, jobs, news, logs, templates)
Lines 61–130   → HeroDither canvas, HeroTrail particles
Lines 131–200  → ProfilePhoto (with client-side dithering)
Lines 201–260  → MusicWidget (connect/playing states, EQ bars)
Lines 261–350  → Hero section composition
Lines 351–460  → Card components (Mail, News, ColdEmail, Jobs, Todo, Calendar, Social, Log)
Lines 461–600  → SettingsModal (3 tabs, layout presets)
Lines 601–660  → App root (state, grid, render)
Lines 661–960  → CSS (all styles in single <style> block)
```

### 24.3 External Dependencies

- **React 18+** (useState, useEffect, useRef, useCallback)
- **Google Fonts**: IBM Plex Mono + DM Sans
- **No UI framework** — pure CSS custom properties
- **No build tools** — designed to run as a single JSX artifact

---

*Daily Digest Design System · Layout Studio · v1.0*
