# 🌱 SEED: Health Endpoint — Standard Usage Signal Shape

**Version:** 1.0
**Reference implementation:** APEX Recruiter `/api/health`
**Consumed by:** KHAOS-Pluto cron (`/api/cron/pulse`) every 10 minutes
**Purpose:** Standard shape for any KHAOS system that wants to expose usage signals to Pluto.

---

## The Problem

Pluto needs to know if a system is being used, not just if it's alive. A HEAD request tells you the process is up. A health endpoint tells you whether anyone is actually using it — today, this week, this month.

Without a standard shape, every system invents its own format and the Pluto cron accumulates one-off parsers. This seed pins the contract.

---

## Response Shape

```json
{
  "status": "healthy",
  "checked_at": "2026-04-19T18:00:00.000Z",
  "usage_24h": {
    "events": 4,
    "unique_actors": 1,
    "event_types": {
      "cv.uploaded": 1,
      "cv.analyzed": 1,
      "matches.generated": 1,
      "pitch.copied": 1
    }
  },
  "usage_7d": {
    "events": 12,
    "unique_actors": 2,
    "event_types": { ... }
  },
  "usage_28d": {
    "events": 47,
    "unique_actors": 3,
    "event_types": { ... }
  },
  "last_usage_at": "2026-04-19T17:45:00.000Z"
}
```

### Field contract

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `status` | `"healthy" \| "degraded"` | yes | Degraded = DB unavailable, null windows |
| `checked_at` | ISO timestamp | yes | When the response was generated |
| `usage_24h` | `AggregateWindow \| null` | yes | Last 24 rolling hours |
| `usage_7d` | `AggregateWindow \| null` | yes | Last 7 rolling days |
| `usage_28d` | `AggregateWindow \| null` | yes | Last 28 rolling days |
| `last_usage_at` | ISO timestamp \| null | yes | Timestamp of the most recent event |

```typescript
interface AggregateWindow {
  events: number           // raw row count (including non-whitelisted)
  unique_actors: number    // distinct actor UUIDs
  event_types: Record<string, number>  // whitelisted events only
}
```

### Degraded shape (DB unavailable)

```json
{
  "status": "degraded",
  "checked_at": "2026-04-19T18:00:00.000Z",
  "usage_24h": null,
  "usage_7d": null,
  "usage_28d": null,
  "last_usage_at": null
}
```

---

## How Pluto reads it

The cron normalises any health endpoint response to a `UsagePulse` object:

```typescript
const u24  = healthData.usage_24h ?? healthData.usage ?? {}
const u7d  = healthData.usage_7d  ?? {}
const u28d = healthData.usage_28d ?? {}

const mapped: UsagePulse = {
  // "today" counter — falls back to u24.events for KHAOS-shape endpoints
  api_calls_24h:    u24.api_calls_24h ?? u24.api_calls ?? u24.events ?? null,
  events_7d:        u7d.events        ?? u24.events    ?? null,
  events_28d:       u28d.events                        ?? null,
  active_actors_7d: u7d.unique_actors ?? u24.unique_actors ?? null,
  events_by_type:   u7d.event_types   ?? u24.event_types   ?? null,
  source: 'health_endpoint',
  last_usage_at: healthData.last_usage_at ?? null,
}
```

Pluto's `UsagePulse` display reads:
- `api_calls_24h` → "today" counter
- `events_7d` → big number (main metric)
- `events_28d` → 28d counter
- `active_actors_7d` → "active users" label
- `events_by_type` → funnel bars

---

## Implementation Pattern (Next.js App Router)

### 1. Telemetry table

Requires an append-only events table. APEX uses `khaos_events`:

```sql
CREATE TABLE khaos_events (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  event_type TEXT NOT NULL,
  actor_id   UUID,            -- FK to auth.users (nullable for anonymous)
  metadata   JSONB DEFAULT '{}',
  created_at TIMESTAMPTZ DEFAULT NOW()
);
CREATE INDEX idx_khaos_events_created ON khaos_events(created_at DESC);
-- No RLS: only service role writes, no user-facing reads
```

### 2. Whitelist

Define a whitelist of event types safe for public aggregate exposure. Rules:
- Include workflow events (what users are doing)
- Exclude error events (`email.error`, `db.error`) — operational, not usage signals
- Exclude PII-adjacent events (login, password-change)

APEX's whitelist:
```typescript
const WHITELISTED_EVENT_TYPES = new Set([
  'cv.uploaded', 'cv.analyzed', 'market.searched',
  'matches.generated', 'shortlist.added', 'ki_match.requested',
  'pitch.viewed', 'pitch.copied',
  'market.overview_viewed', 'profile.edited',
])
```

### 3. Cache

The health endpoint is polled every 10 minutes. Without a cache, every Pluto ping hits the DB. Use a module-level in-process cache:

```typescript
const CACHE_TTL_MS = 5 * 60 * 1000 // 5 minutes

let _cache: { result: HealthResult; cachedAt: number } | null = null

function getCached() {
  if (_cache && Date.now() - _cache.cachedAt < CACHE_TTL_MS) return _cache.result
  return null
}
```

5-minute TTL balances freshness against DB load. Don't cache degraded responses.

### 4. Route

```typescript
export const dynamic = 'force-dynamic'  // hits DB at runtime

export async function GET() {
  const cached = getCached()
  if (cached) return NextResponse.json(cached)

  const supabase = createAdminClient()  // service role for guaranteed reads
  const now = Date.now()

  const { data, error } = await supabase
    .from('khaos_events')
    .select('event_type, actor_id, created_at')
    .gte('created_at', new Date(now - 28 * 24 * 60 * 60 * 1000).toISOString())
    .order('created_at', { ascending: false })

  if (error) {
    return NextResponse.json({ status: 'degraded', checked_at: new Date().toISOString(),
      usage_24h: null, usage_7d: null, usage_28d: null, last_usage_at: null })
  }

  const rows = data ?? []
  const oneDayAgo    = new Date(now -  1 * 24 * 60 * 60 * 1000)
  const sevenDaysAgo = new Date(now -  7 * 24 * 60 * 60 * 1000)

  const rows24h = rows.filter(r => new Date(r.created_at) >= oneDayAgo)
  const rows7d  = rows.filter(r => new Date(r.created_at) >= sevenDaysAgo)

  const result = {
    status: 'healthy',
    checked_at: new Date().toISOString(),
    usage_24h: buildWindow(rows24h),
    usage_7d:  buildWindow(rows7d),
    usage_28d: buildWindow(rows),
    last_usage_at: rows[0]?.created_at ?? null,
  }

  setCache(result)
  return NextResponse.json(result)
}
```

### 5. Route must be public

Pluto's cron hits this endpoint without auth. Add it to your middleware public routes:

```typescript
// middleware.ts
const PUBLIC_PATHS = ['/api/health', '/login', '/auth/...']
```

---

## Security considerations

- No user data leaves this endpoint — only aggregate counts
- `unique_actors` is a count of distinct UUIDs, not the UUIDs themselves
- `event_types` keys are event names only, not values or metadata
- `actor_id` is never exposed in the response (PII audit: test that UUIDs don't appear)

---

## What NOT to do

| Don't | Why |
|-------|-----|
| Expose `metadata` or `actor_id` raw | PII risk. Aggregates only. |
| Skip the whitelist | An event like `payment.failed` would expose financial signal |
| Cache degraded responses | A transient DB blip would persist for 5 minutes |
| Use rolling 24h as "today" in Pluto UI | Calendar-day semantics ≠ rolling 24h. Use `api_calls_24h` for "today" in Pluto. |
| Query without an index on `created_at` | Full table scan on every Pluto ping |

---

## APEX as reference

- Route: `app/api/health/route.ts`
- Cache: `lib/health-cache.ts` (5-min TTL, `AggregateCache` type)
- Telemetry emitter: `lib/telemetry.ts` (`emit(eventType, metadata?)`)
- Tests: `app/api/health/route.test.ts` (Vitest, 11 tests)

---

*First documented: 2026-04-19. Reference implementation shipped in APX-179 (24h/7d) + sync fix (28d).*
