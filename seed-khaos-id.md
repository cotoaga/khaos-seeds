# 🌱 SEED: khaos-id Federated Identity Pattern

**Version:** 1.0
**Source Truth:** Extracted from `cotoaga/khaos-mouseion` Architecture B (ADR-0001)
**Purpose:** When Claude Code wires authentication in any KHAOS ecosystem app, it follows THIS pattern. No local user tables. No re-inventing auth. khaos-id owns identity.

---

## The One Rule That Matters Most

**Mouseion (and every KHAOS sibling) does NOT own identity.** khaos-id is the IdP. KHAOS apps are relying parties. The boundary is the JWT in the httpOnly cookie — apps verify it, they don't mint it.

---

## Architecture

```
User → KHAOS App (gated route)
         ↓  (no session)
     khaos-id.vercel.app/login?redirect_to=<callback>
         ↓  (authenticates via Supabase)
     KHAOS App /api/auth/callback?code=<pkce_code>&next=<original_path>
         ↓  (exchangeCodeForSession → sets httpOnly cookie)
     KHAOS App (gated route, now authenticated)
         ↓  (getVerifiedClaims → verifies JWT against SUPABASE_JWKS_URL)
     ✓  Claims available to Server Components
```

Mouseion and khaos-id share the **same Supabase project** (`NEXT_PUBLIC_SUPABASE_URL`). The JWT is issued by Supabase and verified locally against the JWKS — no round-trip to khaos-id at runtime.

---

## Required Environment Variables

```env
# Supabase project (shared across the ecosystem)
NEXT_PUBLIC_SUPABASE_URL=https://<project>.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=<anon-key>

# Federated JWT verification
SUPABASE_JWKS_URL=https://<project>.supabase.co/auth/v1/.well-known/jwks.json

# khaos-id integration
KHAOS_ID_LOGIN_URL=https://khaos-id.vercel.app/login
NEXT_PUBLIC_SITE_URL=https://<this-app>.cotoaga.ai
```

`KHAOS_ID_LOGIN_URL` and `SUPABASE_JWKS_URL` must NOT have `NEXT_PUBLIC_` prefix — they are server-only. `NEXT_PUBLIC_SITE_URL` is safe to expose (it's just the app's own origin).

---

## Required Files

### `lib/khaos-id.ts` — JWT Verification

```typescript
import { createRemoteJWKSet, jwtVerify, type JWTPayload } from 'jose';
import { createServerClient } from '@supabase/ssr';
import { cookies } from 'next/headers';

let jwks: ReturnType<typeof createRemoteJWKSet> | null = null;
function getJWKS() {
  if (!jwks) jwks = createRemoteJWKSet(new URL(process.env.SUPABASE_JWKS_URL!));
  return jwks;
}

export async function getVerifiedClaims(): Promise<JWTPayload | null> {
  const cookieStore = await cookies();
  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    { cookies: { getAll: () => cookieStore.getAll(), setAll: () => {} } },
  );
  const { data: { session } } = await supabase.auth.getSession();
  if (!session) return null;
  const { payload } = await jwtVerify(session.access_token, getJWKS(), {
    audience: 'authenticated',
  });
  return payload;
}
```

**Used by:** Server Components that need identity. Returns `null` for unauthenticated visitors — never throws. Callers handle the redirect.

### `app/api/auth/callback/route.ts` — PKCE Code Exchange

```typescript
import { createServerClient } from '@supabase/ssr';
import { cookies } from 'next/headers';
import { NextResponse, type NextRequest } from 'next/server';

export async function GET(request: NextRequest) {
  const { searchParams, origin } = new URL(request.url);
  const code = searchParams.get('code');
  const next = searchParams.get('next') ?? '/';
  const errorParam = searchParams.get('error');

  if (errorParam) {
    return NextResponse.redirect(`${origin}/?error=${encodeURIComponent(errorParam)}`);
  }
  if (!code) {
    return NextResponse.redirect(`${origin}/?error=missing_code`);
  }

  const cookieStore = await cookies();
  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() { return cookieStore.getAll(); },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value, options }) =>
            cookieStore.set(name, value, options)
          );
        },
      },
    }
  );

  const { error } = await supabase.auth.exchangeCodeForSession(code);
  if (error) {
    return NextResponse.redirect(`${origin}/?error=auth_failed`);
  }

  return NextResponse.redirect(`${origin}${next}`);
}
```

**This route MUST be in `alwaysPublicRoutes` in middleware** — it receives unauthenticated requests by definition.

### `middleware.ts` — Route Protection

```typescript
import { createServerClient } from '@supabase/ssr';
import { NextResponse, type NextRequest } from 'next/server';

export async function middleware(request: NextRequest) {
  const { pathname } = request.nextUrl;

  const alwaysPublicRoutes = ['/', '/api/auth/callback'];

  const isAlwaysPublic = alwaysPublicRoutes.some(
    route => pathname === route || pathname.startsWith(route + '/')
  );

  if (isAlwaysPublic || pathname.startsWith('/_next') || pathname.includes('.')) {
    return NextResponse.next();
  }

  let supabaseResponse = NextResponse.next({ request });

  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() { return request.cookies.getAll(); },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value }) => request.cookies.set(name, value));
          supabaseResponse = NextResponse.next({ request });
          cookiesToSet.forEach(({ name, value, options }) =>
            supabaseResponse.cookies.set(name, value, options)
          );
        },
      },
    }
  );

  const { data: { user } } = await supabase.auth.getUser();

  if (!user) {
    const callbackUrl = new URL('/api/auth/callback', request.nextUrl.origin);
    callbackUrl.searchParams.set('next', pathname);
    const loginUrl = new URL(
      process.env.KHAOS_ID_LOGIN_URL ?? 'https://khaos-id.vercel.app/login'
    );
    loginUrl.searchParams.set('redirect_to', callbackUrl.toString());
    return NextResponse.redirect(loginUrl.toString());
  }

  return supabaseResponse;
}

export const config = {
  matcher: ['/((?!_next/static|_next/image|favicon.ico|.*\\.(?:svg|png|jpg|jpeg|gif|webp)$).*)'],
};
```

---

## Gating a Server Component Page

Pages that should show claims (but are in `alwaysPublicRoutes`) handle their own auth:

```typescript
import { redirect } from 'next/navigation';
import { getVerifiedClaims } from '@/lib/khaos-id';

const KHAOS_ID_LOGIN_URL = process.env.KHAOS_ID_LOGIN_URL ?? 'https://khaos-id.vercel.app/login';
const SITE_URL = process.env.NEXT_PUBLIC_SITE_URL ?? 'https://khaos-mouseion.cotoaga.ai';

export default async function GatedPage() {
  const claims = await getVerifiedClaims();
  if (!claims) {
    const callbackUrl = `${SITE_URL}/api/auth/callback?next=/this-route`;
    const loginUrl = new URL(KHAOS_ID_LOGIN_URL);
    loginUrl.searchParams.set('redirect_to', callbackUrl);
    redirect(loginUrl.toString());
  }
  // ... render with claims.sub, claims.email, etc.
}
```

---

## Hard Rules (Never Violate)

| Rule | Why |
|------|-----|
| No `users` / `profiles` table | Identity lives in khaos-id. Per-user data joins to `sub` (UUID) only. |
| JWT in httpOnly cookie only | Browser JS never sees tokens. `@supabase/ssr` sets and reads them. |
| Verify, don't trust session payload alone | Always `jwtVerify()` against JWKS — `getSession()` alone is not enough for sensitive operations. |
| `SUPABASE_JWKS_URL` must be explicit | So siblings can repoint at a different Supabase project without code changes. |

---

## What This Seed Does NOT Cover

- khaos-id internals (login UI, magic link vs. password, OAuth providers) — khaos-id's own CLAUDE.md
- Supabase RLS policies for per-user data — project-specific; join on `sub` column
- Token refresh — `@supabase/ssr` handles this automatically via the middleware cookie dance

---

*The seed is planted. khaos-id owns identity. KHAOS apps verify it.*
