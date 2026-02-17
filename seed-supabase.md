# 🌱 SEED: Supabase Integration Pattern

**Version:** 1.0
**Source Truth:** Extracted from `cotoaga/be-part-of-net` (Next.js/TS) and `cotoaga/khaos-researcher` (Node.js/JS)
**Purpose:** When Claude Code sets up Supabase in any KHAOS ecosystem project, it follows THIS pattern. No guessing, no generic tutorials — this is how Kydroon's systems actually work.

---

## Two Patterns, One Philosophy

The ecosystem has two Supabase integration shapes depending on what's being built:

| Shape | When to Use | Reference Implementation |
|-------|------------|--------------------------|
| **Next.js SSR Pattern** | User-facing web apps with auth | `be-part-of-net` |
| **Service Pattern** | Backend services, crons, agents | `khaos-researcher` |

**Choose the shape based on the project, not by preference.** Pluto (dashboard) → SSR Pattern. A CLI tool → Service Pattern. An n8n-triggered agent → Service Pattern.

---

## Shape A: Next.js SSR Pattern

For any Next.js app that has users, cookies, or server/client rendering split.

### Dependencies

```bash
npm install @supabase/ssr @supabase/supabase-js
```

### Environment Variables

```env
# .env.local.example
# NEXT_PUBLIC_ prefix = exposed to browser (intentional)
NEXT_PUBLIC_SUPABASE_URL=your-supabase-url-here
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-supabase-anon-key-here

# NO prefix = server-only (never reaches browser)
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key-here
```

**Convention:** The `NEXT_PUBLIC_` prefix is not optional decoration. It controls what the browser can see. The anon key is safe to expose (RLS protects the data). The service role key NEVER gets the prefix.

### File Structure: `lib/supabase/`

Three files. Always three. Each has one job.

#### `lib/supabase/client.ts` — Browser Client

```typescript
// Supabase client for client-side usage
import { createBrowserClient } from '@supabase/ssr';

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  );
}
```

**Used by:** React components, client-side hooks, anything running in the browser.
**Auth context:** Respects the logged-in user's session. RLS applies.

#### `lib/supabase/server.ts` — Server Client (with cookies)

```typescript
// Supabase server client for server-side usage
import { createServerClient } from '@supabase/ssr';
import { cookies } from 'next/headers';

export async function createClient() {
  const cookieStore = await cookies();

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll();
        },
        setAll(cookiesToSet: Array<{ name: string; value: string; options?: any }>) {
          try {
            cookiesToSet.forEach(({ name, value, options }) =>
              cookieStore.set(name, value, options)
            );
          } catch {
            // The `setAll` method was called from a Server Component.
            // This can be ignored if you have middleware refreshing user sessions.
          }
        },
      },
    }
  );
}
```

**Used by:** Server Components, Route Handlers, Server Actions.
**Auth context:** Reads the user's auth from cookies. RLS applies as that user.

#### `lib/supabase/admin.ts` — Service Role Client (bypasses RLS)

```typescript
// Supabase admin client with service role key (bypasses RLS)
// WARNING: Only use this for trusted server-side operations!
import { createClient } from '@supabase/supabase-js';

export function createAdminClient() {
  const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL;
  const supabaseServiceKey = process.env.SUPABASE_SERVICE_ROLE_KEY;

  if (!supabaseUrl || !supabaseServiceKey) {
    throw new Error('Missing Supabase URL or Service Role Key');
  }

  // Service role key bypasses RLS
  return createClient(supabaseUrl, supabaseServiceKey, {
    auth: {
      autoRefreshToken: false,
      persistSession: false,
    },
  });
}
```

**Used by:** Cron jobs, admin operations, data seeding, system-level queries.
**Auth context:** God mode. Bypasses all RLS. Use with intent.

### Middleware Pattern (Auth-Protected Routes)

```typescript
// middleware.ts (root level)
import { createServerClient } from '@supabase/ssr'
import { NextResponse, type NextRequest } from 'next/server'

export async function middleware(request: NextRequest) {
  const { pathname } = request.nextUrl

  // ===== PUBLIC ROUTES (No auth check) =====
  const alwaysPublicRoutes = [
    '/',
    '/api/auth/callback',
  ]

  const isAlwaysPublic = alwaysPublicRoutes.some(
    route => pathname === route || pathname.startsWith(route + '/')
  )

  if (
    isAlwaysPublic ||
    pathname.startsWith('/_next') ||
    pathname.includes('.')
  ) {
    return NextResponse.next()
  }

  // ===== CREATE SUPABASE CLIENT =====
  let supabaseResponse = NextResponse.next({ request })

  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return request.cookies.getAll()
        },
        setAll(cookiesToSet: Array<{ name: string; value: string; options?: any }>) {
          cookiesToSet.forEach(({ name, value }) => request.cookies.set(name, value))
          supabaseResponse = NextResponse.next({ request })
          cookiesToSet.forEach(({ name, value, options }) =>
            supabaseResponse.cookies.set(name, value, options)
          )
        },
      },
    }
  )

  // ===== CHECK AUTHENTICATION =====
  const { data: { user } } = await supabase.auth.getUser()

  if (!user) {
    const url = request.nextUrl.clone()
    url.pathname = '/'
    return NextResponse.redirect(url)
  }

  return supabaseResponse
}

export const config = {
  matcher: ['/((?!_next/static|_next/image|favicon.ico|.*\\.(?:svg|png|jpg|jpeg|gif|webp)$).*)'],
}
```

**Adapt the public routes array per project.** Everything else is boilerplate.

---

## Shape B: Service Pattern

For anything that isn't a Next.js web app — agents, scripts, cron jobs, automation.

### Dependencies

```bash
npm install @supabase/supabase-js
```

Note: NO `@supabase/ssr` — that's only for Next.js cookie handling.

### Environment Variables

```env
# .env / .env.example
# No NEXT_PUBLIC_ prefix — there's no browser
SUPABASE_URL=https://[PROJECT_ID].supabase.co
SUPABASE_ANON_KEY=eyJ...
SUPABASE_SERVICE_KEY=eyJ...
```

### Singleton Client

```javascript
// src/utils/SupabaseClient.js (or .ts)
import { createClient } from '@supabase/supabase-js'

class SupabaseClient {
  constructor() {
    const supabaseUrl = process.env.SUPABASE_URL
    const supabaseKey = process.env.SUPABASE_SERVICE_KEY

    if (!supabaseUrl || !supabaseKey) {
      console.warn('⚠️ No Supabase credentials — running without persistence')
      this.supabase = null
      return
    }

    this.supabase = createClient(supabaseUrl, supabaseKey, {
      auth: {
        autoRefreshToken: false,
        persistSession: false
      }
    })
  }

  async testConnection() {
    if (!this.supabase) return false
    try {
      // Replace 'your_table' with an actual table name
      const { error } = await this.supabase
        .from('health_check_table')
        .select('id')
        .limit(1)
      if (error) throw error
      return true
    } catch (error) {
      console.error('❌ Supabase connection failed:', error)
      return false
    }
  }

  isAvailable() {
    return this.supabase !== null
  }

  getClient() {
    return this.supabase
  }
}

// Singleton for serverless reuse
let instance = null

export function getSupabaseClient() {
  if (!instance) {
    instance = new SupabaseClient()
  }
  return instance
}
```

**Key difference from Shape A:** The singleton pattern matters in serverless (Vercel Functions). Without it, every invocation creates a new client. The singleton survives across warm invocations.

**Graceful fallback:** The researcher pattern allows running without Supabase (returns `null` client). This is good for local development and testing. Adopt this when it makes sense.

---

## Database Conventions

These apply to BOTH shapes. This is how tables look in the KHAOS ecosystem.

### Table Creation Pattern

```sql
CREATE TABLE your_table (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  -- ... your columns ...
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Auto-update timestamp trigger (create once, reuse)
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_your_table_updated_at
  BEFORE UPDATE ON your_table
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
```

### Conventions

| Convention | Pattern | Why |
|-----------|---------|-----|
| **Primary keys** | `UUID DEFAULT gen_random_uuid()` | No sequential guessing, distributed-safe |
| **Timestamps** | `TIMESTAMPTZ DEFAULT NOW()` | Always timezone-aware, always defaulted |
| **Updated tracking** | Trigger-based `updated_at` | Automatic, can't forget to set it |
| **Enums** | `CHECK (type IN ('a', 'b', 'c'))` | Not Postgres ENUM (those are painful to alter) |
| **JSON columns** | `JSONB` (not JSON) | Indexable, queryable, the right default |
| **Foreign keys** | `REFERENCES table(id) ON DELETE CASCADE` or `SET NULL` | Explicit deletion behavior, no orphans |
| **Indexes** | On every FK and common filter column | Create at table creation, not as afterthought |

### Row Level Security (RLS) Pattern

```sql
-- Always enable RLS on user-facing tables
ALTER TABLE your_table ENABLE ROW LEVEL SECURITY;

-- Read: depends on your model
CREATE POLICY "Anyone can view" ON your_table FOR SELECT USING (true);
-- OR
CREATE POLICY "Users see own data" ON your_table FOR SELECT USING (auth.uid() = user_id);

-- Write: authenticated only
CREATE POLICY "Auth users can insert" ON your_table FOR INSERT
  WITH CHECK (auth.role() = 'authenticated');

-- Update/Delete: creator only
CREATE POLICY "Creators can update" ON your_table FOR UPDATE
  USING (created_by = auth.uid()::uuid);
```

**When to skip RLS:** Service-only tables that no user directly queries (e.g., research_cycles in Researcher). Use the service role key and skip the ceremony.

### Migration File Convention

```
supabase/migrations/
  100_initial_schema.sql
  101_add_feature_x.sql
  102_fix_constraint.sql
```

Numbered sequentially. Descriptive names. Each migration is idempotent where possible (`CREATE TABLE IF NOT EXISTS`, `DROP TABLE IF EXISTS CASCADE` for full resets).

---

## Query Patterns

### Basic CRUD

```typescript
// Read with filter
const { data, error } = await supabase
  .from('table')
  .select('*')
  .eq('column', value)
  .order('created_at', { ascending: false })
  .limit(50)

// Insert
const { data, error } = await supabase
  .from('table')
  .insert([{ column: value }])
  .select()
  .single()

// Update
const { error } = await supabase
  .from('table')
  .update({ column: newValue })
  .eq('id', id)

// Upsert (insert or update)
const { data, error } = await supabase
  .from('table')
  .upsert({ id: existingOrNewId, column: value })
  .select()
  .single()

// Count without fetching data
const { count, error } = await supabase
  .from('table')
  .select('*', { count: 'exact', head: true })
```

### Error Handling

```typescript
const { data, error } = await supabase.from('table').select('*')

if (error) {
  // Supabase errors have: message, details, hint, code
  console.error(`Supabase error [${error.code}]: ${error.message}`)
  throw error // or handle gracefully
}
```

---

## Checklist: Setting Up Supabase in a New Project

For Claude Code — follow this when a directive requires Supabase:

- [ ] Determine the shape: Next.js SSR (Shape A) or Service (Shape B)
- [ ] Install correct dependencies
- [ ] Create `.env.local.example` (Shape A) or `.env.example` (Shape B) with placeholder values
- [ ] Create client file(s) in the correct location:
  - Shape A: `lib/supabase/client.ts`, `server.ts`, `admin.ts`
  - Shape B: `src/utils/SupabaseClient.js` (or equivalent)
- [ ] Add middleware if auth is needed (Shape A only)
- [ ] Create migration files in `supabase/migrations/`
- [ ] Apply conventions: UUID PKs, TIMESTAMPTZ, updated_at triggers, indexes on FKs
- [ ] Enable RLS on user-facing tables, skip on service-only tables
- [ ] Add `.env.local` to `.gitignore` (should already be there)
- [ ] Verify: can you connect and query from the app?

---

## What This Seed Does NOT Cover

- Supabase Auth setup (email/password, magic link, OAuth) — project-specific
- Supabase Storage (file uploads) — project-specific
- Supabase Realtime (subscriptions) — project-specific
- Supabase Edge Functions — not yet used in the ecosystem

These are project-level decisions, not ecosystem-level patterns. Add them to the project's own CLAUDE.md when needed.

---

*The seed is planted. The pattern is the pattern. Don't reinvent it — instantiate it.*
