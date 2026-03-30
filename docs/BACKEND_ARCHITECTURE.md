# Daily Digest — Backend Architecture & Plugin System

## What's needed to make this real

---

## 1. AUTH & ACCOUNTS

### Mail Providers (OAuth2)
| Provider | OAuth Scope | Endpoint |
|----------|------------|----------|
| **Gmail** | `gmail.readonly`, `gmail.modify` | Google OAuth 2.0 → Gmail API v1 |
| **Outlook** | `Mail.Read`, `Mail.ReadWrite` | Microsoft Identity Platform → Graph API |
| **iCloud** | App-Specific Password + IMAP | IMAP `imap.mail.me.com:993` (no OAuth) |

### Music (OAuth2)
- **Spotify**: `user-read-playback-state`, `user-read-currently-playing` → Web Playback SDK
- **Apple Music**: MusicKit JS + Developer Token → Apple Music API

### Social / Leads
- **LinkedIn**: Marketing API or Voyager (unofficial) for DMs/mentions
- **X/Twitter**: API v2 — `tweet.read`, `dm.read`, `users.read`
- **Contra**: No public API — scrape or webhook

### Job Boards
- **Greenhouse**: Harvest API (if employer) or Job Board API
- **Lever**: Postings API (public), Opportunities API (auth)
- **Wellfound**: No public API — RSS + scraper
- **LinkedIn Jobs**: No direct API — use unofficial or aggregator

---

## 2. BACKEND STACK

```
┌────────────────────────────────────────────┐
│              DAILY DIGEST APP              │
├────────────────────────────────────────────┤
│  Frontend: React (current JSX)             │
│  State: Zustand or Redux Toolkit           │
│  Auth: NextAuth.js (multi-provider)        │
├────────────────────────────────────────────┤
│  API Layer: Next.js API Routes / Express   │
│  Queue: BullMQ + Redis (sync jobs)         │
│  Cron: node-cron (polling intervals)       │
├────────────────────────────────────────────┤
│  Database: PostgreSQL (Supabase or Neon)   │
│  Cache: Redis (mail cache, rate limits)    │
│  Storage: S3/R2 (profile photos, assets)   │
├────────────────────────────────────────────┤
│  Hosting: Vercel (frontend + API)          │
│  Workers: Railway / Fly.io (background)    │
│  Secrets: Vercel env / Doppler             │
└────────────────────────────────────────────┘
```

---

## 3. MAIL SYNC ENGINE

### Polling Strategy
```
Gmail:    Every 60s via Gmail API watch() push notifications
Outlook:  Every 60s via Graph API subscriptions (webhooks)
iCloud:   Every 120s via IMAP IDLE (persistent connection)
```

### Smart Sort Algorithm (production version)
```javascript
function computePriority(mail, userContext) {
  let score = 0;
  
  // 1. SENDER WEIGHT (0–30pts)
  // Known contacts from CRM / past replies get higher weight
  if (userContext.vipSenders.has(mail.from)) score += 30;
  else if (userContext.repliedTo.has(mail.from)) score += 20;
  else if (mail.from.endsWith(userContext.workDomains)) score += 15;
  
  // 2. CONTENT SIGNALS (0–30pts)
  // NLP keywords: "interview", "urgent", "deadline", "payment"
  const urgentKeywords = /interview|urgent|deadline|due|payment|offer|approved/i;
  const promoKeywords = /unsubscribe|newsletter|digest|weekly|promo/i;
  if (urgentKeywords.test(mail.subject + mail.snippet)) score += 25;
  if (promoKeywords.test(mail.subject + mail.snippet)) score -= 15;
  if (mail.hasAttachments) score += 5;
  
  // 3. RECENCY (0–25pts)
  // Exponential decay — 48h half-life
  const hoursAgo = (Date.now() - mail.timestamp) / 3600000;
  score += 25 * Math.exp(-hoursAgo / 48);
  
  // 4. UNREAD BOOST (0–15pts)
  if (mail.unread) score += 15;
  
  // 5. THREAD ACTIVITY (0–10pts)
  // Multi-reply threads get boosted
  if (mail.threadLength > 3) score += 10;
  else if (mail.threadLength > 1) score += 5;
  
  // 6. CATEGORY CONTEXT
  // Job-related during active job search gets boosted
  if (userContext.activeJobSearch && mail.category === 'jobs') score += 10;
  
  return Math.min(100, Math.max(0, score));
}
```

### Database Schema
```sql
CREATE TABLE mail_accounts (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  provider TEXT NOT NULL, -- 'google' | 'outlook' | 'icloud'
  email TEXT NOT NULL,
  access_token TEXT ENCRYPTED,
  refresh_token TEXT ENCRYPTED,
  last_sync TIMESTAMPTZ,
  status TEXT DEFAULT 'active'
);

CREATE TABLE mails (
  id UUID PRIMARY KEY,
  account_id UUID REFERENCES mail_accounts(id),
  provider_id TEXT NOT NULL, -- Gmail/Outlook message ID
  from_addr TEXT,
  from_name TEXT,
  subject TEXT,
  snippet TEXT,
  category TEXT, -- 'jobs' | 'work' | 'bills' | 'promo'
  priority_score INT DEFAULT 50,
  is_unread BOOLEAN DEFAULT true,
  has_attachments BOOLEAN DEFAULT false,
  thread_id TEXT,
  thread_length INT DEFAULT 1,
  received_at TIMESTAMPTZ,
  synced_at TIMESTAMPTZ DEFAULT NOW(),
  deeplink TEXT -- direct URL to open in provider
);

CREATE INDEX idx_mails_priority ON mails(account_id, priority_score DESC);
CREATE INDEX idx_mails_unread ON mails(account_id, is_unread, received_at DESC);
```

### Auto-Categorization
```javascript
// Rule-based + ML fallback
const categorize = (mail) => {
  const patterns = {
    jobs: /greenhouse|lever|hired|interview|application|recruiter|hiring|position|role|salary/i,
    bills: /invoice|payment|receipt|billing|subscription|renewal|charge/i,
    promo: /unsubscribe|newsletter|digest|weekly|deal|offer|sale|marketing/i,
  };
  for (const [cat, regex] of Object.entries(patterns)) {
    if (regex.test(mail.subject + ' ' + mail.from)) return cat;
  }
  return 'work'; // default
};
```

---

## 4. PLUGIN SYSTEM — Floating Widgets

### Architecture
Plugins are standalone React components that render as draggable, always-on-top windows. They persist position via localStorage and communicate via a shared event bus.

```javascript
// Plugin manifest
{
  id: "pomodoro",
  name: "Focus Timer",
  icon: "◈",
  component: PomodoroPlugin,
  defaultSize: { w: 280, h: 180 },
  defaultPos: { x: 20, y: 20 },
  alwaysOnTop: true,
  resizable: true,
}
```

### Built-in Plugins

#### 1. Pomodoro / Focus Timer (always visible)
```
┌─────────────────────────┐
│ ◈ FOCUS         ─ □ ✕  │
├─────────────────────────┤
│                         │
│       25:00             │
│    ▶ FOCUS MODE         │
│                         │
│  [START] [SKIP] [RESET] │
│  Sessions: 4 today      │
│  ■■■■□□□□ 50% of goal  │
└─────────────────────────┘
```
- Configurable work/break durations
- Desktop notifications on timer end
- Blocks distracting tabs during focus (browser ext)
- Syncs session count to dashboard Stats card

#### 2. Quick Tasks (always visible)
```
┌─────────────────────────┐
│ ☷ TASKS         ─ □ ✕  │
├─────────────────────────┤
│ ● Reply to Monad        │
│ ● Send invoice — 021    │
│ ○ Post case study       │
│ + Add task...            │
│─────────────────────────│
│ 2/5 done today          │
└─────────────────────────┘
```
- Drag to reorder
- Priority dots (urgent/important/normal)
- Syncs with main dashboard TodoStatsCard
- Keyboard shortcut: Cmd+Shift+T to toggle

#### 3. Now Playing (mini music widget)
```
┌──────────────────┐
│ ♫ Sault — Untit… │
│ ■■▓░░ 2:41/4:03 │
│  ❚❚  ⟫          │
└──────────────────┘
```

### Plugin API
```javascript
// Register a plugin
PluginManager.register({
  id: 'my-plugin',
  name: 'My Plugin',
  component: MyComponent,
  // Lifecycle hooks
  onMount: (ctx) => { /* access shared state */ },
  onUnmount: () => { /* cleanup */ },
  // Communication
  events: {
    'timer:complete': (data) => { /* react to focus timer */ },
    'task:added': (data) => { /* react to new task */ },
  }
});

// Shared context available to all plugins
const PluginContext = {
  user: { name, email, preferences },
  mail: { unreadCount, latestMails },
  tasks: { todos, completedToday },
  timer: { isRunning, timeLeft, sessionsToday },
  emit: (event, data) => {}, // broadcast to other plugins
};
```

### Desktop App (Electron / Tauri)
For always-on-screen plugins, wrap in Tauri (lightweight):
```
tauri.conf.json → transparent: true, alwaysOnTop: true, decorations: false
```
- Tray icon with quick actions
- Global hotkeys (Cmd+Shift+D = toggle digest)
- System notifications for high-priority mail
- Menu bar widget mode (macOS)

---

## 5. API ENDPOINTS

```
POST   /api/auth/connect/:provider    — OAuth flow start
GET    /api/auth/callback/:provider   — OAuth callback
DELETE /api/auth/disconnect/:provider — Remove provider

GET    /api/mail                      — All mail (sorted)
GET    /api/mail?provider=google      — Filter by provider
GET    /api/mail?category=jobs        — Filter by category
POST   /api/mail/sync                 — Force sync all accounts
PATCH  /api/mail/:id/read             — Mark as read

GET    /api/jobs                      — Aggregated job listings
GET    /api/news/:category            — News by category
GET    /api/social/leads              — Social lead matches

GET    /api/plugins                   — List installed plugins
POST   /api/plugins/:id/state         — Save plugin state
```

---

## 6. ENV VARIABLES NEEDED

```env
# Database
DATABASE_URL=postgresql://...
REDIS_URL=redis://...

# Google OAuth
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=

# Microsoft OAuth
AZURE_CLIENT_ID=
AZURE_CLIENT_SECRET=

# iCloud
ICLOUD_APP_PASSWORD=

# Spotify
SPOTIFY_CLIENT_ID=
SPOTIFY_CLIENT_SECRET=

# Music
APPLE_MUSIC_DEV_TOKEN=

# Jobs APIs
GREENHOUSE_API_KEY=
LEVER_API_KEY=

# App
NEXTAUTH_SECRET=
NEXTAUTH_URL=https://digest.layoutstudio.co
```

---

## 7. DEPLOYMENT CHECKLIST

- [ ] Set up Supabase/Neon PostgreSQL
- [ ] Configure Redis (Upstash for serverless)
- [ ] Register OAuth apps: Google, Microsoft, Spotify
- [ ] Set up iCloud App-Specific Password flow
- [ ] Deploy Next.js to Vercel
- [ ] Deploy background workers to Railway
- [ ] Configure webhooks for Gmail push notifications
- [ ] Set up Graph API subscriptions for Outlook
- [ ] Build Tauri desktop wrapper for plugins
- [ ] Set up Sentry error tracking
- [ ] Configure rate limiting per provider
