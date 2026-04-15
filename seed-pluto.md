# 🌱 SEED: Pluto — The Friendly Watchdog from Hades

**Version:** 2.1
**Depends on:** `seed-supabase.md` (Shape A — Next.js SSR Pattern)
**Purpose:** A registry with a pulse. Every gem, app, script, and MCP server Kydroon has built gets a card. Some cards have vital signs. All cards have a home.

**What changed in v2.1:** RLS enabled on all Pluto tables. Custom domain deployment changed the threat model — the anon key is now publicly callable. Read-only RLS for anon, service role bypasses for cron writes.

**What changed in v2.0:** 29 systems (was 24), AI wallet tracking, APEX hero card with cross-instance Supabase, parallelized cron, Basic Auth, per-system health endpoints, migration pattern for adding systems.

---

## The Problem (Don't Lose This)

> "I have so many gems and tools to watch, losing overview."

This is not a monitoring problem. This is an **inventory problem with a health dimension.** The root cause: a prolific builder with no single view of what exists. Pluto solves inventory first, health second.

---

## Core Concept: Registry + Heartbeats

**Registry** = "What exists?" — the inventory of everything built.
**Heartbeats** = "Is it alive? Is it used? Is it growing?" — the vital signs.

Not everything in the registry will have heartbeats. A local script doesn't ping a server. An MCP server doesn't have a deploy pipeline. That's fine. Pluto shows everything, with whatever signal is available.

### The Two-Lens Model

Every system is measured on two orthogonal axes:

- **Usage Pulse:** Am I actually using this thing? (The consumer lens)
- **Development Pulse:** Am I actually building this thing? (The builder lens)

### The Quadrant

```
              DEVELOPMENT →
         Low                 High
    ┌──────────────────┬──────────────────┐
    │                  │                  │
    │   💀 DORMANT     │  🔨 BUILDING    │  USAGE
    │   Not used,      │  Actively built  │    ↓
    │   not built      │  but not consumed│
    ├──────────────────┼──────────────────┤
    │                  │                  │
    │   ⚓ STABLE      │  🔥 THRIVING    │
    │   Used, not      │  Used AND built  │
    │   changing       │  actively        │
    └──────────────────┴──────────────────┘
```

The algorithm is deliberately simple. Don't build a scoring engine:
- `commits_7d > 0` → development active
- any of `page_views_24h`, `visitors_24h`, `api_calls_24h`, `events_7d` > 0 → usage active

---

## Dashboard Layout

```
┌─────────────────────────────────────────────┐
│  AI WALLETS  [Anthropic API] [Max] [OpenAI]  │  ← spend tracker, top of page
│              [Gemini] [Perplexity] [xAI]     │
├─────────────────────────────────────────────┤
│                                             │
│  APEX RECRUITER  [DORMANT]       hero card  │  ← full-width, 7-heartbeat strip
│  DEV PULSE  |  USAGE PULSE  |  COMMENTARY  │
│                                             │
├─────────────────────────────────────────────┤
│  🔥 Thriving (N)  [card] [card] [card]      │  ← quadrant grid
│  🔨 Building (N)  [card] [card] [card]      │
│  ⚓ Stable  (N)   [card] [card] [card]      │
│  💀 Dormant (N)   [card] [card] [card]      │
└─────────────────────────────────────────────┘
│  Last pulse: <timestamp from DB>  •  Refreshing every 15 min  │
```

**APEX always gets the hero treatment.** It is excluded from the quadrant grid. All other 28 systems appear in the grid.

**The footer shows the last heartbeat `checked_at` from the DB** — not `new Date()` (which would only show page render time, which is misleading).

---

## Data Model

Uses the Supabase conventions from `seed-supabase.md`. Shape A (Next.js SSR).

### Table: `pluto_systems` — The Registry

```sql
CREATE TABLE pluto_systems (
  id TEXT PRIMARY KEY,                -- 'apex-recruiter', 'khaos-researcher', 'hondius'
  name TEXT NOT NULL,                 -- 'APEX Recruiter'
  type TEXT NOT NULL CHECK (type IN (
    'app', 'site', 'mcp', 'script', 'workflow', 'agent', 'library', 'api'
  )),
  url TEXT,                           -- Live URL if deployed (nullable)
  repo TEXT,                          -- GitHub repo path: 'cotoaga/apex-recruiter'
  vercel_project TEXT,                -- Vercel project name (nullable)
  health_endpoint TEXT,               -- Full URL to health endpoint (nullable)
  description TEXT,                   -- One-liner: what it does
  stack JSONB DEFAULT '[]',           -- ['next.js', 'supabase', 'tailwind']
  status TEXT DEFAULT 'active' CHECK (status IN (
    'active', 'archived', 'experimental', 'deprecated'
  )),
  notes TEXT,
  registered_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

**Why TEXT primary key:** System IDs are human-readable slugs (`apex-recruiter`, `industrial-ai-complex`). You'll type them, read them, reference them. UUIDs would be hostile here.

Multiple systems can share the same `repo` (e.g. `cotoaga-ai-gems`, `khaos-questor`, and `industrial-ai-complex` all point to `cotoaga/cotoaga-ai-gems`). This is fine — GitHub commits are fetched per system, not deduplicated. Harmless redundancy.

### Table: `pluto_heartbeats` — The Pulse (append-only)

```sql
CREATE TABLE pluto_heartbeats (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  system_id TEXT NOT NULL REFERENCES pluto_systems(id) ON DELETE CASCADE,

  dev_pulse JSONB DEFAULT '{}',
  -- Shape: {
  --   commits_7d: number,
  --   last_commit_at: ISO string | null,
  --   last_commit_message: string | null,
  --   deploys_7d: number,
  --   last_deploy_at: ISO string | null,
  --   build_status: 'success' | 'error' | 'unknown',
  --   open_prs: number
  -- }

  usage_pulse JSONB DEFAULT '{}',
  -- Shape varies per system. Common fields from health endpoints:
  -- { source: 'health_endpoint', page_views_24h: N, visitors_24h: N, last_usage_at: ISO }
  -- APEX: { source: 'apex', events_7d: N, unique_actors_7d: N, event_types: [...] }
  -- Libraries/scripts: { source: 'none' }

  health_status TEXT CHECK (health_status IN ('healthy', 'degraded', 'down', 'unknown')),
  health_response_ms INTEGER,

  quadrant TEXT CHECK (quadrant IN ('thriving', 'stable', 'building', 'dormant')),
  quadrant_override BOOLEAN DEFAULT false,

  checked_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_pluto_heartbeats_system ON pluto_heartbeats(system_id, checked_at DESC);
```

**Never update heartbeats in place.** Always insert a new row. The dashboard reads `ORDER BY checked_at DESC LIMIT 1` per system. Historical rows accumulate — clean up with a Supabase cron if needed (keep last 30 days).

### RLS: Required on All Tables

`NEXT_PUBLIC_SUPABASE_ANON_KEY` is exposed in the browser bundle. Anyone with it can call your Supabase REST API directly, bypassing Next.js and any middleware auth. Enable RLS on every table and grant anon only `SELECT`:

```sql
ALTER TABLE pluto_systems     ENABLE ROW LEVEL SECURITY;
ALTER TABLE pluto_heartbeats  ENABLE ROW LEVEL SECURITY;
ALTER TABLE pluto_wallets     ENABLE ROW LEVEL SECURITY;

CREATE POLICY "pluto_systems_anon_read"
  ON pluto_systems FOR SELECT TO anon USING (true);

CREATE POLICY "pluto_heartbeats_anon_read"
  ON pluto_heartbeats FOR SELECT TO anon USING (true);

CREATE POLICY "pluto_wallets_anon_read"
  ON pluto_wallets FOR SELECT TO anon USING (true);
```

No `INSERT` / `UPDATE` / `DELETE` policies for anon. The cron uses the service role key (`createAdminClient()`), which bypasses RLS automatically — no cron changes needed.

### Table: `pluto_wallets` — AI Provider Spend

```sql
CREATE TABLE pluto_wallets (
  id              TEXT PRIMARY KEY,  -- 'anthropic-api', 'openai', 'gemini', etc.
  provider        TEXT NOT NULL,
  console_url     TEXT,
  plan            TEXT NOT NULL CHECK (plan IN ('credits', 'subscription', 'pay-as-you-go')),
  budget_usd      NUMERIC(10,2),     -- monthly budget (NULL = unknown/unlimited)
  spent_usd       NUMERIC(10,2),     -- auto-computed for Anthropic, NULL for others
  balance_usd     NUMERIC(10,2),     -- manual for others, computed for Anthropic
  period_start    DATE,
  update_source   TEXT NOT NULL CHECK (update_source IN ('auto', 'manual')) DEFAULT 'manual',
  last_updated_at TIMESTAMPTZ DEFAULT NOW(),
  notes           TEXT
);
```

Seed 6 rows: `anthropic-api` (auto), `anthropic-max`, `openai`, `gemini`, `perplexity`, `xai` (all manual).

`anthropic-api` is auto-updated every 15 min by the wallets cron via the Anthropic Admin API `cost_report` endpoint. All others are updated manually in the Supabase dashboard. The wallet card shows "N weeks ago" when `last_updated_at` is stale — this is the signal to go update it manually.

---

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                   PLUTO DASHBOARD                    │
│           khaos-pluto.cotoaga.ai (Vercel)            │
│                                                      │
│  Server components. All data fetched at render time. │
│  No client-side fetching. No useEffect.              │
│                                                      │
│  createClient() calls cookies() → always dynamic.    │
│  Do NOT add export const dynamic = 'force-dynamic'.  │
└──────────────┬───────────────────────────────────────┘
               │ reads (anon key)
               ▼
      ┌──────────────────┐        ┌─────────────────────┐
      │    SUPABASE       │        │   APEX SUPABASE      │
      │  pluto_systems    │        │  (separate instance) │
      │  pluto_heartbeats │        │  khaos_events table  │
      │  pluto_wallets    │        └──────────┬──────────┘
      └────────┬──────────┘                   │
               │ writes (service role)         │ reads (service role)
               ▼                              │
      ┌──────────────────┐                   │
      │  VERCEL CRON     │◄──────────────────┘
      │  /api/cron/pulse │   (apex-recruiter only)
      │  Every 15 min    │
      │  Promise.all     │ ← all 29 systems in PARALLEL
      └────────┬─────────┘
               │ fetches from (per system, all concurrent)
               ▼
      ┌──────────────────────────┐
      │  GitHub API              │ → commits/7d, open PRs
      │  Vercel API              │ → deploys/7d, build status
      │  Health endpoints        │ → page views, visitors, api_calls
      └──────────────────────────┘

      ┌──────────────────┐
      │  VERCEL CRON     │
      │  /api/cron/wallets│  Every 15 min
      └────────┬─────────┘
               │
               ▼
      ┌──────────────────────────┐
      │  Anthropic Admin API     │ → cost_report (current month)
      └──────────────────────────┘
```

---

## Cron Jobs

Both defined in `vercel.json`. Both protected by `Authorization: Bearer <CRON_SECRET>`.

```json
{
  "crons": [
    { "path": "/api/cron/pulse",   "schedule": "*/15 * * * *" },
    { "path": "/api/cron/wallets", "schedule": "*/15 * * * *" }
  ],
  "functions": {
    "app/api/cron/**": { "maxDuration": 300 }
  }
}
```

**`maxDuration: 300` is mandatory.** Without it, Vercel defaults to 10s (Hobby) or 60s (Pro). Processing 29 systems with parallel API calls still needs headroom. 300s = 5 minutes, the Vercel maximum.

### `/api/cron/pulse` — the heartbeat

```
1. Validate CRON_SECRET from Authorization header
2. Fetch all active systems with repos from pluto_systems
3. For each system in parallel (Promise.allSettled):
   a. GitHub commits (7d) — sequential first (required for devPulse)
   b. GitHub open PRs + Vercel deploys — parallel with each other
   c. Health endpoint — if set, 5s AbortSignal timeout
   d. APEX special case — if system.id === 'apex-recruiter':
      query khaos_events on separate Supabase instance
4. computeQuadrant(devPulse, usagePulse) → quadrant
5. Insert heartbeat row
6. Return { checked, total, errors[] }
```

**Use `Promise.allSettled`, not `Promise.all`.** One failing system must not abort the others. Per-system errors are logged and returned in the summary, but the cron always completes.

**Never revert to a sequential `for...of` loop.** 29 systems × 3+ API calls each = guaranteed timeout on any reasonable Vercel plan. This was learned the hard way.

### `/api/cron/wallets` — the spend tracker

```
1. Validate CRON_SECRET
2. Call Anthropic Admin API: GET /v1/organizations/cost_report?bucket_width=1d
3. Sum daily cost buckets for the current month → spent_usd
4. Update pluto_wallets WHERE id = 'anthropic-api'
5. Return { status, anthropic_spent_usd, period }
```

Requires `ANTHROPIC_ADMIN_API_KEY` (Admin API key, not the regular API key). If not set, returns 500 immediately.

---

## APEX Special Case

APEX Recruiter (`apex-recruiter`) is the flagship system. It gets different treatment at every layer:

**Dashboard:** Full-width hero card at the top of the page. NOT in the quadrant grid. Shows a 7-heartbeat strip chart, full dev pulse zone, full usage pulse zone, and KHAOS commentary (signal detection).

**Cron:** Queries `khaos_events` on APEX's own Supabase instance (separate URL + service key). Results go into `usage_pulse.events_7d` and `usage_pulse.unique_actors_7d`. Falls back gracefully if APEX env vars are missing.

**Signals:** `lib/signals.ts` runs 6 detection rules on the APEX heartbeat to generate commentary text shown on the hero card.

```typescript
// lib/supabase/apex.ts
export function createApexClient() {
  return createClient(
    process.env.APEX_SUPABASE_URL!,
    process.env.APEX_SUPABASE_SERVICE_KEY!
  )
}
```

---

## Health Endpoint Shape

Any KHAOS system can expose a health endpoint and Pluto will auto-read it. The cron expects this JSON shape:

```json
{
  "usage_24h": {
    "page_views_24h": 42,
    "visitors_24h": 17,
    "api_calls_24h": 0
  },
  "last_usage_at": "2026-04-14T18:30:00Z"
}
```

The entire `usage_24h` object is spread into `usage_pulse` with `source: 'health_endpoint'` added. Any keys beyond the above are preserved. Libraries and scripts get `source: 'none'` and skip the health endpoint entirely.

Timeout: 5 seconds (`AbortSignal.timeout(5000)`). If the endpoint is slow or down, it's caught silently and the system gets `source: 'none'` for that heartbeat.

---

## Adding a New System

1. Create a migration: `supabase/migrations/105_<system_name>.sql`
2. Use `ON CONFLICT (id) DO NOTHING` for additive-only, or `DO UPDATE SET ...` if replacing
3. Set `health_endpoint` to the full URL if the system exposes usage metrics
4. The cron auto-picks it up — no code changes needed
5. Systems sharing a repo with another system: fine, harmless, don't dedup

```sql
-- supabase/migrations/105_new_system.sql
INSERT INTO pluto_systems (id, name, type, url, repo, vercel_project, health_endpoint, description, stack, status, notes)
VALUES (
  'my-system',
  'My System',
  'app',
  'https://my-system.vercel.app',
  'cotoaga/my-system',
  'my-system',
  'https://my-system.vercel.app/api/health',
  'One-liner description',
  '["next.js","supabase"]',
  'active',
  NULL
)
ON CONFLICT (id) DO NOTHING;
```

---

## Design System: COTOAGA.AI 2026

```css
--bg-primary:  #191A2E;   /* page background */
--bg-card:     #16213E;   /* card background */
--accent:      #EB9929;   /* headers, highlights, stable quadrant */
--success:     #4ADE80;   /* thriving quadrant */
--info:        #2DD4BF;   /* secondary info */
--building:    #0088FF;   /* building quadrant */
--dormant:     #4A4A4A;   /* dormant quadrant */
--danger:      #EF4444;   /* errors */
```

**Zero border-radius everywhere:**
```css
*, *::before, *::after { border-radius: 0 !important; }
```

Do not add `rounded-*` Tailwind classes. Do not use raw hex. Always use CSS vars.

Fonts: Space Grotesk (`font-heading`), Inter (body), JetBrains Mono (mono).

---

## What NOT to Do

These were all learned from actual mistakes:

| Don't | Why |
|-------|-----|
| Add `export const dynamic = 'force-dynamic'` to page.tsx | `cookies()` in `createClient()` already forces dynamic rendering. Redundant. |
| Convert cron to sequential `for...of` | 29 systems × 3+ API calls = guaranteed timeout |
| Use `Promise.all` instead of `Promise.allSettled` in cron | One 404 from GitHub will abort all other systems |
| Add `rounded-*` Tailwind classes | Zero border-radius is enforced globally with `!important` |
| Use raw hex colors | Use `var(--accent)` etc. — design system is maintained via CSS vars |
| Remove RLS from Supabase tables | `NEXT_PUBLIC_SUPABASE_ANON_KEY` is in the browser bundle. Anyone can call the Supabase REST API directly with it, bypassing Next.js and SITE_PASSWORD. Enable RLS with anon=SELECT-only. Service role bypasses RLS automatically — cron writes unaffected. |
| Put the APEX card inside the quadrant grid | APEX is always the hero card above the grid |
| Show `new Date()` in the footer as "Last updated" | That's page render time, not data freshness. Show `MAX(checked_at)` from pluto_heartbeats. |
| Call Vercel deploys API sequentially after commits | Fetch PRs and deploys in parallel within each system's processing |

---

## Middleware: Basic Auth

The dashboard sits behind optional Basic Auth. Set `SITE_PASSWORD` in Vercel env to enable it. If unset, the page is open (fine for local dev).

**Cron routes are explicitly exempt** from Basic Auth — they authenticate via `CRON_SECRET` internally.

```typescript
// middleware.ts
if (request.nextUrl.pathname.startsWith('/api/cron')) {
  return NextResponse.next()  // cron handles its own auth
}
```

---

## Environment Variables

```bash
# Supabase (Pluto's instance)
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# GitHub (classic token, repo read scope — all monitored repos must be accessible)
GITHUB_TOKEN=

# Vercel API (for deployment status)
VERCEL_TOKEN=
VERCEL_TEAM_ID=               # required if projects are under a team

# Cron auth
CRON_SECRET=                  # Vercel sends this; cron routes validate it

# Wallet auto-tracking
ANTHROPIC_ADMIN_API_KEY=      # Admin API key (different from regular API key)

# Dashboard access
SITE_PASSWORD=                # Optional Basic Auth. Unset = open.

# APEX cross-instance (optional — falls back gracefully if missing)
APEX_SUPABASE_URL=
APEX_SUPABASE_SERVICE_KEY=
```

---

## File Map

```
app/
  page.tsx                      # Server component. Fetches systems, heartbeats,
                                # wallets, last pulse time. Renders dashboard.
  api/cron/
    pulse/route.ts              # GET. Parallel heartbeat cron.
    wallets/route.ts            # GET. Anthropic spend auto-tracker.

components/
  WalletSection.tsx             # AI wallet cards (top of page)
  ApexCard.tsx                  # Hero card: strip + dev + usage + commentary
  SystemCard.tsx                # Quadrant grid card

lib/
  quadrant.ts                   # computeQuadrant(devPulse, usagePulse) → Quadrant
  signals.ts                    # detectSignals(heartbeat, prev?) → Signal[]
  supabase/
    server.ts                   # createClient() — uses cookies(), page = dynamic
    admin.ts                    # createAdminClient() — service role, cron only
    apex.ts                     # createApexClient() — APEX cross-instance reads

types/pluto.ts                  # PlutoSystem, PlutoHeartbeat, DevPulse,
                                # UsagePulse, Quadrant, PlutoWallet

supabase/migrations/
  100_pluto_schema.sql          # pluto_systems + pluto_heartbeats
  101_seed_data.sql             # 28 systems
  102_set_apex_health_endpoint.sql
  103_industrial_ai_complex.sql # 29th system
  104_wallets.sql               # pluto_wallets + 6 wallet rows

vercel.json                     # Cron schedules + maxDuration: 300
middleware.ts                   # Basic Auth (cron routes exempt)
```

---

## Registered Systems (29)

| id | name | type | has health endpoint |
|----|------|------|-------------------|
| apex-recruiter | APEX Recruiter | app | yes (hero card) |
| khaos-researcher | KHAOS Researcher | agent | no |
| be-part-of-net | be-part-of.net | app | no |
| dagger | DAGGER | app | no |
| dagger-mvp | DAGGER MVP | app | no |
| khaos-hondius | Hondius | mcp | no |
| cotoaga-net | Cotoaga.Net | site | no |
| cost-of-delay | CoDE | app | no |
| gup | GUP | app | no |
| mint4acme | MINT4ACME | site | no |
| moon-buggy | Moon Buggy | app | no |
| woke-gen-z | Woke Gen Z Racterizar | app | no |
| persona-vectors | Persona Vectors | library | no |
| agents-md | agents.md | library | no |
| sbaas-code | SBaaS CoDE | app | no |
| ai-sales-team | AI Sales Team | agent | no |
| claude-tokenizer | Claude Tokenizer | script | no |
| cognitive-devolution | Cognitive Devolution | library | no |
| eu-ai-act | EU AI Act | library | no |
| km-historical | KM Historical Perspectives | library | no |
| ki-manager-prompts | KI Manager Prompts | library | no |
| leaked-system-prompts | Leaked System Prompts | library | no |
| linear-mcp-server | Linear MCP Server | mcp | no |
| wifi-densepose | WiFi DensePose | library | no |
| khaos-seeds | KHAOS Seeds | library | no |
| khaos-pluto | Pluto | app | no |
| cotoaga-ai-gems | COTOAGA.AI Gems | site | no |
| khaos-questor | KHAOS-Questor | app | yes (gems.cotoaga.ai) |
| industrial-ai-complex | Industrial AI Complex | site | yes (gems.cotoaga.ai) |

`khaos-questor`, `industrial-ai-complex`, and `cotoaga-ai-gems` all share the repo `cotoaga/cotoaga-ai-gems`. Intentional.

---

## Phase Log

**Phase 1 (v1.0) — Built**
- Registry + heartbeat schema
- GitHub + Vercel dev pulse
- Quadrant grid
- 24 systems seeded

**Phase 2 (v2.0) — Built**
- APEX hero card with cross-instance Supabase
- Health endpoints (per-system usage metrics)
- AI wallet tracking (6 providers, 1 auto)
- Parallelized cron (Promise.allSettled)
- Basic Auth middleware
- Footer shows real data freshness (last heartbeat time)
- 29 systems
- maxDuration: 300 in vercel.json

**Phase 2.1 (v2.1) — Built**
- RLS enabled on pluto_systems, pluto_heartbeats, pluto_wallets
- anon role: SELECT only
- service role: bypasses RLS (cron writes unaffected)
- Triggered by custom domain deployment changing the threat model
- Migration: `supabase/migrations/105_enable_rls.sql`

**Future (not yet built)**
- Historical trend charts (heartbeats accumulate — data is there)
- Alerts / notifications (the dog should bark)
- MCP interface: "How's my universe?" conversation layer
- Per-system drill-down pages
- seed-health-endpoint.md — standard health endpoint shape for all KHAOS systems

---

*The watchdog doesn't need to be smart. It needs to be there, with its nose against every door, tail wagging when things are alive, whimpering when they're not. Phase 1 built the dog. Phase 2 gave it eyes for spending money and a nose for usage metrics. Phase 3: teach it to bark.*
