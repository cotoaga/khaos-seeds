# 🌱 SEED: Pluto — The Friendly Watchdog

**Version:** 1.0
**Depends on:** `seed-supabase.md` (Shape A — Next.js SSR Pattern)
**Purpose:** A registry with a pulse. Every gem, app, script, and MCP server Kydroon has built gets a card. Some cards have vital signs. All cards have a home.

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

**Phase 1 simplification:** Quadrant placement can be manual or computed from simple thresholds. Don't build an ML scoring engine. A commit in the last 7 days = "development active." A usage signal in the last 7 days = "usage active." That's the whole algorithm.

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
  health_endpoint TEXT,               -- '/api/health' or full URL (nullable)
  description TEXT,                   -- One-liner: what it does
  stack JSONB DEFAULT '[]',           -- ['next.js', 'supabase', 'tailwind']
  status TEXT DEFAULT 'active' CHECK (status IN (
    'active', 'archived', 'experimental', 'deprecated'
  )),
  notes TEXT,                         -- Freeform, because Kydroon will want it
  registered_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Updated_at trigger (from seed-supabase.md pattern)
CREATE TRIGGER update_pluto_systems_updated_at
  BEFORE UPDATE ON pluto_systems
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- Index on type for filtering
CREATE INDEX idx_pluto_systems_type ON pluto_systems(type);
CREATE INDEX idx_pluto_systems_status ON pluto_systems(status);
```

**Why TEXT primary key instead of UUID:** System IDs are human-readable slugs (`apex-recruiter`, `khaos-researcher`). You'll type them, read them, reference them. UUIDs would be hostile here.

### Table: `pluto_heartbeats` — The Pulse

```sql
CREATE TABLE pluto_heartbeats (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  system_id TEXT NOT NULL REFERENCES pluto_systems(id) ON DELETE CASCADE,
  
  -- Development pulse (from GitHub + Vercel APIs)
  dev_pulse JSONB DEFAULT '{}',
  -- Expected shape: {
  --   commits_7d: number,
  --   last_commit_at: ISO string,
  --   last_commit_message: string,
  --   last_deploy_at: ISO string,
  --   deploys_7d: number,
  --   build_status: 'success' | 'error' | 'unknown',
  --   open_prs: number
  -- }
  
  -- Usage pulse (system-specific)
  usage_pulse JSONB DEFAULT '{}',
  -- Shape varies per system. Examples:
  -- Researcher: { models_total: 1900000, last_research_at: ISO, research_cycles_7d: 56 }
  -- Apex: { matches_generated_7d: 15, last_match_at: ISO, cvs_analyzed_7d: 8 }
  -- be-part-of-net: { nodes: 42, edges: 87, last_login_at: ISO }
  
  -- Health check (if endpoint exists)
  health_status TEXT CHECK (health_status IN ('healthy', 'degraded', 'down', 'unknown')),
  health_response_ms INTEGER,
  
  -- Computed quadrant (or manual override)
  quadrant TEXT CHECK (quadrant IN ('thriving', 'stable', 'building', 'dormant')),
  quadrant_override BOOLEAN DEFAULT false,  -- true if manually set
  
  checked_at TIMESTAMPTZ DEFAULT NOW()
);

-- Index for querying latest heartbeat per system
CREATE INDEX idx_pluto_heartbeats_system ON pluto_heartbeats(system_id, checked_at DESC);

-- Keep only last 30 days of heartbeats (optional cleanup policy)
-- Can be enforced via Supabase cron or a cleanup function
```

### No RLS for Phase 1

Pluto is eyes-only. Single user. No auth in Phase 1 — it's a Vercel preview URL, not a public site. Skip the auth ceremony. Add it when/if Pluto gets a custom domain.

Service role key for writes (cron job). Anon key for reads (dashboard). Simple.

---

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                    PLUTO DASHBOARD                   │
│              pluto.cotoaga.ai (Vercel)              │
│                                                     │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌──────────┐ │
│  │  APEX   │ │RESEARCHER│ │  BPON   │ │ HONDIUS  │ │
│  │  🔥     │ │  ⚓      │ │  ⚓     │ │  💀?     │ │
│  └─────────┘ └─────────┘ └─────────┘ └──────────┘ │
│                                                     │
│         Reads from: pluto_heartbeats (latest)       │
└──────────────────────┬──────────────────────────────┘
                       │ reads
                       ▼
              ┌──────────────────┐
              │    SUPABASE      │
              │  pluto_systems   │
              │  pluto_heartbeats│
              └────────┬─────────┘
                       │ writes
                       ▼
              ┌──────────────────┐
              │  VERCEL CRON     │
              │  /api/cron/pulse │
              │  Every 15 min    │
              └──────────────────┘
                       │ fetches from
                       ▼
              ┌──────────────────┐
              │  GitHub API      │ → commits, PRs
              │  Vercel API      │ → deploys, build status
              │  Health endpoints│ → system-specific usage
              └──────────────────┘
```

**No n8n.** The cron job is a Vercel Cron route (`/api/cron/pulse`). One less dependency. If the cron logic outgrows a single route, THEN consider n8n. Not before.

---

## Cron Job: `/api/cron/pulse`

Runs every 15 minutes via `vercel.json` cron configuration.

### What it does:

1. **Read all active systems** from `pluto_systems`
2. **For each system with a `repo`:** Hit GitHub API for dev metrics
3. **For each system with a `vercel_project`:** Hit Vercel API for deploy metrics
4. **For each system with a `health_endpoint`:** Hit it, record status + response time
5. **Compute quadrant** based on simple thresholds:
   - `commits_7d > 0` → development active
   - `usage_pulse` has any recent signal → usage active
   - Both active → `thriving`
   - Only dev → `building`
   - Only usage → `stable`
   - Neither → `dormant`
6. **Upsert heartbeat** into `pluto_heartbeats`

### Environment Variables (Pluto-specific)

```env
# Supabase (from seed-supabase.md)
NEXT_PUBLIC_SUPABASE_URL=...
NEXT_PUBLIC_SUPABASE_ANON_KEY=...
SUPABASE_SERVICE_ROLE_KEY=...

# GitHub API (for dev pulse)
GITHUB_TOKEN=ghp_...          # Personal access token, read-only scope

# Vercel API (for deploy pulse)
VERCEL_TOKEN=...               # Vercel API token
VERCEL_TEAM_ID=...             # Optional, for team deployments

# Cron secret (Vercel cron auth)
CRON_SECRET=...                # Protects the cron endpoint from external calls
```

### Vercel Cron Configuration

```json
// vercel.json
{
  "crons": [
    {
      "path": "/api/cron/pulse",
      "schedule": "*/15 * * * *"
    }
  ]
}
```

---

## Dashboard: Single Page, Cards + Quadrant

### Views

**Primary view:** Grid of system cards, visually grouped by quadrant.

Each card shows:
- System name + type badge
- Quadrant indicator (emoji + color)
- Last commit: relative time
- Last deploy: relative time
- Health: green/yellow/red dot (if endpoint exists)
- One-liner description
- Links: live URL, repo

**That's it for Phase 1.** No drill-down pages. No historical charts. No settings panel. One page, one glance, whole universe.

### The "Add System" Flow

Phase 1: Insert a row directly in Supabase (or KHAOS does it via Ops Mode).
Phase 2 (maybe): A simple form on the dashboard itself.

The friction of adding a system should approach zero. If Kydroon builds something new and has to write code to register it with Pluto, Pluto fails.

---

## Phase 1 Seed Data

Register these systems on first deploy:

```sql
INSERT INTO pluto_systems (id, name, type, url, repo, vercel_project, health_endpoint, description, stack) VALUES
  ('apex-recruiter', 'APEX Recruiter', 'app', 'https://apex-recruiter.com', 'cotoaga/apex-recruiter', 'apex-recruiter', NULL, 'AI-powered job matching for recruiters', '["next.js", "supabase", "claude", "perplexity"]'),
  ('khaos-researcher', 'KHAOS Researcher', 'agent', 'https://khaos-researcher.vercel.app', 'cotoaga/khaos-researcher', 'khaos-researcher', NULL, 'AI model intelligence tracker — futility made visible', '["node.js", "supabase", "vercel"]'),
  ('be-part-of-net', 'be-part-of.net', 'app', 'https://be-part-of.net', 'cotoaga/be-part-of-net', 'be-part-of-net', NULL, 'The Anti-Social Social Network', '["next.js", "supabase", "d3"]'),
  ('dagger', 'DAGGER', 'app', NULL, 'cotoaga/dagger', NULL, NULL, 'Directed Acyclic Graph Generated Enlightenment Repository', '["javascript"]'),
  ('dagger-mvp', 'DAGGER MVP', 'app', NULL, 'cotoaga/dagger-mvp', NULL, NULL, 'DAGGER proof of concept', '["javascript"]'),
  ('khaos-hondius', 'Hondius', 'mcp', NULL, 'cotoaga/KHAOS-Hondius', NULL, NULL, 'Classification engine — books, texts, concepts', '["typescript", "mcp"]'),
  ('cotoaga-net', 'Cotoaga.Net', 'site', 'https://cotoaga.net', 'cotoaga/Cotoaga.Net', NULL, NULL, 'Mad scientist''s laboratory — WordPress', '["wordpress", "elementor"]'),
  ('cost-of-delay', 'CoDE', 'app', 'https://cotoaga.net/code/', 'cotoaga/Cost-of-Delay-Estimator', NULL, NULL, 'Cost of Delay Estimator', '["html", "javascript"]'),
  ('gup', 'GUP', 'app', NULL, 'cotoaga/GUP', NULL, NULL, 'Great Unsolved Problem — TSP + 3D bin-packing via evolutionary algorithms (diploma thesis 1993)', '[]'),
  ('mint4acme', 'MINT4ACME', 'site', 'https://mint-4-acme.vercel.app', 'cotoaga/MINT4ACME', 'mint-4-acme', NULL, 'Digital archaeology — 1997 automotive sales web app (CSC Ploenzke). VRML, Flash, framesets.', '["html", "digital-archaeology"]'),
  ('moon-buggy', 'Moon Buggy', 'app', NULL, 'cotoaga/moon-buggy', NULL, NULL, 'Recreation of Moon Patrol', '["javascript"]'),
  ('woke-gen-z', 'Woke Gen Z Racterizar', 'app', NULL, 'cotoaga/woke-gen-z-racterizar', NULL, NULL, 'Dual-bot Frankenstein of woke ELIZA and cryptic Racter', '["typescript"]'),
  ('persona-vectors', 'Persona Vectors', 'library', NULL, 'cotoaga/persona_vectors', NULL, NULL, 'Persona vector explorations', '[]'),
  ('agents-md', 'agents.md', 'library', NULL, 'cotoaga/agents.md', NULL, NULL, 'Agent documentation', '[]'),
  ('sbaas-code', 'SBaaS CoDE', 'app', NULL, 'cotoaga/sbaas-code', NULL, NULL, 'SBaaS Code implementation', '[]'),
  ('ai-sales-team', 'AI Sales Team', 'agent', NULL, 'cotoaga/cotoaga-net-ai-sales-team', NULL, NULL, 'AI-powered sales automation', '[]'),
  ('claude-tokenizer', 'Claude Tokenizer', 'script', NULL, 'cotoaga/claude-tokenizer', NULL, NULL, 'Token counting utility', '[]'),
  ('cognitive-devolution', 'Cognitive Devolution', 'library', NULL, 'cotoaga/cognitive-devolution', NULL, NULL, 'Research on cognitive patterns', '[]'),
  ('eu-ai-act', 'EU AI Act', 'library', NULL, 'cotoaga/EU-AI-Act', NULL, NULL, 'EU AI Act reference material', '[]'),
  ('km-historical', 'KM Historical Perspectives', 'library', NULL, 'cotoaga/km-historical-perspectives', NULL, NULL, 'Knowledge Management historical research', '[]'),
  ('ki-manager-prompts', 'KI Manager Prompts', 'library', NULL, 'cotoaga/KI-Manager-Prompts', NULL, NULL, 'AI manager prompt collection', '[]'),
  ('leaked-system-prompts', 'Leaked System Prompts', 'library', NULL, 'cotoaga/Leaked-System-Prompts', NULL, NULL, 'System prompt research collection', '[]'),
  ('linear-mcp-server', 'Linear MCP Server', 'mcp', NULL, 'cotoaga/Linear-MCP-Server', NULL, NULL, 'Linear integration for MCP', '["mcp"]'),
  ('wifi-densepose', 'WiFi DensePose', 'library', NULL, 'cotoaga/wifi-densepose', NULL, NULL, 'WiFi-based pose estimation research', '[]')
ON CONFLICT (id) DO NOTHING;
```

**24 systems.** That's the actual inventory. Most won't have live URLs or health endpoints. They'll show up as grey cards with just their repo link and whatever GitHub tells us about their development pulse. That's still infinitely more visibility than "check my GitHub profile and try to remember what everything is."

---

## Stack

| Layer | Choice | Why |
|-------|--------|-----|
| **Framework** | Next.js 14+ (App Router) | Consistent with ecosystem |
| **Styling** | Tailwind CSS | Consistent with ecosystem |
| **Database** | Supabase (via seed-supabase.md) | Consistent with ecosystem |
| **Hosting** | Vercel | Consistent with ecosystem |
| **Data fetch** | GitHub REST API + Vercel REST API | Free tier sufficient for this scale |
| **Cron** | Vercel Cron | No external dependency |
| **Auth** | None (Phase 1) | Single user, preview URL |

### Aesthetic

COTOAGA.AI style system. Dark theme. Klein Bottle Green for healthy, Deep Space Blue for background, amber for warnings, red for dead. Cards with subtle glow for thriving systems.

---

## What Phase 1 Does NOT Include

- **Historical trends** — Heartbeats accumulate, but no charts yet
- **Alerts/notifications** — No barking yet, just the visual
- **Usage pulse automation** — Phase 1 has dev pulse only (GitHub/Vercel). Usage signals are manually seeded or empty.
- **MCP interface** — No "How's my universe?" conversation layer
- **Auth** — No login, Vercel preview URL only
- **Per-system drill-down pages** — One page, cards only
- **Health endpoints on existing systems** — Pluto works with what's queryable today

---

## Future Seeds (Not Phase 1)

- **seed-health-endpoint.md** — Standard `/api/health` response shape that any KHAOS system can implement. Returns usage metrics in a predictable JSON shape. Pluto's cron knows how to read it.
- **seed-pluto-mcp.md** — MCP server that reads Pluto's Supabase tables and answers "How's my universe?"
- **seed-cotoaga-aesthetic.md** — The visual design system: colors, typography, card patterns, glow effects.

---

*The watchdog doesn't need to be smart. It needs to be there, with its nose against every door, tail wagging when things are alive, whimpering when they're not. Build the dumb loyal dog first. Teach it tricks later.*
