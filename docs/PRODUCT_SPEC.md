# Daily Digest — Product Spec

A personal command center that pulls in everything that matters each morning — mail, jobs, design AI news, and social leads — so nothing slips through the cracks.

---

## Problem

Checking Gmail, job boards, LinkedIn, X, and design blogs separately every day is a time sink. Opportunities get buried. Important emails get lost between promotions and spam. By the time you spot a perfect job listing, it's been up for a week.

**Daily Digest** consolidates all of it into one clean, scannable view.

---

## Visual Direction

The UI should feel like sitting in front of a machine that was built to process intelligence — not a generic SaaS dashboard. Three reference worlds to pull from:

### Theme Option A — Retro Spaceship Dashboard
Think Apollo-era mission control crossed with a sci-fi cockpit. CRT monitor glow, phosphor green or amber on dark backgrounds, chunky segmented displays for counts and timers, toggle switches for settings, radar-sweep animations for scanning jobs and feeds. Status indicators use LED-dot styles (red/amber/green). Typography leans monospaced — IBM Plex Mono, Space Mono, or OCR-A. Panels have beveled edges and subtle scan-line overlays. The vibe is "you're piloting something serious."

### Theme Option B — Retro DAW / Audio Plugin
Inspired by vintage mixing consoles and VST plugin interfaces — Waves, FabFilter, Universal Audio. Dark metal/brushed aluminum panel backgrounds. Knobs and faders as visual metaphors (e.g., a volume fader showing email load, VU meters for activity levels). Skeuomorphic textures — rubber, brushed steel, backlit LCD readouts. Color palette is warm: amber, burnt orange, cream on charcoal. Typography mixes clean sans-serifs with LCD/calculator-style numerics. The vibe is "your workflow has a sound to it."

### Theme Option C — OSINT Terminal
Modeled after intelligence analyst workstations — Maltego, Shodan, OSINT dashboards. Dense information grids, graph-network visualizations for connections between data points, blinking activity logs, terminal-style feeds. Dark background with high-contrast accent colors (cyan, magenta, warning yellow). Data feels live — scrolling tickers, real-time timestamps, pulsing connection nodes. Map overlays for job locations. The vibe is "you're tracking signals across the internet."

### Shared Principles (All Themes)
- Dark mode only (or dark-dominant with subtle light variant)
- Information-dense but not cluttered — every pixel earns its space
- Micro-animations that feel functional, not decorative (scan sweeps, loading pulses, data ticks)
- Sound design opportunity: subtle UI sounds on interactions (click, toggle, notification chime) matching the chosen aesthetic

---

## Layout — Bento Grid

The main dashboard uses a bento-style grid layout — modular cards of varying sizes arranged in a tight, visually balanced grid. Each card is a self-contained widget. Users can rearrange, resize, and toggle visibility of cards.

```
┌──────────────────────────────────────────────────────────────┐
│  DAILY DIGEST · Wed, Feb 18 · 07:00 WAT          [⚙] [👤]  │
├────────────────────┬────────────────────┬────────────────────┤
│                    │                    │                    │
│   📬 MAIL          │   🎯 JOBS          │   ⏱ FOCUS TIMER   │
│   OVERVIEW         │   SCANNER          │                   │
│                    │                    │   30:00            │
│  ┌──────┬───────┐  │  Sr. Brand Designer│   ● READY         │
│  │Job 3 │Work 7 │  │  — Monad (Remote)  │                   │
│  ├──────┼───────┤  │                    │   [Start] [Reset]  │
│  │Promo │Bills 2│  │  Visual Designer   │                   │
│  │ 12   │       │  │  — Uniswap (NYC)   │───────────────────│
│  └──────┴───────┘  │                    │                   │
│                    │  Design Lead        │   ☑ TODO LIST     │
│                    │  — Phantom (Remote) │                   │
│                    │                    │   □ Update folio   │
│                    │  → 11 new listings  │   □ Reply Monad   │
│                    │                    │   ☑ Send invoice   │
│                    │                    │   □ Dribbble post  │
│                    │                    │   [+ Add task]     │
│                    │                    │                   │
├────────────────────┴────────────────────┼────────────────────┤
│                                         │                    │
│   🤖 AI DESIGN TOOL RADAR (This Week)   │   📡 SOCIAL LEADS  │
│                                         │                    │
│  Figma AI auto-layout shipped           │  @founderX:        │
│  Krea 2.0 — real-time motion gen        │  "Looking for a    │
│  Runway Gen-4 beta access open          │  brand designer    │
│  Galileo AI added Figma export          │  for Web3 startup" │
│  Adobe Firefly video in preview         │                    │
│                                         │  LinkedIn: 3 new   │
│  → Full roundup                         │  #HiringDesigner   │
│                                         │  → Browse leads    │
│                                         │                    │
└─────────────────────────────────────────┴────────────────────┘
```

### Bento Card Types

| Card | Size | Content |
|---|---|---|
| **Mail Overview** | 2×2 | Category tiles with unread counts, tap to expand |
| **Job Scanner** | 2×3 | Scrollable list of new postings, filters at top |
| **Focus Timer** | 1×1 | Pomodoro countdown, start/pause/reset controls |
| **Todo List** | 1×2 | Editable checklist with drag-to-reorder |
| **AI Tool Radar** | 3×2 | Weekly card grid of new tools with icons + summaries |
| **Social Leads** | 1×2 | Feed of relevant LinkedIn/X/Contra posts and briefs |
| **Quick Stats** | 1×1 | At-a-glance numbers — apps sent, response rate, streak |

Cards snap to grid. Drag to rearrange. Double-tap header to collapse. Long-press to resize between preset dimensions.

---

## Core Features

### 1. Smart Mail Grouping

Connect multiple Google/mail accounts and have incoming mail automatically sorted into clear categories:

- **Job Updates** — application confirmations, interview invites, rejections, recruiter outreach
- **Work Mails** — client emails, team comms, project threads (flagged by sender domain or keywords)
- **Promotions** — marketing emails, newsletters, product launches
- **Bills & Payments** — invoices, subscription renewals, payment confirmations, receipts
- **Everything Else** — uncategorized overflow, reviewable and re-assignable

Each category shows an unread count and a preview of the top 3 most recent items. Tapping into any category expands the full list.

---

### 2. Design AI Tool Radar

A weekly roundup of new and updated AI tools relevant to design work. Sources include Product Hunt, Twitter/X, design blogs, and curated newsletters.

**What it tracks:**

- New AI design tools launched that week (image gen, UI tools, motion, 3D, prototyping)
- Major updates to existing tools (Figma AI, Midjourney, Runway, Adobe Firefly, etc.)
- Trending tools in the design community
- Brief summary + link for each entry

**Delivery:** Updated every Monday morning. Pinned at the top of the digest until dismissed. Mid-week hotfixes or major drops trigger an interim update.

---

### 3. Job Board Scanner

Automatically checks job boards for new postings matching your profile — no more manual searching.

**Platforms monitored:**

**ATS & Aggregators:**
- Greenhouse
- Lever
- Ashby
- Workday
- Workable
- BambooHR
- SmartRecruiters
- JazzHR
- Indeed
- Glassdoor
- ZipRecruiter
- Google Jobs

**Design-Specific:**
- Dribbble Jobs
- Behance Jobs
- Coroflot
- AIGA Design Jobs
- Krop
- Working Not Working
- If You Could Jobs
- UX Jobs Board
- Designmodo Jobs

**Web3 / Crypto:**
- Web3 Career
- Crypto Jobs List
- CryptoJobsList.com
- Bankless Jobs
- Remote3
- Froog (Web3 jobs)
- Stablegram Jobs
- DeFi Jobs

**Freelance / Contract:**
- Contra
- Toptal
- Upwork (filtered for design)
- Fiverr Pro
- 99designs
- Braintrust
- YunoJuno
- Superside Talent

**General Tech:**
- Wellfound (AngelList)
- LinkedIn Jobs
- Otta
- Cord
- Hired
- Arc.dev
- Remotive
- We Work Remotely
- Remote OK
- FlexJobs
- Pangian
- Himalayas
- Lemon.io
- AngelList Talent
- Built In
- The Muse
- Dice

**Regional:**
- Jobberman (Nigeria / Africa)
- MyJobMag (Africa)
- TotalJobs (UK)
- Reed (UK)
- CWJobs (UK)
- Bayt (Dubai / MENA)
- GulfTalent (Dubai / MENA)
- Naukrigulf (MENA)
- JobStreet (Singapore / Asia)
- NodeFlair (Singapore)

**Filters:**

- **Role keywords** — Brand Designer, Visual Designer, Product Designer, Design Lead, Creative Director, Motion Designer
- **Industry tags** — Web3, Crypto, Fintech, Gaming, SaaS, DeFi
- **Location** — Remote, Dubai, London, Singapore, Lagos, Zurich (configurable)
- **Seniority** — Senior, Lead, Principal, Head of
- **Posted within** — Last 1–72 hours (configurable)
- **Scan frequency** — Hourly (default). Every 60 minutes, the scanner re-checks all connected boards and surfaces new listings with a "NEW" badge and timestamp

**Each listing shows:**

- Company name + logo
- Role title
- Location / remote status
- Salary range (if listed)
- Posted date
- Quick-apply button (redirects to application page)
- Save for later option

---

### 4. Social Feed & Lead Monitor

Pulls in relevant posts, gig requests, and opportunities from LinkedIn, X (Twitter), Contra, and other lead sources based on keywords and hashtags tied to your work.

**LinkedIn tracking:**

- Posts tagged #DesignJobs, #HiringDesigners, #BrandDesign, #Web3Design, #CreativeJobs
- Posts from recruiters or hiring managers mentioning designer roles
- Posts from connections sharing job openings
- InMail notifications from recruiters

**X / Twitter tracking:**

- Hashtags: #DesignTwitter, #HiringDesigners, #Web3Jobs, #FreelanceDesign, #NeedADesigner, #LookingForDesigner, #UIDesign, #BrandIdentity
- Keywords: "looking for a designer", "need a brand designer", "hiring a creative", "design agency needed"
- Accounts: design job boards, Web3 hiring accounts, recruiter accounts

**Contra tracking:**

- New project briefs matching your skill tags (Brand Identity, Motion Graphics, UI/UX, Web3, 3D)
- Client requests posted in your category
- Commission-free gig opportunities
- Direct client invitations and messages
- Trending projects in your niche

**Additional Lead Sources:**

- Slack communities: Designer Hangout, Friends of Figma, Web3 Designers, Freelance Designers
- Discord servers: design DAOs, Web3 hiring channels, creative communities
- Reddit: r/DesignJobs, r/forhire, r/web3jobs, r/freelance
- Telegram groups: crypto/Web3 hiring channels

**Each post/lead shows:**

- Source platform icon + author name/handle
- Post or brief preview (first 280 chars)
- Engagement stats (likes, reposts) where applicable
- Timestamp
- Direct link to original post/brief
- Lead quality indicator (🟢 High match / 🟡 Partial / 🔵 Worth a look)
- Quick action: Reply / DM / Save / Apply

---

### 5. Todo List + Pomodoro Focus Timer

A built-in task manager paired with a focus timer — because spotting opportunities means nothing if you don't act on them.

#### Todo List

- Add, edit, delete, and reorder tasks via drag-and-drop
- Mark tasks as complete (with satisfying check animation)
- Optional priority levels: 🔴 Urgent, 🟡 Important, 🔵 Normal
- Optional due dates and recurring tasks
- Tags for context: `#apply`, `#client`, `#portfolio`, `#admin`
- Tasks can be linked to digest items (e.g., "Apply to Monad listing" auto-links to the job card)
- Completed tasks move to a collapsible "Done" section at the bottom

#### Pomodoro Timer

- **Default session:** 30 minutes focus / 5 minutes break
- **Configurable:** 25, 30, 35, or 40 minute sessions
- **Attach a timer to a task** — select a todo item, hit start, and the timer runs against that task
- Session counter tracks how many focus blocks you've completed today
- Subtle ambient sound options matching the theme:
  - Spaceship: low engine hum + beep on completion
  - DAW: tape reel ambience + click track pulse
  - OSINT: keyboard clatter + data stream static
- Break reminder notification when session ends
- Daily/weekly stats: total focus time, sessions completed, most-focused-on tags

#### Timer States (Visual)

```
READY       →  30:00  ●  (idle, pulsing dot)
RUNNING     →  24:37  ▶  (counting down, progress ring fills)
PAUSED      →  24:37  ❚❚ (frozen, blinking indicator)
BREAK       →  05:00  ☕  (break countdown, softer color)
COMPLETED   →  00:00  ✓  (session done, streak updated)
```

---

## Setup & Connection Management

Getting 70+ sources wired up shouldn't feel like a chore. The approach is a two-layer system: a guided first-run wizard that gets the essentials connected in under 5 minutes, and a persistent Connections Hub you can access anytime to add, remove, or troubleshoot sources.

### First Launch — Onboarding Wizard

When a new user opens the app for the first time, they land on a full-screen setup flow — not the dashboard. The wizard walks through connection in priority order, one category at a time. Each step is skippable. Progress is saved, so you can bail and finish later.

```
┌─────────────────────────────────────────────────┐
│                                                 │
│   DAILY DIGEST SETUP                            │
│   Let's wire everything up. Takes ~4 minutes.   │
│                                                 │
│   ● Step 1: Email        ←  You are here        │
│   ○ Step 2: Job Profile                         │
│   ○ Step 3: Social Feeds                        │
│   ○ Step 4: Freelance                           │
│   ○ Step 5: Preferences                         │
│                                                 │
│   [Skip Setup →]                                │
│                                                 │
└─────────────────────────────────────────────────┘
```

**Step 1 — Email Accounts**
- "Connect your Gmail or email accounts so we can sort your inbox."
- One-tap Google OAuth button. Supports multiple accounts — each gets its own OAuth flow.
- After connecting, the user sees a confirmation: "Found 3,412 emails. We'll start categorizing them now."
- Option to add non-Gmail accounts via IMAP (Outlook, Yahoo, ProtonMail, custom domain).

**Step 2 — Job Profile**
- "Tell us what kind of work you're looking for so we know what to scan."
- Pre-filled suggestions based on common designer roles. User taps to select or types custom entries.
- Fields:
  - **Role titles** (multi-select chips): Brand Designer, Visual Designer, Product Designer, Design Lead, Creative Director, Motion Designer, UI/UX Designer, Art Director
  - **Industries** (multi-select): Web3, Crypto, Fintech, Gaming, SaaS, DeFi, Entertainment, Fashion
  - **Locations** (multi-select + "Remote" toggle): Lagos, Dubai, London, Singapore, Zurich, NYC, San Francisco, or type custom
  - **Seniority** (multi-select): Mid, Senior, Lead, Principal, Head of, Director
  - **Salary range** (optional slider): Min–Max range in USD/year
- No need to manually connect each job board — the scanner uses these filters across all 70+ boards automatically. Boards that require login (LinkedIn, Wellfound, etc.) show an OAuth button inline.

**Step 3 — Social Feeds**
- "Connect your social accounts to catch leads and hiring posts."
- **LinkedIn** — OAuth connect button. Explain what data is accessed: "We'll monitor your feed for hiring posts, recruiter messages, and design-related content. We don't post on your behalf."
- **X / Twitter** — OAuth connect button. "We'll track hashtags and keywords related to design hiring. Read-only access."
- **Auto-generated keyword list** based on Step 2 answers. User can review and edit:
  - Suggested hashtags: #HiringDesigners, #Web3Jobs, #DesignJobs, #NeedADesigner, etc.
  - Suggested keywords: "looking for a designer", "need a brand designer", etc.
- Each platform shows a toggle: Active / Paused

**Step 4 — Freelance & Lead Platforms**
- "Connect platforms where clients find you directly."
- **Contra** — OAuth connect. "We'll surface new project briefs matching your skills."
- **Slack** — Workspace connect via OAuth. Select which channels to monitor (auto-suggests channels with "jobs", "hiring", "freelance" in name).
- **Discord** — Bot invite link for your servers. Select channels to track.
- **Reddit** — Read-only OAuth. Pre-selects r/DesignJobs, r/forhire, r/web3jobs.
- **Telegram** — Bot token or phone-link for group monitoring.
- Each platform is optional. Skipped platforms show as "Not connected" cards on the dashboard with a one-tap connect option.

**Step 5 — Preferences**
- **Digest time**: "When do you want your daily summary?" — time picker, defaults to 7:00 AM WAT
- **Scan frequency**: Hourly (default) / 30 min / 15 min
- **Theme selection**: Visual preview of Spaceship / DAW / OSINT — tap to select
- **Pomodoro default**: 30 min (adjustable)
- **Notification channels**: Push / Email / Both

After completing the wizard, the user lands on their dashboard with whatever data has already been pulled. Empty cards show a loading state: "Scanning… first results in a few minutes."

### Connections Hub — Persistent Admin Panel

Accessible anytime via the ⚙ icon on the dashboard header. This is the central nervous system for managing all integrations.

```
┌─────────────────────────────────────────────────┐
│  ⚙ CONNECTIONS HUB                              │
├─────────────────────────────────────────────────┤
│                                                 │
│  EMAIL ACCOUNTS                                 │
│  ┌─────────────────────────────────────────┐    │
│  │ ✅ adewale@gmail.com       [Disconnect] │    │
│  │    Last sync: 2 min ago · 47 unread     │    │
│  ├─────────────────────────────────────────┤    │
│  │ ✅ wale@layoutstudio.co    [Disconnect] │    │
│  │    Last sync: 2 min ago · 12 unread     │    │
│  ├─────────────────────────────────────────┤    │
│  │ ✅ wale@021labs.com        [Disconnect] │    │
│  │    Last sync: 2 min ago · 8 unread      │    │
│  ├─────────────────────────────────────────┤    │
│  │ [+ Add another email account]           │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
│  SOCIAL ACCOUNTS                                │
│  ┌─────────────────────────────────────────┐    │
│  │ ✅ LinkedIn    @adewale    [Manage]     │    │
│  │ ✅ X/Twitter   @waborle    [Manage]     │    │
│  │ ⚠️ Contra      Token expired [Reconnect]│    │
│  │ ⬜ Reddit      Not connected [Connect]  │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
│  COMMUNITY CHANNELS                             │
│  ┌─────────────────────────────────────────┐    │
│  │ ✅ Slack: Friends of Figma  [Manage]    │    │
│  │ ✅ Discord: Web3 Designers  [Manage]    │    │
│  │ ⬜ Telegram: Not connected  [Connect]   │    │
│  │ [+ Add community source]               │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
│  JOB BOARDS                                     │
│  ┌─────────────────────────────────────────┐    │
│  │ Auto-scanned (no login needed): 54      │    │
│  │ ✅ LinkedIn Jobs (via LinkedIn OAuth)    │    │
│  │ ✅ Wellfound   (connected)  [Manage]    │    │
│  │ ⬜ Hired       (not connected) [Login]  │    │
│  │ ⬜ Otta        (not connected) [Login]  │    │
│  │ [View all 70+ boards →]                 │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
│  JOB PROFILE                                    │
│  ┌─────────────────────────────────────────┐    │
│  │ Roles: Brand, Visual, Motion, Lead      │    │
│  │ Industries: Web3, Fintech, Gaming       │    │
│  │ Locations: Remote, Dubai, London, Lagos │    │
│  │ Seniority: Senior, Lead, Principal      │    │
│  │ [Edit Profile →]                        │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
│  SYSTEM STATUS                                  │
│  ┌─────────────────────────────────────────┐    │
│  │ Last full scan: 7 min ago               │    │
│  │ Next scan: in 53 min                    │    │
│  │ Sources active: 64 / 73                 │    │
│  │ Sources erroring: 2 (Contra, Krop)      │    │
│  │ [Force scan now]  [View error log]      │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Connection States

Every connected source shows one of five states:

| State | Indicator | Meaning |
|---|---|---|
| **Connected** | ✅ Green | Active, syncing on schedule |
| **Syncing** | 🔄 Spinning | Currently pulling data |
| **Warning** | ⚠️ Amber | Token expiring soon, rate limited, or partial data |
| **Error** | 🔴 Red | Failed to connect, token expired, API down |
| **Not connected** | ⬜ Grey | Available but not yet set up |

Tapping any source in Warning or Error state shows a diagnostic card: what went wrong, when it last worked, and a one-tap fix (usually "Reconnect" which re-triggers OAuth).

### Connection Categories & Auth Methods

| Source Type | Auth Method | Login Required? |
|---|---|---|
| **Gmail / Email** | Google OAuth 2.0 / IMAP credentials | Yes — user connects once |
| **LinkedIn** | LinkedIn OAuth 2.0 | Yes — user connects once |
| **X / Twitter** | X OAuth 2.0 (read-only) | Yes — user connects once |
| **Contra** | Contra OAuth / API key | Yes — user connects once |
| **Slack** | Slack OAuth (workspace install) | Yes — user selects channels |
| **Discord** | Bot invite + channel selection | Yes — user selects servers/channels |
| **Reddit** | Reddit OAuth (read-only) | Yes — user connects once |
| **Telegram** | Bot API token | Yes — user provides bot token |
| **Greenhouse, Lever, Ashby** | Public API (no auth) | No — auto-scanned |
| **Dribbble, Behance, Coroflot** | Public scraping / RSS | No — auto-scanned |
| **Indeed, Glassdoor, RemoteOK** | Public scraping | No — auto-scanned |
| **Web3 Career, Crypto Jobs** | Public API / scraping | No — auto-scanned |
| **Regional boards** | Public scraping | No — auto-scanned |

The key distinction: most job boards don't need the user to log in at all — the scanner reads public listings. Only platforms that require authenticated access (LinkedIn, Wellfound, Hired, Otta) need an OAuth flow. The Connections Hub makes this clear by separating "auto-scanned" boards from "login required" boards.

### Incomplete Setup Handling

If the user skips the wizard or leaves sources unconnected:

- Dashboard cards for disconnected sources show a soft prompt: "Connect LinkedIn to see social leads" with a one-tap connect button directly on the card
- A small status bar at the top of the dashboard shows: "4 sources not connected — [Complete setup]"
- No nagging — the prompt is visible but not blocking. The dashboard works with whatever is connected
- Weekly nudge (dismissable): "You're missing leads from Contra and LinkedIn. Connect in 30 seconds?"

### Data & Privacy

- All OAuth tokens are encrypted at rest and in transit
- Users can revoke any connection at any time — data from that source is purged within 24 hours
- No data is sold or shared with third parties
- Email content is processed for categorization only — no human reads it
- "Download my data" and "Delete my account" options available in settings

---

## Known Risks & Pre-Built Solutions

Every problem below is something that will happen — not might happen. Each one has a mitigation designed in from day one.

### 1. API Access & Rate Limits

**The Problem:** LinkedIn's API is notoriously restrictive — they don't hand out feed-reading access to indie apps. X's API costs money at scale. Contra may not have a public API at all. Half the "connections" in this spec depend on APIs that may throttle, paywall, or revoke access.

**Mitigation:**

- **LinkedIn:** Don't rely on the official API for feed monitoring. Use a browser-session approach — the user authenticates via their own LinkedIn session, and a headless browser (Puppeteer/Playwright) pulls their feed on a schedule. This is how tools like PhantomBuster and Dux-Soup operate. If LinkedIn blocks headless access, fall back to email notifications parsing — LinkedIn sends email digests of feed activity, which can be routed to the connected Gmail and parsed by the mail classifier. Additionally, offer a manual "paste a LinkedIn post URL" option so users can manually flag leads they spot.
- **X / Twitter:** Use API v2 filtered stream (Basic tier: $100/mo, 10K tweets/mo). For higher volume, supplement with Nitter-style RSS scraping as a fallback. Cache results aggressively — most design hiring tweets don't need real-time delivery, a 15-minute delay is fine. If the API cost becomes prohibitive at scale, implement a shared pool where trending design hiring tweets are fetched once and distributed to all users, rather than running per-user queries.
- **Contra:** If no public API exists, use authenticated browser scraping via the user's session. Contra's project brief pages are relatively simple HTML. As a secondary path, monitor Contra's email notifications through the connected Gmail (Contra sends digest emails for new matching briefs). Build a feedback loop: if Contra launches an official API later, migrate to it.
- **General rate-limit strategy:** Implement exponential backoff on all API calls. Stagger scan times across users (don't hit LinkedIn with 10,000 requests at exactly :00 every hour). Cache all results with a TTL matching the scan frequency. Show "last successful sync" timestamps so the user knows if a source is lagging.
- **API deprecation insurance:** Every source has a fallback tier. Tier 1: Official API. Tier 2: Authenticated scraping. Tier 3: Email notification parsing. Tier 4: RSS/Atom feed. If Tier 1 dies, the system auto-degrades to the next tier and surfaces a notice in the Connections Hub.

### 2. Scraping Fragility & Legal Risk

**The Problem:** Over half the job boards rely on web scraping. Sites change their HTML structure without notice. Anti-bot measures (Cloudflare, CAPTCHA) block scrapers. Some sites explicitly prohibit scraping in their ToS.

**Mitigation:**

- **Resilient scraping architecture:** Don't write brittle CSS-selector scrapers. Use an LLM-based extraction layer — feed the raw page HTML to a lightweight model that extracts structured job data (title, company, location, date, URL). When a site changes its layout, the LLM adapts without code changes. Only fall back to manual selector updates for edge cases.
- **Anti-bot rotation:** Use a residential proxy pool with rotating IPs and randomized user agents. Headless browsers should mimic real user behavior — random delays, scroll patterns, mouse movements. Use stealth plugins (puppeteer-extra-plugin-stealth).
- **Legal CYA:** Only scrape publicly visible job listings — the same data any visitor sees. Don't scrape behind login walls without user authentication. Don't store or redistribute scraped content beyond displaying it to the user. Include a "data sourced from [platform]" attribution link on every listing. Build a platform-specific blocklist — if any board sends a cease-and-desist, immediately disable scraping for that source and switch to their official channel (RSS, API, or manual).
- **Monitoring:** Automated health checks on every scraping target every 6 hours. If a scraper returns zero results or malformed data two cycles in a row, it's flagged as broken. The Connections Hub shows it as ⚠️ Warning with "Source may have changed — investigating." A developer is alerted to fix it.

### 3. Duplicate Job Listings

**The Problem:** The same job gets posted on LinkedIn, Greenhouse, Wellfound, and Dribbble simultaneously. Scanning 70+ boards means you'll surface the same "Senior Brand Designer at Monad" four times.

**Mitigation:**

- **Deduplication engine:** Every incoming listing is fingerprinted using a composite key: normalized company name + normalized job title + location. Fuzzy matching handles variations like "Sr. Brand Designer" vs "Senior Brand Designer" vs "Brand Designer, Senior" (Levenshtein distance + title normalization).
- **Canonical listing:** When duplicates are detected, merge them into a single card that shows all source platforms as badges: "Found on: Greenhouse · LinkedIn · Wellfound". The "Apply" button defaults to the original source (usually the company's ATS like Greenhouse) rather than the aggregator.
- **User-side merge:** If the system misses a duplicate, the user can manually merge two listings with a "Same job?" action. This feedback trains the dedup model over time.

### 4. Notification Overload

**The Problem:** Hourly scans across 70+ job boards, multiple social feeds, and email accounts will generate an absurd number of updates. If every new listing and tweet triggers a notification, the tool becomes noise — the exact problem it's supposed to solve.

**Mitigation:**

- **Tiered notification system:**
  - **🔴 Immediate push** — Only for exact-match, high-confidence results: a job at a company on your watchlist, a DM from a recruiter, a Contra brief that matches 90%+ of your profile tags. Expected volume: 0–5 per day.
  - **🟡 Hourly batch** — New listings and social leads grouped into a single hourly summary notification: "3 new jobs, 2 social leads since last check." Tapping opens the dashboard, not individual items.
  - **🟢 Daily digest** — The full morning summary. Everything from the last 24 hours, prioritized and categorized. This is the default for most content.
  - **⬜ Silent** — Low-priority items (promotions, tools you've already seen, low-match jobs) are logged on the dashboard but never trigger a notification.
- **Smart priority scoring:** Each incoming item gets a relevance score (0–100) based on: keyword match strength, company watchlist status, seniority alignment, recency, and engagement signals (e.g., a tweet with 500 likes asking for a designer ranks higher than one with 2 likes). Only items above the user's threshold (default: 70) trigger a push.
- **Notification budget:** Hard cap of 10 push notifications per day by default. User can adjust up or down. If the cap is hit, remaining items roll into the next digest.
- **Mute controls:** Mute by source, by keyword, by company. "Stop showing me Fiverr listings" = one tap.

### 5. Email Misclassification

**The Problem:** AI-powered mail sorting will miscategorize emails. A client invoice gets filed under "Promotions." A recruiter email lands in "Everything Else." Misclassification erodes trust fast — if users can't trust the categories, they'll ignore them.

**Mitigation:**

- **User correction loop:** Every email card has a "Wrong category?" action. One tap to move it to the right bucket. Every correction feeds back into the classifier — the model learns your specific patterns over time.
- **Sender rules:** After 2 corrections from the same sender, the system auto-creates a sender rule: "Emails from recruiter@monad.xyz always go to Job Updates." These rules are visible and editable in settings.
- **Confidence thresholds:** If the classifier confidence is below 70%, the email goes to "Needs Review" (a sub-section of Everything Else) rather than being auto-sorted into a wrong bucket. Better to say "I'm not sure" than to be confidently wrong.
- **Domain-based pre-rules:** Emails from known ATS domains (greenhouse.io, lever.co, ashbyhq.com) are auto-tagged as Job Updates before the AI classifier even runs. Emails from known billing platforms (Stripe, PayPal, Paystack, Wise) auto-tag as Bills. This catches the easy 60% with zero AI needed.
- **Weekly accuracy report:** A small footer in the Mail Overview card: "Sorted 342 emails this week · 97.4% accuracy · 9 manually corrected." Transparency builds trust.

### 6. OAuth Token Expiry & Silent Failures

**The Problem:** OAuth tokens expire. LinkedIn tokens last 60 days. Google tokens can be revoked. If a token dies silently, the user sees an empty card and thinks the feature is broken — not that they need to re-authenticate.

**Mitigation:**

- **Proactive refresh:** All OAuth tokens are refreshed automatically using refresh tokens before they expire. A background job checks token health daily and refreshes anything within 7 days of expiry.
- **Loud failure states:** If a token does expire or get revoked, the affected dashboard card immediately shows a clear state: "LinkedIn disconnected — [Reconnect in 1 tap]". Not a vague error, not a spinner, not an empty card. A specific, actionable message.
- **Connection health dashboard:** The System Status section in the Connections Hub shows a live readout of every source's auth status. Sources approaching token expiry show ⚠️ Amber with "Re-auth recommended" before they actually break.
- **Email alert on critical failure:** If Gmail or LinkedIn (the two highest-value sources) disconnect, send a push notification and an email to a secondary address: "Your Gmail connection dropped. Daily Digest can't sort your mail until you reconnect."

### 7. Data Freshness vs. Server Costs

**The Problem:** Hourly scans across 70+ sources for every user is expensive. At 1,000 users, that's 70,000+ API calls and scraping sessions per hour. At 10,000 users, it's untenable without serious infrastructure spend.

**Mitigation:**

- **Shared data pool:** Job listings from public boards are the same for all users. Scrape each board once per cycle, store the results in a shared index, then filter per-user based on their job profile. This means 70 scrapes per hour total — not 70 per user.
- **Per-user scanning only for authenticated sources:** Gmail, LinkedIn feed, Contra briefs, and Slack/Discord are user-specific and must be scanned per-user. Batch these calls efficiently and stagger them across the hour.
- **Smart scan scheduling:** Not all sources need hourly checks. Greenhouse and Lever post new jobs a few times a day, not every hour. Analyze each source's posting frequency and set dynamic scan intervals: high-velocity boards (LinkedIn, Indeed) scan hourly, low-velocity boards (AIGA, Krop) scan every 4–6 hours. Show the user the actual interval per source in the Connections Hub.
- **CDN caching for AI Radar:** The weekly AI tool roundup is the same for all users. Generate it once, cache it, serve it from CDN. Zero per-user cost.
- **Tiered pricing to offset costs:** Free tier scans every 4 hours. Paid tier scans hourly. Premium scans every 15 minutes or real-time. This funds the infrastructure naturally.

### 8. Mobile Bento Grid Responsiveness

**The Problem:** Bento grids look gorgeous on desktop and tablets. On a phone, they become an unreadable mess of tiny cards. A 3-column grid doesn't work on a 375px-wide screen.

**Mitigation:**

- **Responsive card stacking:** On screens below 768px, the bento grid collapses into a single-column scrollable feed. Cards maintain their identity but stack vertically in priority order. The user's custom card arrangement on desktop doesn't affect mobile — mobile has its own order (configurable separately).
- **Card priority for mobile:** Default mobile stack order: Focus Timer + Todo (top, always visible) → Mail Overview → Job Scanner → Social Leads → AI Radar. The most actionable cards are always above the fold.
- **Collapsible cards:** Each card can be collapsed to just its header on mobile: "📬 Mail (47 unread) ▾" — tap to expand. This keeps the feed scannable without endless scrolling.
- **Swipe gestures:** On mobile, swipe left on a job listing to save, swipe right to dismiss. Swipe down on a card to collapse it. These reduce tap-target friction on small screens.
- **PWA vs native:** If building as a PWA, ensure the manifest enables "standalone" display mode so it feels like a native app. If going native, build mobile-first and treat desktop as the scaled-up version, not the other way around.

### 9. Pomodoro Timer Conflicts with Notifications

**The Problem:** The user starts a 30-minute focus session. Eight minutes in, three job notifications pop up, a Contra brief arrives, and their Gmail shows 5 new work emails. The focus session is destroyed by the very tool meant to help them focus.

**Mitigation:**

- **Focus Mode / Do Not Disturb:** When a Pomodoro session is active, all non-critical notifications are silenced automatically. They're queued and delivered as a batch when the session ends or during the break. The dashboard still updates in the background — nothing is lost — but no push notifications, no badge count updates, no sounds.
- **Exception list:** Users can whitelist specific senders or sources that break through Focus Mode. E.g., "Always notify me if wale@021labs.com emails, even during focus."
- **Visual distinction:** During an active Pomodoro, the dashboard dims slightly or applies a subtle overlay on non-timer cards. This visually reinforces "you're in focus mode" without hiding information.
- **Post-session summary:** When the timer ends, a brief summary appears: "While you focused: 2 new jobs, 1 Contra brief, 4 emails. [Review now]"

### 10. Stale or Dead Job Listings

**The Problem:** Job boards are terrible at cleaning up filled positions. A listing posted 3 weeks ago might still show as "active" even though the role was filled on day 5. Users apply to dead listings, waste time, and lose trust in the scanner.

**Mitigation:**

- **Freshness decay:** Every listing gets a freshness score that decays over time. Listings older than 7 days are visually dimmed and tagged "May be filled." Listings older than 14 days drop to the bottom of the list. Listings older than 30 days are auto-archived unless still showing as "active" on the source.
- **Cross-reference validation:** If a listing disappears from its source URL (returns 404 or "position filled" page), it's immediately marked as "Closed" in the dashboard. The scanner checks listing URLs on every cycle.
- **Community signals:** If multiple users report a listing as filled (via a "Mark as filled" button), it's flagged for all users. Crowdsourced freshness.
- **Application tracking integration:** If you applied to a listing through the tool and receive a rejection email, the listing auto-updates to "Applied — Rejected" and stops appearing in your active feed.

### 11. Privacy & Security Risks

**The Problem:** The tool has access to Gmail, LinkedIn, X, Slack, Discord, and more. That's an extraordinary concentration of sensitive data. A breach would be catastrophic. Users will rightly be cautious about granting this much access.

**Mitigation:**

- **Minimal permission scopes:** Request only the narrowest OAuth scopes needed. Gmail: read-only (no send, no delete). LinkedIn: basic profile + feed read. X: read-only. Never request write access unless absolutely necessary for a specific feature (and even then, ask the user first).
- **Zero-knowledge architecture (goal):** Where possible, process data on-device rather than server-side. Mail classification can run locally using a lightweight on-device model. Job listings from the shared pool don't contain user data. Only authenticated source data (Gmail, LinkedIn) needs to transit through servers — and it should be encrypted end-to-end.
- **Transparent permissions page:** In the Connections Hub, every connected account shows exactly what permissions are granted: "Gmail: Read emails and labels. Cannot send, delete, or modify." Users can see and understand what they've authorized.
- **SOC 2 compliance path:** If this tool ever handles enterprise users or team data, SOC 2 Type II certification becomes necessary. Architect for it from day one: audit logs, encryption at rest, access controls, incident response procedures.
- **Regular third-party security audits:** Budget for annual penetration testing once the tool has paying users.

### 12. Information Overload on the Dashboard

**The Problem:** Even with a beautiful bento grid, showing mail counts, job listings, AI tools, social leads, a todo list, AND a timer on one screen can feel overwhelming — especially first thing in the morning.

**Mitigation:**

- **Progressive disclosure:** The default dashboard view shows summary counts, not full lists. "🎯 Jobs: 11 new" — not all 11 listings. The user drills in only when ready.
- **Priority-first ordering:** Every card sorts its content by relevance score. The top item in each card is always the most important one. If there are 11 new jobs, the #1 match is front and center.
- **Card visibility defaults:** New users start with 4 cards visible: Mail, Jobs, Todo+Timer, and Social Leads. AI Radar and Quick Stats are collapsed by default. Users expand them when they want to. First experience should feel focused, not fire-hosed.
- **Morning briefing mode:** An optional "briefing" screen that shows a single-column, 60-second summary before the full dashboard loads. Like a news anchor reading the top 5: "3 new jobs worth a look. 1 Contra brief that's a 95% match. 12 unread work emails. Your portfolio task is overdue." Tap through to the full dashboard when ready.
- **Zen mode:** A toggle that hides everything except the Todo list and Pomodoro timer. When you need to work, not scan. One tap to return to full dashboard.

### 13. Platform Dependency & Single Points of Failure

**The Problem:** Heavy reliance on Google OAuth (for Gmail), LinkedIn (for social leads), and X (for tweets). If Google changes their OAuth flow, or LinkedIn locks down further, or X/Twitter raises API prices again, core features break.

**Mitigation:**

- **Multi-provider email support:** Don't build exclusively for Gmail. Support IMAP from day one as a fallback for any email provider. If Google OAuth breaks, the user can still connect via IMAP credentials.
- **Source redundancy for social:** LinkedIn and X are the primary social feeds, but the system also monitors Slack, Discord, Reddit, and Telegram. If LinkedIn access degrades, the other sources still surface leads. No single platform is a single point of failure.
- **Self-hosted fallback path:** If all third-party APIs become untenable, offer a "bring your own data" mode where users can pipe in job listings via RSS, CSV import, or manual entry. The dashboard and todo/timer features still work without any external connections.
- **Abstracted integration layer:** Every source is connected through an adapter pattern. The core application never talks to LinkedIn directly — it talks to a "social feed adapter" that can be swapped out. If LinkedIn's API changes, only the adapter needs updating, not the whole application.

---

## Technical Approach

| Layer | Stack |
|---|---|
| **Auth** | OAuth 2.0 for Google, LinkedIn, X API, Contra API |
| **Mail Processing** | Gmail API → AI classification (fine-tuned model or rule-based + LLM fallback) |
| **Job Scraping** | Greenhouse/Lever/Ashby APIs + headless browser scraping for boards without APIs |
| **AI Tool Tracking** | Product Hunt API + RSS feeds + X search API |
| **Social Monitoring** | LinkedIn API (limited) + X API v2 filtered stream + Contra API + Slack/Discord bots |
| **Scan Frequency** | Hourly cron job across all sources. Push notification on high-priority matches |
| **Todo + Timer** | Local-first storage (IndexedDB / SQLite) with optional cloud sync |
| **Notifications** | Daily digest push (default: 7:00 AM WAT) + hourly alerts for high-priority matches |
| **Frontend** | Mobile-first PWA or native app — bento grid via CSS Grid |
| **Backend** | Node.js or Python service running scheduled jobs (cron) |
| **Theming** | CSS custom properties per theme, user-selectable in settings |

---

## User Settings

- **Connected accounts** — Add/remove Google accounts, LinkedIn, X, Contra, Slack workspaces, Discord servers
- **Scan frequency** — Hourly (default), 30 min, 15 min, or real-time for premium
- **Job filters** — Edit keywords, locations, seniority levels, industries
- **Social keywords** — Customize tracked hashtags and phrases
- **Digest time** — Choose when the daily summary is generated
- **Notification preferences** — Push, email summary, or both
- **AI Radar frequency** — Weekly (default) or daily
- **Theme** — Spaceship / DAW / OSINT
- **Pomodoro duration** — 25 / 30 / 35 / 40 minutes
- **Break duration** — 5 / 10 minutes
- **Bento layout** — Drag to rearrange, toggle card visibility

---

## Roadmap

**Phase 1 — Foundation**
Mail grouping + job board scanner + todo list with pomodoro timer. The daily essentials.

**Phase 2 — Intelligence**
AI tool radar + social feed monitor. The discovery and opportunity layer.

**Phase 3 — Actions**
One-tap apply, quick DM templates, saved listings, application tracker. Link tasks to digest items.

**Phase 4 — Themes + Polish**
Ship all three visual themes. Add ambient sound design. Micro-animations and transitions.

**Phase 5 — Analytics**
Weekly report: jobs viewed, applied to, leads engaged, focus time logged, response rates, streaks.

---

## Why This Matters

Every missed post is a potential client. Every buried email is a delayed response. Every late application is a closed door. This tool makes sure you see everything relevant — fast — and act on it before the window closes.
