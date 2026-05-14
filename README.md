# 🌱 KHAOS Seeds

Ecosystem-wide patterns for Claude Code. When a KHAOS project needs to integrate with shared infrastructure, the relevant seed tells Claude Code exactly how — based on real implementations, not generic tutorials.

## Seeds

| Seed | Purpose | Source Truth |
|------|---------|-------------|
| [seed-style.md](seed-style.md) | COTOAGA × KHAOS Design System — tokens, typography, color, edges, components, brand discipline (Layers 1–3) | Reference implementations: `apex-recruiter` (dark), Klein Bottle, AI Risk Amplification, Industrial AI Complex |
| [seed-multiverse.md](seed-multiverse.md) | KHAOS-Multiverse cosmology — Cognitive Sovereignty, the Instruments, Layer 0 substrate (Kydroon), Layer 4 capstone | The workshop's operational doctrine |
| [seed-supabase.md](seed-supabase.md) | Supabase integration (two shapes: Next.js SSR + Service) | `be-part-of-net`, `khaos-researcher` |
| [seed-pluto.md](seed-pluto.md) | Pluto system registry + watchdog dashboard (v2.2) | Original design |
| [seed-health-endpoint.md](seed-health-endpoint.md) | Standard health endpoint shape for Pluto consumption (24h/7d/28d) | `apex-recruiting` |

## Usage

Every KHAOS project's `CLAUDE.md` should reference the seeds repo:

```markdown
## Ecosystem Seeds
Clone `cotoaga/khaos-seeds` alongside this project.
Before implementing Supabase: read `../khaos-seeds/seed-supabase.md`
Before styling any cotoaga × KHAOS artifact: read `../khaos-seeds/seed-style.md`
For strategic / cosmological context: read `../khaos-seeds/seed-multiverse.md`

## Seed Pins
Style Seed: v2.1
Multiverse Seed: v1.0  (omit if project is pure-implementation)
```

Or via global Claude Code config (`~/.claude/CLAUDE.md`):

```markdown
## Ecosystem Seeds
KHAOS seeds at ../khaos-seeds/ (relative to any project).
Read relevant seeds before implementing shared patterns.
```

## Adding Seeds

A seed captures a **proven pattern** extracted from working code or working doctrine. Not aspirational architecture — actual implementation.

Requirements:
- Name: `seed-{domain}.md`
- Must reference the source repo(s) or source-of-doctrine it was extracted from
- Must include a **"What This Seed Does NOT Cover"** section
- Must be directly usable by Claude Code (concrete) — OR — must declare actionable vs context layers if doctrine-heavy
- Versioned with `**Version:** X.Y` header
- Cross-references to companion seeds resolve by name, not by version pinning

## Seed Types

Most seeds are **concrete implementation patterns** (supabase, pluto, health-endpoint, style). The seed teaches Claude Code how to build the thing.

Some seeds are **doctrine seeds** (multiverse). The seed teaches operators (Kurt, KHAOS, Claude Code in strategic context) the *why* underneath the implementation seeds. Doctrine seeds are read once and consulted rarely; implementation seeds are read every time the relevant work begins.

Both kinds live in this repo. They cross-reference each other where it matters.

## Versioning

- **Pin per project.** Projects declare which seed versions they're built against in their `CLAUDE.md`.
- **Forward-only evolution.** New seed versions ship; old projects keep their pin until they upgrade deliberately.
- **Upgrade is an ADR.** Bumping a pinned seed version is a documented decision in `docs/adr/`.
- **Coupling is loose.** Seeds reference each other by name; they evolve independently.

---

*Seeds grow. Frameworks fossilize. Plant seeds.*
