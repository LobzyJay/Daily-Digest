# DAILY DIGEST — Prototype → Local Live Tool
> From single JSX artifact → full Next.js app on localhost

---

## CURRENT STATE
- **1 file**: `daily-digest.jsx` (~650 lines, React component)
- **All mock data**: hardcoded jobs, mails, news, social leads
- **Working UI**: bento grid, light/dark mode, ASCII dither, mouse trail, SVG line icons, smart sort algorithm, provider tabs (Gmail/Outlook/iCloud), account management modal, custom email templates, pomodoro timer, task list
- **No backend**: no persistence, no real APIs, no auth

## TARGET STATE
- Next.js 14 app on `localhost:3000`
- Real mail from Gmail + Outlook + iCloud
- Persistent tasks, templates, settings (SQLite — zero config)
- Live news via RSS feeds
- Spotify/Apple Music playback
- Job board aggregation
- One `npm run dev` and it works

---

# PHASE 0 — SCAFFOLD
**Time: ~30 min | DO FIRST**

### 0.1 Create project
```bash
npx create-next-app@latest daily-digest --typescript --tailwind --app --src-dir
cd daily-digest
```

### 0.2 Install dependencies
```bash
# Database (zero config, one file)
npm i better-sqlite3
npm i -D @types/better-sqlite3

# Mail providers
npm i googleapis                          # Gmail API
npm i @microsoft/microsoft-graph-client   # Outlook
npm i @azure/msal-node                    # Microsoft OAuth
npm i imapflow                            # iCloud IMAP

# Auth
npm i next-auth@beta

# Music
npm i spotify-web-api-node

# News
npm i rss-parser

# Jobs
npm i cheerio                             # scraping fallback

# Data fetching
npm i swr

# Utils
npm i zod date-fns
```

### 0.3 Target file structure
```
daily-digest/
├── src/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── page.tsx                      # Main dashboard
│   │   ├── globals.css                   # All CSS from artifact
│   │   └── api/
│   │       ├── auth/[...nextauth]/route.ts
│   │       ├── mail/
│   │       │   ├── route.ts              # GET all mail
│   │       │   ├── sync/route.ts         # POST force sync
│   │       │   └── [id]/read/route.ts    # PATCH mark read
│   │       ├── accounts/
│   │       │   ├── route.ts              # GET/POST accounts
│   │       │   └── [id]/route.ts         # DELETE account
│   │       ├── news/route.ts             # GET ?cat=design
│   │       ├── jobs/route.ts             # GET all jobs
│   │       ├── tasks/route.ts            # CRUD tasks
│   │       ├── templates/route.ts        # CRUD custom templates
│   │       ├── settings/route.ts         # GET/PATCH
│   │       └── music/
│   │           ├── auth/route.ts
│   │           └── now-playing/route.ts
│   ├── lib/
│   │   ├── db.ts                         # SQLite + migrations
│   │   ├── mail/
│   │   │   ├── gmail.ts
│   │   │   ├── outlook.ts
│   │   │   ├── icloud.ts
│   │   │   ├── categorize.ts
│   │   │   └── sort.ts
│   │   ├── news/feeds.ts
│   │   ├── jobs/scraper.ts
│   │   └── music/spotify.ts
│   ├── components/
│   │   ├── Dashboard.tsx
│   │   ├── Hero.tsx
│   │   ├── MailCard.tsx
│   │   ├── NewsCard.tsx
│   │   ├── JobsCard.tsx
│   │   ├── ColdEmailCard.tsx
│   │   ├── TimerCard.tsx
│   │   ├── TodoStatsCard.tsx
│   │   ├── SocialCard.tsx
│   │   ├── LogBar.tsx
│   │   ├── SettingsModal.tsx
│   │   ├── Icons.tsx                     # Ic object
│   │   └── ui/                           # Mo, Tag, Dot, Card, Head
│   └── hooks/
│       ├── useMail.ts
│       ├── useNews.ts
│       ├── useJobs.ts
│       ├── useTasks.ts
│       ├── useTemplates.ts
│       └── useMusic.ts
├── data/
│   └── digest.db                         # auto-created
├── .env.local
└── package.json
```

### 0.4 Create `.env.local`
```env
# ═══ GOOGLE (Gmail) ═══
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=

# ═══ MICROSOFT (Outlook) ═══
AZURE_CLIENT_ID=
AZURE_CLIENT_SECRET=
AZURE_TENANT_ID=common

# ═══ iCLOUD ═══
ICLOUD_EMAIL=
ICLOUD_APP_PASSWORD=

# ═══ SPOTIFY ═══
SPOTIFY_CLIENT_ID=
SPOTIFY_CLIENT_SECRET=

# ═══ APP ═══
NEXTAUTH_SECRET=     # run: openssl rand -base64 32
NEXTAUTH_URL=http://localhost:3000
```

---

# PHASE 1 — DATABASE + PERSISTENCE
**Time: ~1 hour | HIGH priority**

### 1.1 SQLite setup (`src/lib/db.ts`)
- [ ] Initialize `better-sqlite3` with WAL mode
- [ ] Auto-create tables: `accounts`, `mails`, `tasks`, `templates`, `settings`, `timer_sessions`
- [ ] Export `db` singleton

### 1.2 API routes for tasks
- [ ] `GET /api/tasks` → select all ordered by position
- [ ] `POST /api/tasks` → insert new task
- [ ] `PATCH /api/tasks` (body: `{id, done?, text?, position?}`) → update
- [ ] `DELETE /api/tasks` (body: `{id}`) → remove

### 1.3 API routes for templates
- [ ] `GET /api/templates` → all custom templates
- [ ] `POST /api/templates` → save (name, subject, body)
- [ ] `DELETE /api/templates` (body: `{id}`) → remove

### 1.4 API routes for settings
- [ ] `GET /api/settings` → all key-value pairs
- [ ] `PATCH /api/settings` → upsert key-value
- [ ] Persist: theme (light/dark), timer duration, connected accounts

### 1.5 API routes for accounts
- [ ] `GET /api/accounts` → list connected mail accounts
- [ ] `POST /api/accounts` → add new account
- [ ] `DELETE /api/accounts/:id` → disconnect

---

# PHASE 2 — MAIL INTEGRATION
**Time: ~3 hours | HIGH priority**

### 2.1 Gmail
- [ ] Register OAuth app: console.cloud.google.com → APIs & Services → Credentials
- [ ] Enable Gmail API
- [ ] OAuth consent screen → External → Scopes: `gmail.readonly`, `gmail.modify`
- [ ] Create OAuth 2.0 Client ID (Web) → Redirect: `http://localhost:3000/api/auth/callback/google`
- [ ] `src/lib/mail/gmail.ts`:
  - `authenticate(code)` → exchange for tokens
  - `getMessages(accessToken, max=20)` → list + batch get
  - `markAsRead(accessToken, msgId)`
  - Returns: `{ id, from, fromName, subject, snippet, threadId, date, isUnread }`
- [ ] Deeplink: `https://mail.google.com/mail/u/0/#inbox/{messageId}`

### 2.2 Outlook
- [ ] Register: portal.azure.com → App registrations
- [ ] Platform: Web → Redirect: `http://localhost:3000/api/auth/callback/azure-ad`
- [ ] Permissions: `Mail.Read`, `Mail.ReadWrite`, `User.Read`
- [ ] `src/lib/mail/outlook.ts`:
  - Uses `@microsoft/microsoft-graph-client`
  - `getMessages(accessToken)` → `/me/messages?$top=20&$orderby=receivedDateTime desc`
  - Same return shape
- [ ] Deeplink: `https://outlook.live.com/mail/0/inbox/id/{messageId}`

### 2.3 iCloud
- [ ] Generate App-Specific Password: appleid.apple.com → Security
- [ ] `src/lib/mail/icloud.ts`:
  - Uses `imapflow` → connect to `imap.mail.me.com:993`
  - Fetch INBOX headers (FROM, SUBJECT, DATE, FLAGS)
  - Same return shape
- [ ] Deeplink: `https://www.icloud.com/mail/` (no per-message link)

### 2.4 Auto-categorization (`src/lib/mail/categorize.ts`)
- [ ] Rule-based regex matching (jobs/bills/promo/work)
- [ ] Fallback: default to 'work'

### 2.5 Smart sort (`src/lib/mail/sort.ts`)
- [ ] Port from prototype: priority(50%) + recency with 48h decay(30%) + unread(20%)
- [ ] Run server-side on real data

### 2.6 Sync flow
- [ ] `POST /api/mail/sync` → fetch all accounts → categorize → score → store in SQLite
- [ ] `GET /api/mail?provider=all|google|outlook|icloud` → return sorted from DB
- [ ] Frontend: `useMail()` hook with SWR, refreshInterval 60s

---

# PHASE 3 — NEWS FEEDS (LIVE RSS)
**Time: ~1 hour | MEDIUM priority**

### 3.1 Feed sources
```
DESIGN:   figma.com/blog/feed, creativebloq.com/feed, itsnicethat.com/feed
DEEP TECH: nature.com/nature.rss, technologyreview.com/feed, arstechnica.com/feed
SPORT:    espn.com/rss/news, bbc.co.uk/sport/rss.xml
OSINT:    bellingcat.com/feed, citizenlab.ca/feed, icij.org/feed
```

### 3.2 Implementation
- [ ] `src/lib/news/feeds.ts` — use `rss-parser` to fetch + parse
- [ ] `GET /api/news?cat=design` → fetch RSS, deduplicate, return latest 10
- [ ] Cache in SQLite with 15-min TTL
- [ ] Frontend: `useNews(category)` hook replacing hardcoded `NEWS` object

---

# PHASE 4 — SPOTIFY
**Time: ~1 hour | MEDIUM priority**

- [ ] Register: developer.spotify.com/dashboard
- [ ] Redirect: `http://localhost:3000/api/music/auth/callback`
- [ ] Scopes: `user-read-playback-state`, `user-read-currently-playing`, `user-modify-playback-state`
- [ ] `GET /api/music/now-playing` → `{ track, artist, albumArt, progress, duration, isPlaying }`
- [ ] `POST /api/music/play` + `POST /api/music/pause`
- [ ] Frontend: poll every 5s, drive MusicWidget

---

# PHASE 5 — JOBS
**Time: ~2 hours | MEDIUM priority**

- [ ] **Greenhouse**: `https://boards-api.greenhouse.io/v1/boards/{company}/jobs` (public, no auth)
- [ ] **Lever**: `https://api.lever.co/v0/postings/{company}` (public)
- [ ] **Wellfound**: Cheerio scrape as fallback
- [ ] Maintain `job_sources.json` with company slugs to track
- [ ] Match score: keyword match (brand, UI/UX, motion, Web3, crypto)
- [ ] Chance score: seniority fit + time posted + competition estimate
- [ ] Cache in SQLite, refresh every 4 hours
- [ ] `GET /api/jobs` → sorted, scored results

---

# PHASE 6 — UI MIGRATION
**Time: ~2 hours | HIGH — do alongside Phase 1**

### 6.1 Split the monolith
- [ ] Extract each component from `daily-digest.jsx` → `src/components/*.tsx`
- [ ] Extract all CSS → `src/app/globals.css`
- [ ] Extract `Ic` icons → `src/components/Icons.tsx`
- [ ] Extract `Mo`, `Tag`, `Dot`, `Card`, `Head` → `src/components/ui/`
- [ ] Move palette / CSS variables into `globals.css` `:root`

### 6.2 Wire up to real APIs
- [ ] `ALL_MAIL` → `useMail()` → `GET /api/mail`
- [ ] `NEWS` → `useNews()` → `GET /api/news`
- [ ] `JOBS` → `useJobs()` → `GET /api/jobs`
- [ ] `INIT_TODOS` → `useTasks()` → `GET/POST/PATCH /api/tasks`
- [ ] Templates → `useTemplates()` → `GET/POST/DELETE /api/templates`
- [ ] Accounts → `useAccounts()` → `GET/POST/DELETE /api/accounts`

### 6.3 SWR hooks pattern
```typescript
import useSWR from 'swr';
const f = (url: string) => fetch(url).then(r => r.json());

export function useMail(provider = 'all') {
  const { data, error, mutate } = useSWR(`/api/mail?provider=${provider}`, f, { refreshInterval: 60000 });
  return { mails: data?.mails || [], loading: !data && !error, mutate };
}
```

---

# PHASE 7 — POLISH
**Time: ~1 hour | LOW — do last**

- [ ] Loading skeletons per card
- [ ] Error states (disconnected provider, rate limit)
- [ ] Toast notifications (synced, task done, mail sent)
- [ ] Keyboard shortcuts: `Cmd+K` quick actions, `Cmd+Shift+T` toggle tasks
- [ ] PWA manifest → installable as desktop app
- [ ] Optional: deploy to Vercel for remote access

---

# PRIORITY ORDER

| # | Phase | What | Time |
|---|-------|------|------|
| 1 | 0 | Scaffold Next.js project | 30m |
| 2 | 6.1 | Split JSX into components, get it rendering | 2h |
| 3 | 1 | SQLite — tasks, templates, settings persist | 1h |
| 4 | 2.1 | Gmail integration (do this one first) | 2h |
| 5 | 3 | RSS news feeds (quick win, real data fast) | 1h |
| 6 | 4 | Spotify now playing | 1h |
| 7 | 2.2 | Outlook integration | 1h |
| 8 | 2.3 | iCloud integration | 30m |
| 9 | 5 | Job board aggregation | 2h |
| 10 | 7 | Polish, PWA, deploy | 1h |

**Total: ~12 hours of focused work**

---

# START RIGHT NOW

```bash
# 1
npx create-next-app@latest daily-digest --typescript --tailwind --app --src-dir
cd daily-digest

# 2
npm i better-sqlite3 googleapis @azure/msal-node @microsoft/microsoft-graph-client imapflow spotify-web-api-node rss-parser cheerio swr zod date-fns
npm i -D @types/better-sqlite3

# 3
mkdir -p data

# 4 — create .env.local, paste template above, fill in keys

# 5 — start splitting daily-digest.jsx into src/components/

# 6
npm run dev
# → http://localhost:3000
```

---

# OAUTH REGISTRATION LINKS

| Provider | URL |
|----------|-----|
| Google (Gmail) | https://console.cloud.google.com/apis/credentials |
| Microsoft (Outlook) | https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps |
| Apple (iCloud) | https://appleid.apple.com/account/manage |
| Spotify | https://developer.spotify.com/dashboard |
