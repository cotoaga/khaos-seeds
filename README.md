# 🌱 KHAOS Seeds

Ecosystem-wide patterns for Claude Code. When a KHAOS project needs to integrate with shared infrastructure, the relevant seed tells Claude Code exactly how — based on real implementations, not generic tutorials.

## Seeds

| Seed | Purpose | Source Truth |
|------|---------|-------------|
| [seed-supabase.md](seed-supabase.md) | Supabase integration (two shapes: Next.js SSR + Service) | `be-part-of-net`, `khaos-researcher` |
| [seed-pluto.md](seed-pluto.md) | Pluto system registry + watchdog dashboard | Original design |

## Usage

Every KHAOS project's `CLAUDE.md` should reference the seeds repo:

```markdown
## Ecosystem Seeds
Clone `cotoaga/khaos-seeds` alongside this project.
Before implementing Supabase: read `../khaos-seeds/seed-supabase.md`
```

Or via global Claude Code config (`~/.claude/CLAUDE.md`):

```markdown
## Ecosystem Seeds
KHAOS seeds at ../khaos-seeds/ (relative to any project).
Read relevant seeds before implementing shared patterns.
```

## Adding Seeds

A seed captures a **proven pattern** extracted from working code. Not aspirational architecture — actual implementation.

Requirements:
- Name: `seed-{domain}.md`
- Must reference the source repo(s) it was extracted from
- Must include a "What This Seed Does NOT Cover" section
- Must be directly usable by Claude Code (concrete, not philosophical)

---

*Seeds grow. Frameworks fossilize. Plant seeds.*
