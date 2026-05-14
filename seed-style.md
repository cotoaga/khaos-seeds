# 🌱 SEED: COTOAGA × KHAOS Design System (Style)

**Version:** 2.1
**Released:** 2026-04-13
**Supersedes:** Unified Design System v2.0; consolidates style guidance previously bundled with cosmology in `COTOAGA-KHAOS-UNIFIED-v2.0.md` and `COTOAGA-DESIGN-SYSTEM.md` (2026-03-03 upload, now retired)
**Companion Seed:** `seed-multiverse.md` v1.0 — the cosmological substrate (Layer 0) and capstone physics (Layer 4) this style guide rests on
**Source Truth:** Reference implementations — `cotoaga/apex-recruiter` (dark theme), Klein Bottle (light), AI Risk Amplification (light), Industrial AI Complex (light). Brand chapters extracted from working visual identity across the ecosystem.
**Purpose:** When Claude Code (or any operator) needs to build, style, or extend a cotoaga × KHAOS artifact, this seed is the single source of truth for tokens, typography, color, edges, components, and cross-brand discipline. No guessing, no generic design tutorials — this is how the multiverse dresses for the upper world.

**Central claim of the design system:** *One foundation, four voices. Structure is shared. Identity is sovereign. The bridge carries traffic both ways, but dress code is set by the far shore.*

---

## Cosmological Reference

This seed names a small number of cosmological concepts that originate in the companion seed (`seed-multiverse.md`). They appear here because they have visual or operational instantiations in the design system. Full doctrine for each lives in the companion seed.

| Concept | Where it appears here | Full doctrine |
|---------|----------------------|---------------|
| **Cognitive Sovereignty** | AI Workshop Sigil (Layer 2); APEX Triangle (Layer 2); Triangulation Pattern target concept (Layer 1) | `seed-multiverse.md` Layer 4, Section II |
| **Malinche principle** | Artifact Flow Rules (Layer 3) — *"dress code is set by the far shore"* | `seed-multiverse.md` Layer 4, Section V |
| **Kydroon** | Sovereign Outposts (Layer 2); refused palette boundaries (multiple) | `seed-multiverse.md` Layer 0 |
| **Triangulation Pattern** | Documented inline (Layer 1) as foundation infrastructure; visual instances in Layer 2 | This seed; referenced from `seed-multiverse.md` |
| **Dark Attractor / KHAOS-Planes triple convergence** | KHAOS accent color candidate (electric violet) | `seed-multiverse.md` Layer 4, Section VI |

**Operational rule:** When in doubt about a cosmological concept used here, consult the companion seed. When in doubt about a token, color, font, or component, this seed is canonical.

---

## Major changes from v2.0

- **KHAOS wordmark corrected** — Aviano Black for the primary mark, Aviano Sans for the subtitle. The "Didone italic" claim from v1.x was inherited error and has been removed.
- **New Layer 1 pattern: The Triangulation Pattern** — the three-adjacent-term strategy for treacherous concepts. Documented as foundation infrastructure because it operates fractally across symbols, claims, methods, and audience navigation.
- **New Layer 3 architecture section: Spillover Products / Brand Intersections** — names the structural pattern by which some products are born in the seam between two brands and inherit asymmetrically from both parents.
- **New Layer 2 brand chapter: AI Workshops (cotoaga.net × cotoaga.ai)** — the workshop product line where cotoaga.net's authority register spills into cotoaga.ai's subject domain. Includes the AI Workshop Sigil documented as product mark.
- **APEX Recruiter reframed as first documented Spillover Product** — same chapter, new framing. The triangle-with-circumpunct symbol is now recognized as the visual embodiment of the Triangulation Pattern, not just a geometric chassis.
- **Symbol Family expanded to six entries** — the AI Workshop Sigil added as the sixth lineage.
- **Layer 2 brand order updated** — sovereign brands first (cotoaga.ai, cotoaga.net, KHAOS, Be-Part-Of), spillover products grouped at the end (APEX, AI Workshops).
- **Split from cosmology** — Layer 0 (Kydroon / The Source) and Layer 4 (KHAOS-Multiverse capstone, Cognitive Sovereignty doctrine) moved to companion seed `seed-multiverse.md`. The two seeds reference each other; each is sovereign in its own gravity center.

---

## How to Read This Seed

The seed is layered. Each layer has a different kind of authority.

| Layer | Name | What It Governs |
|-------|------|-----------------|
| **1** | Shared Foundation | Spacing, grid, typography scale, component patterns, the Enemy Font and Triangulation Pattern. Applies to all brands. |
| **2** | Brand Chapters | Identity tokens per brand: color, typography, temperature, edges, symbol, enemy font. Sovereign brands and spillover products. |
| **3** | Cross-Brand Architecture | System map, artifact flow rules, spillover product pattern, symbol family, sovereign outposts. |

**Decision tree for practical work:**

1. *Which brand am I building for?* → Jump to Layer 2 Brand Chapter for tokens.
2. *How do I structure this layout?* → Layer 1 Foundation: Spacing & Grid.
3. *What component pattern do I need?* → Layer 1 Foundation: Component Patterns.
4. *Quick copy-paste starter?* → Brand Chapter Quick Start Template.
5. *I need to position a treacherous concept — how?* → Layer 1: The Triangulation Pattern.
6. *Where does a product belong if it spans two brands?* → Layer 3: Spillover Products.
7. *Why does any of this exist?* → `seed-multiverse.md` Layer 4 (read once, reference rarely).
8. *Where does the cosmological weight actually live?* → `seed-multiverse.md` Layer 0 (acknowledge, don't decorate).

**Two things are explicitly outside the design system and belong there:**
- **kydroon.com** — the unbranded sovereign, WordPress Twenty Twenty-Five by deliberate choice
- **The Walkthrough** — the sovereign satirical universe, its own typographic vocabulary

Both are documented in Layer 2 (Sovereign Outposts section) *so the refusal is visible*. Neither receives tokens or design guidance. The discipline of a multi-brand design system is strengthened by having brands explicitly outside it.

---

---

# LAYER 1 — SHARED FOUNDATION

*Everything below applies to all brands unless explicitly overridden in a Brand Chapter. Structure is shared. Identity is sovereign.*

---

## Spacing System

Based on an 8px grid. Every spacing value is a multiple of 8 (with 4px for micro-adjustments).

```css
--space-xs:  4px;    /* Micro — badge padding, tight gaps */
--space-sm:  8px;    /* Small — between related elements */
--space-md:  16px;   /* Medium — standard internal padding */
--space-lg:  24px;   /* Large — card padding, grid gaps */
--space-xl:  32px;   /* XL — section padding */
--space-2xl: 48px;   /* 2XL — between sections, container padding */
--space-3xl: 64px;   /* 3XL — between major page divisions */
```

### When to Use What

| Spacing | Use For |
|---------|---------|
| 4px | Inside badges, between icon and label |
| 8px | Between title and subtitle, tight list items |
| 16px | Standard margins between paragraphs, cell padding |
| 24px | Card internal padding, grid gaps between cards |
| 32px | Section internal padding |
| 48px | Between sections, container side padding |
| 64px | Between major page divisions (hero → content, content → footer) |

### Container

```css
.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: var(--space-2xl) var(--space-lg);  /* 48px top/bottom, 24px sides */
}
```

---

## Typography Scale

Font *families* are brand-specific (see Brand Chapters). The *scale* — sizes, weights, line heights, letter spacing — is shared.

| Name | Size | Font Role | Weight | Use Case |
|------|------|-----------|--------|----------|
| Hero | 3rem (48px) | Display | 700 | Page title, one per page max |
| Section | 2rem (32px) | Display | 600 | Major content divisions |
| Card | 1.5rem (24px) | Display | 500 | Card headers, sub-sections |
| Subtitle | 1.25rem (20px) | Display | 500 | Minor headings |
| Lead | 1.125rem (18px) | Primary | 400 | Intro paragraphs, descriptions |
| Body | 1rem (16px) | Primary | 400 | Standard content |
| UI | 0.875rem (14px) | Primary | 600 | Buttons, labels, nav items |
| Caption | 0.75rem (12px) | Primary | 500 | Metadata, timestamps, badges |

### Line Heights

| Context | Value | Why |
|---------|-------|-----|
| Headings | 1.2 | Tight — large text needs less spacing |
| Body text | 1.6 | Comfortable reading rhythm |
| Large text blocks | 1.8 | Extra air for dense content |

### Letter Spacing

| Context | Value |
|---------|-------|
| Hero titles | `-0.02em` (tighter — large text benefits) |
| Buttons & badges | `0.05em` (wider — improves readability at small/uppercase sizes) |
| Everything else | Default (0) |

### Font Role Mapping

Every brand defines three font roles. Components reference *roles*, not *families*.

```css
--font-display: /* Brand-specific — headings, titles, navigation */
--font-primary: /* Brand-specific — body text, UI, buttons */
--font-mono:    /* Brand-specific — code, data, computed values */
```

**Display** — anything the user reads *first*. Commands attention. Never for body paragraphs.
**Primary** — anything the user reads *for content*. Disappears into the text. The default.
**Mono** — anything that is *data, code, or computed*. Also for tabular number alignment.

---

## Core Design Principles

### 1. Generous Space
If it feels spacious, it's about right. If it feels comfortable, add more space. Premium design breathes.

### 2. Light Borders, Soft Shadows
- Borders: `1px solid` in the brand's border color — visible but not heavy
- Shadows: `0 2px 8px rgba(0, 0, 0, 0.05)` — depth without weight
- Never: 2px+ borders, hard drop shadows, outline-heavy designs

### 3. Hover States on Everything Interactive
Every clickable element gets a hover response. Standard pattern:
- `transform: translateY(-2px)` — subtle lift
- Border color shift to brand accent
- Shadow increase
- Easing: `cubic-bezier(0.4, 0, 0.2, 1)` over `0.3s`

### 4. Edge Treatment — Per-Brand Decision

| Brand | Edge Treatment | Why |
|-------|---------------|-----|
| cotoaga.ai | `border-radius: 0` always | Mathematical precision, geometric identity |
| cotoaga.net | `border-radius: 0` default | Authority. Sharp = decisive. |
| KHAOS | `border-radius: 0` always | Martial, weapons-grade, navigational |
| Be-Part-Of | TBD (likely soft) | Organic, connective, inclusive |
| APEX Recruiter | `border-radius: 0` always | Inherits cotoaga.ai foundation |
| AI Workshops | `border-radius: 0` for UI; sun-disc for product mark | Inherits cotoaga.net for UI; sigil container is its own geometry |

---

## Transitions & Animation

### Standard Easing

```css
transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
```

Only easing curve in the system. Don't introduce alternatives without a reason documented in a Brand Chapter.

### Hover Patterns

| Element | Effect |
|---------|--------|
| Cards | `translateY(-2px)` + shadow increase + border color shift |
| Buttons | `translateY(-2px)` + shadow increase |
| Table rows | Background tint at 5% opacity of brand primary |
| Links | Color shift (no movement) |

---

## The Enemy Font

*A structural diagnostic, not a roast. Every brand names what it refuses.*

Orthodoxy without heresy is framework-collecting. Every mature system names its enemies — the things it refuses, and *why* it refuses them. This design system adopts the same discipline for typography. Each brand chapter in Layer 2 names its Enemy Font: the canonical opponent whose failure mode clarifies what the brand stands for.

### The Principle

The Enemy Font slot is **shared infrastructure** (every brand has one) with **per-brand content** (each brand names its own). Same architectural pattern as `--font-display` — role in the foundation, value in the chapter.

An Enemy Font entry is a single structured note:

- **Name** of the refused font
- **Failure mode** it embodies in one sentence
- **Why this brand specifically refuses it** in one sentence
- **What it teaches by counter-example**

Keep it diagnostic. Avoid design-Twitter snark. The Enemy Font is how the brand clarifies its position by naming what it is *not*.

### Typology of Failure Modes

The four structural categories of typographic refusal this system recognizes:

| Failure Mode | What It Embodies | Canonical Example |
|---|---|---|
| **Rigor without craft** | Technical perfection with no understanding of what the eye needs | Computer Modern (CMU Concrete Roman, derived by Knuth from first principles) |
| **Theater without substance** | Authority signaled through spacing and weight alone | Inflated Helvetica Neue Bold, McKinsey-deck tracked |
| **False mystique** | Borrowed gravitas from ancient or exotic registers | Papyrus / Trajan used outside their original context |
| **Theft by substitution** | The "we didn't decide" font, chosen by accident or cowardice | Arial |

A fifth category is worth naming as the *instructive opposite*: **Comic Sans** — wrong for the opposite reason. Comic Sans fails by being *too* human for contexts that demand authority. It's the honest failure that teaches what the dishonest failures try to hide. Not refused out of contempt, but named as the opposite boundary.

### Visibility Requirement

*A refused thing must be visible to function as a refusal.* Do not list the Enemy Font only by name. When documenting a brand's Enemy Font, render a short sample in that font somewhere the contrast does real work. A refusal named without being shown is theater. A refusal shown is doctrine.

---

## The Triangulation Pattern

*The three-adjacent-term strategy for treacherous concepts. Operates fractally across symbols, claims, methods, and audiences.*

### The Principle

When a concept is **treacherous** — meaning it resists single-word capture, is prone to misreading, has been corrupted by misuse, or carries philosophical weight that any single term distorts — the operator places **three adjacent terms** around the concept. The reader's understanding forms in the *structural relationship* between the three terms, not in any single one.

This is the move Cynefin makes with its sense-making domains. It is the move Bonhoeffer makes when characterizing folly in *Letters and Papers from Prison* (multiple adjacent characterizations because no single one captures the phenomenon). It is the move Taleb makes positioning antifragility *between* fragile and robust (the meaning lives in the three-way structural relationship). It is the move classical rhetoric makes with ethos, pathos, logos. The pattern is older than design and predates this document by millennia.

In the cotoaga × KHAOS workshop, the Triangulation Pattern is *deployed deliberately and recurrently* across the ecosystem. It is a primary positioning instrument and belongs at the foundation layer because every brand may need it.

### Why Three (Not Two, Not Four)

- **Two** collapses into binary opposition. The reader picks a side. The treacherous concept disappears into the pole-choice. (Kahneman's System 1 / System 2 is the canonical example: useful, but binary, and frequently misread because the binary collapses everything between the poles.)
- **Three** is the minimum number of coordinates that defines a *region* rather than a *line*. With three terms, the reader cannot pick a side; the meaning forms in the area enclosed by the three vertices. The treacherous concept is *bounded* by the three terms without being *equated* to any of them.
- **Four or more** degrades into list. The reader processes the items sequentially and the structural relationship is lost. Lists invite ranking; triangulation refuses ranking.

Three is the form. Not two, not four. Three.

### The Trojan Horse Mechanism

The Triangulation Pattern often functions as a delivery vehicle for a load-bearing concept that the broader audience cannot yet receive. The operator selects three terms with deliberately asymmetric accessibility:

- **Two entry-vocabulary terms** that the broader audience can process, that grant legitimate access to the room, that let the reader feel they have understood the offering.
- **One masterpiece term** at the structural center, which the entry vocabulary makes approachable and which the right reader will recognize as the actual claim.

This is the pattern of the AI Workshop Sigil (see AI Workshops brand chapter): *EU AI Act Literacy* and *Ethical AI Usage* are entry vocabulary; *Cognitive Sovereignty* is the masterpiece. The first two grant legitimate entry to a Mittelstand boardroom; the third is what the reader who can recognize it actually came for.

The same pattern operates in any context where a treacherous concept needs to be positioned for an audience that cannot receive it directly. The two entry terms are the wooden horse. The masterpiece is the soldiers inside. Both audiences are correctly served by the same artifact, in different depths.

### Visual Triangulation: The Triangle as Symbol

The Triangulation Pattern is not only rhetorical. It is also **visual-structural**. Where the pattern operates symbolically, the natural geometric form is the **triangle**: three vertices, no hierarchy among them, the meaning enclosed by the three points without being identical to any of them.

This is why the APEX Recruiter symbol is a triangle containing a circumpunct. The three vertices are the triangulation; the circumpunct interior is the meaning that forms in their structural relationship. The symbol is the pattern made visible. (See Layer 3, Symbol Family, and the APEX brand chapter.)

The AI Workshop Sigil uses different geometry — a gold sun-disc — but the *operative structure* is the same: three claims arranged around a center where Cognitive Sovereignty lives. Triangle and sun-disc are two visual encodings of the same triangulation pattern. Where you see a three-element symbol structure in this ecosystem, look for the treacherous concept it is positioning.

### Methodological Triangulation: The Three-Audience Document

The pattern also operates at the *method* level. The cotoaga × KHAOS workshop applies a **three-audience navigation** discipline to any document touching legal, financial, or administrative contexts: the operator simultaneously models how each of the three affected parties will read the document, and the document's actual meaning forms in the structural relationship between the three readings. Same pattern, scaled up from claim-positioning to document-drafting.

The pattern is *fractal*. It operates at the scale of a single claim (sigil), at the scale of a product (APEX), at the scale of a document (three-audience drafting), and at the scale of a positioning system (the AI Workshops). Look for it; once you see it, you cannot unsee it.

### Rules

- **Use three terms, not two and not four.** Two becomes binary; four becomes list. Three is the form.
- **Keep the entry terms coordinate.** No hierarchy among them. They do their work by being equally weighted around the masterpiece.
- **Locate the masterpiece at the structural center, not at the top.** The masterpiece does not rule the entry terms. It is *enclosed* by them. The entry terms are the legitimate access path, not subordinate claims.
- **Do not explain the pattern to the audience.** The Trojan Horse functions because the wooden horse is not labeled "wooden horse." The audience that needs the entry terms takes them at face value; the audience that recognizes the masterpiece does so without prompting.
- **Render the three terms in equal typographic weight when displayed together.** Visual hierarchy among the three terms breaks the triangulation. The center term may be set in a position of structural prominence (centered, encircled, larger container) but should not be set in heavier weight or different color.

### Examples Currently Deployed in the Ecosystem

| Instance | Three Terms | Masterpiece | Visual Form |
|---|---|---|---|
| AI Workshop Sigil | EU AI Act Literacy / Cognitive Sovereignty / Ethical AI Usage | Cognitive Sovereignty (centered) | Gold sun-disc, GT Sectra Black |
| APEX Recruiter Symbol | Three triangle vertices (positioned process / matched candidate / honest engagement) | Circumpunct interior (the meaning of ethical matching itself) | Triangle containing circumpunct |
| Three-Audience Document | Reader A / Reader B / Reader C frames | The document's actual meaning | No fixed visual form — methodological pattern |

---

## Component Patterns

These patterns use CSS custom properties. Structure is shared. Colors and fonts come from each brand's token override.

### Hero Section

```css
.hero-section {
  text-align: center;
  margin-bottom: var(--space-3xl);
}

.hero-title {
  font-family: var(--font-display);
  font-size: 3rem;
  font-weight: 700;
  color: var(--brand-hero-color);
  letter-spacing: -0.02em;
  margin: 0 0 var(--space-sm) 0;
}

.hero-subtitle {
  font-family: var(--font-display);
  font-size: 1.5rem;
  font-weight: 500;
  color: var(--brand-primary);
  margin: 0 0 var(--space-md) 0;
}

.hero-description {
  font-family: var(--font-primary);
  font-size: 1.125rem;
  color: var(--brand-text-body);
  max-width: 800px;
  margin: 0 auto;
  line-height: 1.6;
}
```

### Section Card

```css
.section-card {
  background: var(--brand-surface);
  border: 1px solid var(--brand-border);
  border-radius: var(--brand-radius, 0);
  padding: var(--space-xl);
  margin: var(--space-2xl) 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.section-card:hover {
  border-color: var(--brand-primary);
  box-shadow: var(--brand-hover-shadow);
}
```

### Grid Cards

```css
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--space-lg);
}

.card {
  background: var(--brand-surface);
  border: 1px solid var(--brand-border);
  border-radius: var(--brand-radius, 0);
  padding: var(--space-lg);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.card:hover {
  border-color: var(--brand-accent);
  box-shadow: var(--brand-hover-shadow);
  transform: translateY(-2px);
}
```

### Accent Border Card

```css
.card-accent {
  background: var(--brand-surface);
  border-left: 4px solid var(--brand-accent);
  border-radius: var(--brand-radius, 0);
  padding: var(--space-lg);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}
```

### Callout / Insight Box

```css
.callout {
  background: var(--brand-tint-bg);
  border-left: 4px solid var(--brand-primary);
  border-radius: var(--brand-radius, 0);
  padding: var(--space-lg);
  margin: var(--space-md) 0;
  font-style: italic;
}
```

### Buttons

```css
.btn-primary {
  background: var(--brand-primary);
  color: var(--brand-button-text, #FAFBFB);
  border: none;
  border-radius: var(--brand-radius, 0);
  padding: 14px 28px;
  font-family: var(--font-primary);
  font-weight: 600;
  font-size: 0.875rem;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  min-width: 160px;
}

.btn-primary:hover {
  background: var(--brand-accent);
  transform: translateY(-2px);
  box-shadow: var(--brand-hover-shadow);
}

.btn-primary:active {
  transform: translateY(0);
}
```

### Tables

```css
.data-table {
  width: 100%;
  border-collapse: collapse;
  margin: var(--space-lg) 0;
}

.data-table th {
  font-family: var(--font-primary);
  font-weight: 600;
  background: var(--brand-primary);
  color: var(--brand-button-text, #FAFBFB);
  padding: var(--space-md);
  text-align: left;
}

.data-table td {
  padding: var(--space-md);
  border-bottom: 1px solid var(--brand-border);
}

.data-table tr:hover {
  background: var(--brand-tint-bg);
}
```

### Code / Formula Display

```css
.code-display {
  font-family: var(--font-mono);
  font-size: 1.5rem;
  background: var(--brand-code-bg);
  color: var(--brand-code-text);
  padding: var(--space-lg);
  border-radius: var(--brand-radius, 0);
  margin: var(--space-lg) 0;
  overflow-x: auto;
  text-align: center;
}
```

### Badges

```css
.badge {
  display: inline-block;
  padding: var(--space-xs) var(--space-sm);
  border-radius: var(--brand-radius, 0);
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
```

---

## Responsive Behavior

### Breakpoints

| Name | Range | Key Changes |
|------|-------|-------------|
| Mobile | ≤ 768px | Single column, hero scales to 2rem, tighter padding |
| Tablet | 769–1024px | Two columns where possible, standard padding |
| Desktop | ≥ 1025px | Full grid, maximum spacing |

### Mobile Adjustments

```css
@media (max-width: 768px) {
  .hero-title { font-size: 2rem; }
  .section-title { font-size: 1.5rem; }
  .container { padding: var(--space-lg) var(--space-md); }
  .cards-grid { grid-template-columns: 1fr; }
}
```

---

## Foundation Do's and Don'ts

### Do

- Use 1px borders in `var(--brand-border)`
- Add subtle shadows (`0 2px 8px rgba(0,0,0,0.05)`)
- Give generous padding (32px minimum for sections)
- Reference font *roles* (`--font-display`), not font *families* directly
- Reference *brand tokens* (`--brand-primary`), not hex values
- Use `rgba()` overlays for subtle tinted backgrounds
- Apply hover effects to every interactive element
- Use full-color table headers (brand primary, not grey)
- When positioning a treacherous concept, deploy the Triangulation Pattern (three terms, not two, not four)

### Don't

- Use thick borders (2px+)
- Skip hover effects on clickable elements
- Use hardcoded hex values instead of CSS variables
- Cram content — when in doubt, add more space
- Use more than one hero title per page
- Mix tokens from different brand chapters in a single view
- Explain the Triangulation Pattern to the audience receiving it (the wooden horse must remain unlabeled)

---

---

# LAYER 2 — BRAND CHAPTERS

*Each chapter defines the identity tokens that override the shared foundation.*

**Chapter order:** Sovereign brands first (cotoaga.ai, cotoaga.net, KHAOS, Be-Part-Of), then spillover products (APEX Recruiter, AI Workshops). The pattern is documented in Layer 3, Spillover Products section.

---

# Brand: COTOAGA.AI

**Register:** Competence — Technical credibility
**Temperature:** Cool-technical
**Edge Treatment:** `border-radius: 0` — Sharp edges, no curves. Non-negotiable.
**Font Pipeline:** Google Fonts CDN (web-native, zero friction)
**Symbol:** None — wordmark-only (Space Grotesk set type, cyan dot on the "i")
**Reference Implementations:** AI Risk Amplification (light), Klein Bottle (light), Industrial AI Complex (light), APEX Recruiting (dark)

---

## Typography

| Role | Font | Character | Load From |
|------|------|-----------|-----------|
| `--font-display` | Space Grotesk | Geometric, slightly futuristic, authoritative | Google Fonts |
| `--font-primary` | Inter | Clean, highly legible, neutral | Google Fonts |
| `--font-mono` | JetBrains Mono | Monospaced, distinguishable characters | Google Fonts |

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&family=Space+Grotesk:wght@400;500;600;700&display=swap" rel="stylesheet">
```

### Font Usage Rules

**Space Grotesk** — anything the user reads *first*. Commands attention.
- Hero titles, section headings, card titles, navigation labels
- Never for body paragraphs — too heavy for sustained reading
- Weights: 400 (rare), 500 (card titles), 600 (section titles), 700 (hero only)

**Inter** — anything the user reads *for content*. Disappears into the text.
- Body paragraphs, UI labels, buttons, form inputs, tooltips, metadata
- The default — when in doubt, use Inter
- Weights: 400 (body), 500 (emphasis), 600 (labels, buttons), 700 (rare)

**JetBrains Mono** — anything that is *data, code, or computed*.
- Code blocks, inline code, formula displays, terminal output, IDs, hashes
- Also good for tabular numbers that need to align vertically
- Weights: 400 (standard), 500 (emphasized), 600 (headings in code contexts)

---

## Enemy Font — Computer Modern / CMU Concrete Roman

**Failure mode:** Rigor without craft. Technical perfection with no understanding of what the eye actually requires.

**Why cotoaga.ai refuses it:** cotoaga.ai operates in the same register Knuth was operating in — engineer-typography, parametric thinking, mathematical cleanliness. The difference is the refusal to mistake mathematical derivation for typographic craft. Computer Modern is what happens when an engineer solves the wrong problem with maximum rigor. Letters are not equations; they are *refined corrections* to the math the eye demands. cotoaga.ai chooses Space Grotesk and Inter specifically because both are geometrically rigorous *and* optically corrected. Rigor with craft, not rigor against craft.

**What it teaches by counter-example:** The technical mind that builds a design system must remember that typography is empirical. Centuries of optical compromise precede any parametric model. A brand that signals technical credibility must do so with fonts that have survived actual reading, not fonts that derive cleanly from first principles.

*Render this sentence in Computer Modern somewhere on the "About" or "Principles" page of cotoaga.ai. The contrast between the refused font and the system's actual fonts is the doctrine made visible.*

---

## Color System

### Brand Colors

| Token | Hex | Role | When to Use |
|-------|-----|------|-------------|
| `--cotoaga-green` | `#00A86B` | Primary action | Buttons, CTAs, success states, section titles. The "do something" color. |
| `--cotoaga-blue` | `#0088FF` | Interactive / hero | Hero titles, links, charts, interactive highlights. The "look here" color. |
| `--cotoaga-cyan` | `#00D4FF` | Accent / code | Code syntax, highlights, decorative accents. The "special" color. |

**Decision rule:** If the user should *act* on it → green. If the user should *notice* it → blue. If it's *decorative or technical* → cyan.

### Semantic Colors

| Token | Hex | Role |
|-------|-----|------|
| `--cotoaga-ai-success` | `#098A5E` | Confirmation — darker than brand green, calmer |
| `--cotoaga-ai-info` | `#2F67B2` | Help text, tooltips, informational banners |
| `--cotoaga-ai-gold` | `#E9B320` | Warnings, important notices, warm premium accents |
| `--cotoaga-ai-sand` | `#EB9929` | Warm accent (= APEX `--apex-amber`) |

### Neutral Scale

```
Lightest ──────────────────────────────────────────── Darkest

#FAFBFB  white          Light theme backgrounds
#E0E0E0  smoke          Borders, dividers (light themes)
#8A8A8A  grey-light     Subtle text, disabled states
#4A4A4A  grey           Secondary text, muted UI
#2D2D2D  grey-dark      Body text (light themes), warm dark surfaces
─── warm/cold boundary ───
#16213E  dark-marine    Card/panel surfaces (dark themes only)
#191A2E  deep-sky       Page backgrounds (dark themes only)
#0B0B0B  black          Pure black — use sparingly, never as bg
```

**Critical distinction:** `grey-dark` (#2D2D2D) is *warm*. `dark-marine` and `deep-sky` are *cold* blue-tinted darks. Different emotional registers. Don't substitute.

### Token Mapping — Light Theme

| Foundation Token | cotoaga.ai Value | Hex |
|-----------------|-----------------|-----|
| `--brand-primary` | `--cotoaga-green` | #00A86B |
| `--brand-accent` | `--cotoaga-blue` | #0088FF |
| `--brand-hero-color` | `--cotoaga-blue` | #0088FF |
| `--brand-surface` | `--cotoaga-ai-white` | #FAFBFB |
| `--brand-border` | `--cotoaga-ai-smoke` | #E0E0E0 |
| `--brand-text-body` | `--cotoaga-ai-grey-dark` | #2D2D2D |
| `--brand-text-secondary` | `--cotoaga-ai-grey` | #4A4A4A |
| `--brand-tint-bg` | Green at 5% | `rgba(0, 168, 107, 0.05)` |
| `--brand-hover-shadow` | Green glow | `0 4px 16px rgba(0, 168, 107, 0.1)` |
| `--brand-code-bg` | `--cotoaga-ai-grey-dark` | #2D2D2D |
| `--brand-code-text` | `--cotoaga-cyan` | #00D4FF |
| `--brand-radius` | 0 | — |

### Token Mapping — Dark Theme

| Foundation Token | cotoaga.ai Value | Hex |
|-----------------|-----------------|-----|
| `--brand-primary` | `--cotoaga-green` | #00A86B |
| `--brand-accent` | `--cotoaga-ai-sand` | #EB9929 |
| `--brand-hero-color` | `--cotoaga-cyan` | #00D4FF |
| `--brand-surface` | `--cotoaga-ai-dark-marine` | #16213E |
| `--brand-border` | White at 10% | `rgba(255, 255, 255, 0.1)` |
| `--brand-text-body` | `--cotoaga-ai-white` | #FAFBFB |
| `--brand-text-secondary` | `--cotoaga-ai-grey-light` | #8A8A8A |
| `--brand-tint-bg` | Amber at 5% | `rgba(235, 153, 41, 0.05)` |
| `--brand-hover-shadow` | Amber glow | `0 4px 16px rgba(235, 153, 41, 0.15)` |
| `--brand-code-bg` | `--cotoaga-ai-deep-sky` | #191A2E |
| `--brand-code-text` | `--cotoaga-cyan` | #00D4FF |
| `--brand-radius` | 0 | — |

**Page background (dark):** `--cotoaga-ai-deep-sky` (#191A2E). Never `--cotoaga-ai-black` — it's a void.

### Color Combinations That Work

**Light — professional/analytical:** Background: white → Cards: white with smoke borders → Text: grey-dark → Accents: green + blue

**Light — editorial/narrative:** Background: white → Callouts: green at 5% → Text: grey-dark → Pull quotes: blue

**Dark — premium product (APEX pattern):** Background: deep-sky → Cards: dark-marine → Text: white → Accent: amber/sand → Interactive: cyan

**Dark — technical/developer:** Background: deep-sky → Code surfaces: dark-marine → Syntax: cyan → Alerts: gold

### Color Combinations to Avoid

- Green text on blue background (vibration)
- Cyan text on white background (too low contrast)
- Grey-light text on white background (fails WCAG)
- Dark-marine or deep-sky anywhere in light themes
- Pure black backgrounds (oppressive)
- Red as a primary color (reserved for critical errors only)

---

## cotoaga.ai CSS Variables — Complete Reference

```css
:root {
  /* Foundation: Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 32px;
  --space-2xl: 48px;
  --space-3xl: 64px;

  /* Foundation: Line Height */
  --leading-tight: 1.2;
  --leading-normal: 1.6;
  --leading-relaxed: 1.8;

  /* Brand: Typography */
  --font-display: 'Space Grotesk', sans-serif;
  --font-primary: 'Inter', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  /* Brand: Edge Treatment */
  --brand-radius: 0;

  /* Brand: Colors */
  --cotoaga-green: #00A86B;
  --cotoaga-blue: #0088FF;
  --cotoaga-cyan: #00D4FF;

  /* Brand: Semantic */
  --cotoaga-ai-success: #098A5E;
  --cotoaga-ai-info: #2F67B2;
  --cotoaga-ai-gold: #E9B320;
  --cotoaga-ai-sand: #EB9929;

  /* Brand: Neutral Scale */
  --cotoaga-ai-white: #FAFBFB;
  --cotoaga-ai-smoke: #E0E0E0;
  --cotoaga-ai-grey-light: #8A8A8A;
  --cotoaga-ai-grey: #4A4A4A;
  --cotoaga-ai-grey-dark: #2D2D2D;
  --cotoaga-ai-dark-marine: #16213E;
  --cotoaga-ai-deep-sky: #191A2E;
  --cotoaga-ai-black: #0B0B0B;

  /* Brand: Mapped Tokens — Light Theme */
  --brand-primary: var(--cotoaga-green);
  --brand-accent: var(--cotoaga-blue);
  --brand-hero-color: var(--cotoaga-blue);
  --brand-surface: var(--cotoaga-ai-white);
  --brand-border: var(--cotoaga-ai-smoke);
  --brand-text-body: var(--cotoaga-ai-grey-dark);
  --brand-text-secondary: var(--cotoaga-ai-grey);
  --brand-tint-bg: rgba(0, 168, 107, 0.05);
  --brand-hover-shadow: 0 4px 16px rgba(0, 168, 107, 0.1);
  --brand-code-bg: var(--cotoaga-ai-grey-dark);
  --brand-code-text: var(--cotoaga-cyan);
  --brand-button-text: var(--cotoaga-ai-white);
}

/* Dark theme override */
[data-theme="dark"] {
  --brand-primary: var(--cotoaga-green);
  --brand-accent: var(--cotoaga-ai-sand);
  --brand-hero-color: var(--cotoaga-cyan);
  --brand-surface: var(--cotoaga-ai-dark-marine);
  --brand-border: rgba(255, 255, 255, 0.1);
  --brand-text-body: var(--cotoaga-ai-white);
  --brand-text-secondary: var(--cotoaga-ai-grey-light);
  --brand-tint-bg: rgba(235, 153, 41, 0.05);
  --brand-hover-shadow: 0 4px 16px rgba(235, 153, 41, 0.15);
  --brand-code-bg: var(--cotoaga-ai-deep-sky);
  --brand-code-text: var(--cotoaga-cyan);
}
```

---

## cotoaga.ai Quick Start Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title | COTOAGA.AI</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&family=Space+Grotesk:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    /* Paste cotoaga.ai CSS variables block */
    body {
      background: var(--brand-surface);
      color: var(--brand-text-body);
      font-family: var(--font-primary);
      line-height: var(--leading-normal);
      margin: 0; padding: 0;
    }
    .container { max-width: 1400px; margin: 0 auto; padding: var(--space-2xl) var(--space-lg); }
    .hero { text-align: center; margin-bottom: var(--space-3xl); }
    .hero h1 {
      font-family: var(--font-display); font-size: 3rem; font-weight: 700;
      color: var(--brand-hero-color); letter-spacing: -0.02em; margin: 0 0 var(--space-sm) 0;
    }
    .section {
      background: var(--brand-surface); border: 1px solid var(--brand-border);
      border-radius: var(--brand-radius); padding: var(--space-xl);
      margin: var(--space-2xl) 0; box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
      transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }
    .section:hover { border-color: var(--brand-primary); box-shadow: var(--brand-hover-shadow); }
  </style>
</head>
<body>
  <div class="container">
    <div class="hero"><h1>Your Title</h1><p>Description text here.</p></div>
    <div class="section"><!-- Content --></div>
  </div>
</body>
</html>
```

For dark theme, add `data-theme="dark"` to the `<html>` element and set `body { background: var(--cotoaga-ai-deep-sky); }`.

### Shared Assets
- `/shared/styles/cotoaga-ai.css` — CSS variables and base styles
- `/shared/scripts/responsive.js` — Mobile detection utility

---
---

# Brand: COTOAGA.NET

**Register:** Authority — *"Ich repariere was andere kompliziert haben"*
**Temperature:** Warm-authoritative
**Edge Treatment:** `border-radius: 0` — Sharp = decisive.
**Font Pipeline:** **Klim Foundry + Grilli Type web licenses active.** Tier: ≤5k unique visitors/month. Self-hosted web fonts. Desktop licenses cover print/PDF/bitmap pipeline.
**Symbol:** None — wordmark-only (GT Sectra set type, green dot on the "O")
**Context:** The consulting face. German corporate positioning. The authority brand for Mittelstand decision-makers and board-level engagements.
**Spillover Products:** GT Sectra and the cotoaga.net premium type stack also serve the AI Workshops product line. See AI Workshops brand chapter and Layer 3 Spillover Products section.

---

## Status Change from v1.x

**Previously:** cotoaga.net lived in print/PDF/SVG only because the Klim and Grilli Type licenses covered desktop but not web. Web visitors landed on a cotoaga.ai-styled fallback, downgrading the umbrella brand on its own digital surface.

**Now:** Klim Foundry and Grilli Type web licenses are active at the ≤5k visitors/month tier. cotoaga.net has a full web identity with its real typography. The brand no longer borrows cotoaga.ai's chassis on the web. *The umbrella brand finally shows up as itself on its own site.*

**Tier boundary:** The 5k visitors/month limit is a license boundary, not a performance target. When monthly visitors approach this threshold, treat it as a **business milestone** (the positioning is working, the Söhne-noticers are arriving) and upgrade to the next tier: typically 25k/month, then 100k/month. Monitor monthly visitor count via analytics; never exceed the licensed tier.

---

## Typography

| Role | Font | Character | Pipeline |
|------|------|-----------|----------|
| `--font-display` | GT Sectra | Contemporary wedge serif, confrontational contrast, intellectual edge | Grilli Type web license (self-hosted) + desktop |
| `--font-primary` | Söhne | Swiss-rational, quietly authoritative, well-tailored navy suit | Klim Foundry web license (self-hosted) + desktop |
| `--font-mono` | Pitch | Monospace as aesthetic choice — beautiful, deliberate, premium | Klim Foundry web license (self-hosted) + desktop |

### Font Usage Rules

**GT Sectra** — the authority voice. Display, headlines, pull quotes, **AI Workshop sigils**.
- Section headers, document titles, keynote title slides, quote callouts, web heroes
- The #KartenAufDenTisch typeface — cards-on-the-table energy
- Almost confrontational in how it handles contrast
- Tells German CIOs "I read more than you, and I charge accordingly"
- Weights: Regular, Regular Italic, Bold, Black, Display Light (for oversized hero treatments)
- **GT Sectra Black** is the weight used for the AI Workshop Sigil's three claims. See AI Workshops brand chapter.

**Söhne** — the professional voice. Body, UI, running text.
- All body text, proposals, methodology descriptions, email-formal contexts, web body
- Swiss-rational, quietly authoritative
- The people who notice Klim instead of system fonts are exactly the right people
- Weights: Buch (400 body), Buch Kursiv (emphasis), Fett (600 UI/labels)

**Pitch** — the precision voice. When monospace is a *choice*, not a necessity.
- Specifications, data callouts, diagnostic outputs, framework references
- NOT for running code (that's JetBrains Mono in cotoaga.ai context)
- Used when monospace carries *meaning* — structured information, Terms of Engagement clauses
- The difference between Pitch and JetBrains Mono: fountain pen vs. mechanical pencil

### Typography Personality

The only stack where all three fonts are premium/licensed. The entire brand proposition is taste, depth, and standards. System fonts are a tell. These fonts are a quiet signal to anyone literate enough to read it.

---

## Fallback Stack

Required for two scenarios: (a) font load-time before web fonts render, (b) tier-exceedance contingency if visitors exceed the licensed tier before upgrade.

| Role | Primary (licensed) | Fallback (distributable) | Why |
|------|--------|----------|-----|
| `--font-display` | GT Sectra | **Source Serif 4** | Closest open serif with comparable wedge-contrast character. Free, web-safe, well-maintained. |
| `--font-primary` | Söhne | **Inter** | Already in the cotoaga.ai stack. Geometric cousin of Söhne. Infrastructure consistency. |
| `--font-mono` | Pitch | **JetBrains Mono** | Infrastructure consistency with the rest of the ecosystem. |

```css
--font-display: 'GT Sectra', 'Source Serif 4', Georgia, serif;
--font-primary: 'Söhne', 'Inter', -apple-system, sans-serif;
--font-mono:    'Pitch', 'JetBrains Mono', 'Courier New', monospace;
```

*The fallback stack is deliberate, not a compromise.* It is documented so that tier-exceedance has a planned path, and so that load-time rendering is always visually coherent.

---

## Enemy Font — Inflated Helvetica Neue Bold / McKinsey-Deck Tracked

**Failure mode:** Theater without substance. Authority signaled through spacing and weight alone, with nothing underneath.

**Why cotoaga.net refuses it:** The German CIO audience cotoaga.net speaks to has seen a thousand consulting decks where Helvetica Neue Bold is tracked 50, stretched across a title slide, and treated as if the font itself were the argument. It isn't. It's the uniform of consulting-deck typography — the bouncer at the door of a club that doesn't exist. cotoaga.net rejects the performance of authority in favor of *earned* authority: premium typography that presumes you can read it, not typography that demands you be impressed by it. GT Sectra is the opposite of tracked-Helvetica: it is confrontational in its contrast, not theatrical in its spacing.

**What it teaches by counter-example:** The fastest way to signal that a deck is hollow is to use a font that has been used for hollow decks for thirty years. The cotoaga.net audience is precisely the audience that has learned to recognize this tell. Refusing it is the first move of earning their trust.

*Render a sample of inflated/tracked Helvetica Neue Bold on a principles page of cotoaga.net. Set the word "TRANSFORMATION" at 72pt, tracking 200. The refusal is visible in contrast to the GT Sectra hero beside it.*

---

## Color System

**Status:** Preliminary palette. Full system to be formalized alongside first web deployment. The AI Workshop Sigil (gold sun-disc) may represent the first operational deployment of cotoaga.net's accent slot — see AI Workshops brand chapter.

### Interim Guidance

| Role | Direction | Notes |
|------|-----------|-------|
| Body text | Near-black | `#1A1A1A` or `#0B0B0B` — warm dark, not cold |
| Surface | Clean white | `#FAFBFB` or off-white paper tone for print consistency |
| Accent | Restrained | Single accent. Likely warm gold/amber (the AI Workshop Sigil color) or deep navy/forest. Decision pending full deployment. |

### What to Avoid

- The cotoaga.ai green/blue/cyan palette. cotoaga.net must be visually distinct from cotoaga.ai. The two brands share a foundation but not a palette.
- Any color from Kydroon's red/oxblood/bronze substrate. Layer 0 is sovereign.
- Multiple accents. cotoaga.net's brand proposition is restraint. Premium typography + single accent is the full dress code.

### Wordmark

The cotoaga.net wordmark uses a dark green dot (complementary to cotoaga.ai's cyan dot). The wordmark pair is the visual bridge between the two brands — same typographic structure, different color on the single punctuation mark. One wordmark family, two registers.

---

## Edge Treatment

`border-radius: 0` default for UI surfaces. Sharp = decisive. cotoaga.net's authority comes from clarity, not from softening. Rounded corners would signal friendliness; the brand signals *correctness*. Friendliness is Be-Part-Of's register, not cotoaga.net's.

**Exception for product marks:** The AI Workshop Sigil uses a scalloped sun-disc container, which is its own meaningful geometry (radiant rather than soft). The edge treatment rule applies to UI cards and components, not to symbol containers. See AI Workshops brand chapter.

---

## Primary Output

**Print + PDF + proposals + web.** cotoaga.net is no longer print-only. The brand is now fully present on its own digital surface.

---
---

# Brand: KHAOS

**Register:** Vision — Boundary intelligence, complexity navigation
**Temperature:** Cold-martial
**Edge Treatment:** `border-radius: 0` — Weapons-grade. No softness.
**Font Pipeline:** Self-hosted / desktop applications (Illustrator → PDF, bitmap). Body stack uses cotoaga.ai distributable fonts for web rendering when KHAOS artifacts cross into digital surfaces.
**Symbol:** Eight-pointed Chaos Star (Elric lineage) — navigation through chaos
**Sibling symbol:** KHAOS-Pluto — the friendly watchdog overseeing the KHAOS and cotoaga.x gems (see Symbol Family in Layer 3)
**Wordmark:** **Aviano Black** (primary mark) + **Aviano Sans** (subtitle in tracked small-caps). Both weights from the same Aviano family.

---

## Typography

| Role | Font | Character | Pipeline |
|------|------|-----------|----------|
| `--font-display` | Aviano Sans / Aviano Black | Art Deco geometry, temporal depth, structured confidence | insigne Design license / desktop |
| `--font-primary` | Space Grotesk | Bridges to cotoaga.ai DNA, warm-technical personality | Google Fonts (web) / desktop |
| `--font-mono` | JetBrains Mono | Operational system output, specs, prompts | Google Fonts (web) / desktop |

### Font Usage Rules

**Aviano Black** — the wordmark weight. Maximum gravity. Used for the primary KHAOS mark.
- The KHAOS wordmark itself is set in Aviano Black
- Document titles when maximum authority is required
- Never for body text — it is a display weapon at its heaviest

**Aviano Sans** — the subtitle and display weight. Section headers and structured headlines.
- The KHAOS subtitle ("Knowledge-Helping Artificial Optimization Specialist") is set in Aviano Sans, tracked small-caps
- Section headers, presentation headlines, document titles below the wordmark level
- All-caps with wide tracking (`0.08em`) for maximum Deco authority when appropriate
- Never for body text — it's a display face, not a workhorse
- Carries temporal depth: Art Deco geometry heading complexity-science content creates productive dissonance

**Space Grotesk** — the thinking voice. Body and running text.
- Promoted from display (cotoaga.ai) to body (KHAOS) — what's headline-grabbing in the technical world becomes everyday language in the thinking layer
- All document body text, annotations, methodology descriptions
- Weights: 400 (body), 500 (emphasis), 600 (sub-headings within Aviano-headed sections)

**JetBrains Mono** — the operational voice. Data and specifications.
- KHAOS specs, prompt structures, code, structured artifacts
- When the ecosystem outputs something executable
- Same usage as cotoaga.ai — infrastructure consistency

### Wordmark Typography

The KHAOS wordmark uses **Aviano Black** for the primary mark — KHAOS set in the heaviest Aviano weight, carrying the maximum Art Deco gravity the family allows. The subtitle ("Knowledge-Helping Artificial Optimization Specialist") is set in **Aviano Sans**, tracked in small-caps beneath. Both weights come from the same Aviano family — *one typeface family, two weights, no Didone import.* The Melniboné energy comes from Aviano Black's Art Deco gravity, not from a separate logotype face. The previous "Didone italic" claim in v1.x of this document was inherited error and has been removed.

---

## Enemy Font — Papyrus / Trajan-on-Everything

**Failure mode:** False mystique. Borrowed gravitas from ancient or exotic registers, used to imply depth the work does not actually have.

**Why KHAOS refuses it:** KHAOS operates in ancient-martial register — Chaos Star, Warrens, Houses, Deck of Dragons, Melnibonéan lineage. The temptation to reach for Papyrus or all-caps Trajan as "ancient-looking" fonts is exactly the failure KHAOS must refuse. KHAOS earns its temporal depth through Aviano (Art Deco, 1930s, genuinely aged from a real historical period). It does not cosplay ancient by using fonts that signal ancient-to-people-who-haven't-looked-closely. Papyrus is what you reach for when you want to *seem* mythological without doing the work. KHAOS does the work.

**What it teaches by counter-example:** There is a difference between *earned mystique* (Aviano's temporal depth from real Art Deco lineage) and *performed mystique* (Papyrus's pretend-ancient calligraphic gesture). The distinction is the entire KHAOS doctrine applied to type selection.

*Render a sample in Papyrus somewhere in the KHAOS documentation — perhaps as a footnote to the wordmark explanation. The contrast between Papyrus and the actual KHAOS Aviano wordmark is instructive.*

---

## Color System

### Palette

| Token | Hex | Role |
|-------|-----|------|
| `--khaos-black` | `#0B0B0B` | Primary — wordmark, body text, dominant surface |
| `--khaos-white` | `#FAFBFB` | Counter — paper, negative space, breathing room |
| `--khaos-grey` | `#4A4A4A` | Secondary text, annotations, subordinate information |
| `--khaos-grey-light` | `#8A8A8A` | Tertiary — metadata, timestamps, structural lines |
| `--khaos-accent` | TBD | Reserved — accent color not yet defined. Candidate: electric violet (Crown chakra / Dark Attractor signature from KHAOS-Planes color convergence) |

### Color Philosophy

KHAOS runs monochrome by default. Black, white, grey. The Chaos Star is black. The wordmark is black. The system trusts contrast and typography to do the work, not color.

When color enters KHAOS, it enters with *meaning* — as a signal, not decoration. The accent color, when formally defined, will carry specific semantic weight. The candidate color (electric violet) surfaced through the triple convergence discovery documented in the KHAOS-Planes development: KHAOS-Planes node colors, chakra energy topology, and Cotoaga Design System brand colors independently arrived at overlapping mappings, with violet consistently attaching to Crown / Dark Attractor position.

**Cross-brand flow:** When KHAOS produces artifacts that flow into cotoaga.ai or cotoaga.net, they adopt the receiving brand's color system. KHAOS monochrome is for the ecosystem's own documentation and thinking tools.

---

## KHAOS Component Overrides

### Document Headers
Aviano Sans, all-caps, tracked at `0.08em`, bold weight. Rule line underneath (1px, `--khaos-black`). 48px space below minimum.

### Body Sections
Space Grotesk at body weight. Line height 1.8 (relaxed) — KHAOS documents tend toward density, extra air is essential.

### Callouts / Key Insights
Black left border (4px), light grey background (`rgba(0,0,0,0.03)`), Space Grotesk italic. No color — border weight carries emphasis.

### Operational Specs
JetBrains Mono in dark code block (`--khaos-black` background, white text). Shared treatment with cotoaga.ai.

---
---

# Brand: BE-PART-OF.NET

**Register:** Connection — Community, network, relational intelligence
**Temperature:** Warm-organic
**Edge Treatment:** TBD — likely `border-radius: 4-8px` (the only brand that may break the sharp-edges rule)
**Font Pipeline:** Inherits cotoaga.ai distributable stack (Google Fonts CDN)
**Symbol:** Circumpunct — alchemical unity, everything is connected

---

## Typography

Inherits cotoaga.ai's distributable stack. The "slab serif TBD" experiment from v1.x is **retired**; Be-Part-Of will not develop its own type system. It inherits cotoaga.ai's fonts and expresses its relational identity through color, edge treatment, and imagery rather than typography.

- `--font-display`: Space Grotesk
- `--font-primary`: Inter
- `--font-mono`: JetBrains Mono

The organic/connective character of Be-Part-Of is carried by:
- Softer edge treatment (potentially rounded corners)
- Warmer palette (greens toward sage/moss, blues toward sky)
- Imagery with organic motifs
- The circumpunct symbol and its semantic weight

---

## Color System

### What We Know

| Element | Color | Notes |
|---------|-------|-------|
| Symbol: outer ring | Organic green | Warmer than cotoaga.ai green — toward sage/moss |
| Symbol: core | Blue (~#0088FF) | Signal, focus, the connected point |
| Symbol: field | White | Breathing room between boundary and center |

### Design Direction

Palette should feel warmer than any other brand. Greens toward sage/moss, blues toward sky rather than electric. Full system pending first deployment.

---

## Enemy Font — TBD

*Reserved slot. Be-Part-Of has not yet named its canonical opponent. Candidate categories: over-designed display fonts that perform "friendliness" theatrically (the warm-authoritative-overlap failure), or brutally geometric fonts that refuse human warmth entirely. To be decided in the first full design pass.*

---
---

# SPILLOVER PRODUCTS

*Brands born in the seam between two sovereign brands. Each inherits asymmetrically from both parents. The pattern is documented in Layer 3, Spillover Products section.*

The cotoaga × KHAOS ecosystem has two spillover products as of v2.1: APEX Recruiter (cotoaga.ai × Be-Part-Of) and AI Workshops (cotoaga.net × cotoaga.ai). Each is documented as a brand chapter in its own right, with the parent brands named explicitly so the inheritance is visible.

---

# Brand: APEX RECRUITER

**Register:** Process — Ethical matching as geometric craft
**Temperature:** Cool-technical + warm-relational (the intersection)
**Edge Treatment:** `border-radius: 0` — Inherits cotoaga.ai foundation.
**Font Pipeline:** Inherits cotoaga.ai (Google Fonts CDN — Space Grotesk, Inter, JetBrains Mono)
**Theme:** cotoaga.ai dark theme (deep-sky background, amber/sand accent)
**Symbol:** Triangle enclosing the Be-Part-Of circumpunct (green ring, blue core)
**Spillover Product Status:** First documented instance. Parent brands: **cotoaga.ai (dominant — typography, color, edge treatment, dark theme chassis) × Be-Part-Of (secondary — circumpunct symbol, relational matching domain).**

---

## Structural Position

APEX Recruiter is the first documented Spillover Product in the system. The triangle is cotoaga.ai's geometric chassis (sharp edges, mathematical precision, process discipline). The circumpunct inside it is Be-Part-Of's connection symbol (green outer ring, blue core, relational intelligence).

**APEX is structurally the intersection of cotoaga.ai ∩ Be-Part-Of** — geometric process wrapped around human matching. This is exactly what an ethical recruiting platform should be: rigorous process containing authentic connection. The symbol does diagnostic work that the verbal positioning would take paragraphs to establish. *The triangle holds the circumpunct; the process serves the connection; neither collapses into the other.*

### The Triangle as Triangulation Pattern

The APEX triangle is *not* an arbitrary geometric chassis. It is the **visual embodiment of the Triangulation Pattern documented in Layer 1.** Three vertices — three coordinate positions around the treacherous concept of *ethical matching*. The circumpunct interior is the meaning that forms in the structural relationship between the three vertices, not at any single one.

This connects APEX directly to the AI Workshop Sigil, which deploys the same triangulation pattern in different geometry (gold sun-disc with three GT Sectra Black claims around a centered Cognitive Sovereignty masterpiece). **Where you see a three-element symbol structure in this ecosystem, you are looking at the Triangulation Pattern made visible.** APEX and the AI Workshop Sigil are the two current visual instances. They are structurally the same move in different registers.

This is the deeper reason APEX's symbol works: the geometry is doing the positioning work. *Recruiting is treacherous; the triangle bounds it without collapsing it.*

---

## Typography

Full inheritance from cotoaga.ai. No overrides.

- `--font-display`: Space Grotesk
- `--font-primary`: Inter
- `--font-mono`: JetBrains Mono

---

## Color System

Full inheritance from cotoaga.ai **dark theme**. APEX is the canonical dark-theme reference implementation for the cotoaga.ai chassis.

| Token | Value |
|-------|-------|
| `--brand-primary` | `--cotoaga-green` (#00A86B) |
| `--brand-accent` | `--cotoaga-ai-sand` / `--apex-amber` (#EB9929) |
| `--brand-hero-color` | `--cotoaga-cyan` (#00D4FF) |
| `--brand-surface` | `--cotoaga-ai-dark-marine` (#16213E) |
| Page background | `--cotoaga-ai-deep-sky` (#191A2E) |

APEX = cotoaga.ai dark. The variable `--apex-amber` is an alias for `--cotoaga-ai-sand` to preserve legacy naming where it already exists in the codebase.

---

## Enemy Font

Inherits cotoaga.ai's Enemy Font (Computer Modern). No separate refusal needed — APEX's typographic personality is entirely cotoaga.ai.

---
---

# Brand: AI WORKSHOPS (cotoaga.net × cotoaga.ai)

**Register:** Authority applied to AI subject matter — Mittelstand-grade workshops on EU AI Act, Cognitive Sovereignty, and Ethical AI Usage
**Temperature:** Warm-authoritative (cotoaga.net inheritance) operating in cool-technical subject domain (cotoaga.ai inheritance)
**Edge Treatment:** `border-radius: 0` for UI surfaces (cotoaga.net inheritance); scalloped sun-disc for the product mark (its own meaningful geometry)
**Font Pipeline:** GT Sectra (cotoaga.net licensed stack) for headlines, sigil, and authority-register materials. Söhne for body text.
**Symbol:** AI Workshop Sigil — gold radiating sun-disc with three GT Sectra Black claims, Cognitive Sovereignty centered as the masterpiece
**Spillover Product Status:** Second documented instance. Parent brands: **cotoaga.net (dominant — typography, color, audience, register, sigil) × cotoaga.ai (secondary — subject domain, semantic field, AI as topic).**

---

## Structural Position

The AI Workshops are the place where cotoaga.net spills into cotoaga.ai. The workshops carry cotoaga.net's authority register (premium licensed type, ehrbarer Kaufmann audience, Mittelstand-grade positioning) into cotoaga.ai's subject-matter domain (artificial intelligence, regulatory navigation, ethical deployment, cognitive infrastructure).

This is the *strategic positioning advantage* of the workshops. There are many EU AI Act workshops on the market. None of them are taught with the typographic and intellectual register of cotoaga.net. The workshops are not competing with other AI workshops on content — they are competing on *who shows up to teach them*. The audience for the workshops is the same audience cotoaga.net is built for: German Mittelstand decision-makers who notice premium typography, who recognize Bonhoeffer and Cipolla, who can distinguish *Bildung* from *Ausbildung* and have private opinions about the difference.

The product line is first-of-its-kind in the system because it is the **first operational delivery vehicle for Cognitive Sovereignty as a positive claim.** Every other manifold in the KHAOS workshop diagnoses the *absence* of cognitive sovereignty (Tegel diagnoses cognitive captivity; the Cognitive Devolution paper diagnoses voluntary surrender; Milliways stages the captivity for live audiences). The AI Workshops are where the *capacity for self-defense* is taught. Diagnosis happens elsewhere; equipment happens here.

---

## The AI Workshop Sigil

### Form

A **gold radiating sun-disc** with scalloped edges, containing **three claims set in GT Sectra Black**, vertically stacked:

1. **EU AI Act Literacy** (top)
2. **Cognitive Sovereignty** (center, structural masterpiece position)
3. **Ethical AI Usage** (bottom)

The three claims are set in equal weight and equal size. No visual hierarchy among them. The center claim is positioned at the structural center of the disc; this is the *only* indication of its masterpiece status. No bolder weight, no different color, no enclosure within the disc that distinguishes it from the other two. The triangulation must be visually coordinate or the pattern breaks.

### Color

Gold/amber sun-disc on white or contrasting field. The gold may represent the first operational deployment of cotoaga.net's accent slot — the brand's color palette is preliminary and the sigil's gold is a candidate for the formal accent definition. To be confirmed in cotoaga.net's full color system pass.

### Typography

GT Sectra Black for all three claims. This is the heaviest GT Sectra weight, used for maximum gravity in a small visual footprint. Tracked normally (no inflation, no theatrical spacing). The font does the work; the spacing is restrained.

### Function

The sigil functions simultaneously as **product mark** and **audience filter**.

**As product mark:** It identifies materials, slides, web pages, and proposals as belonging to the AI Workshop product line. Wherever the gold sun-disc appears, the audience knows they are in workshop territory.

**As audience filter:** The sigil deploys the Triangulation Pattern (Layer 1) for deliberate audience self-selection. The three claims are structured as Trojan Horse delivery:
- **EU AI Act Literacy** is the first entry-vocabulary claim. It anchors the workshops in the actual regulatory text and the specific way the Act uses "literacy" without operationally defining it. Anyone who recognizes EU AI Act compliance as a real concern can open the door without feeling sold mysticism. *Legitimate entry credential for compliance officers, board members, legal teams.*
- **Ethical AI Usage** is the second entry-vocabulary claim. It anchors in the Karten-auf-den-Tisch game and the lived-experience layer of ethics-as-practice rather than ethics-as-policy. Anyone who recognizes that ethical questions live in actual decisions, not in framework documents, can engage. *Legitimate entry credential for managers, practitioners, anyone who has had to make a real call.*
- **Cognitive Sovereignty** is the masterpiece. It is the term the broader audience cannot yet process — the math is approximately 0.1 × 0.1 = 0.01 comprehension across the general population, since both "cognitive" and "sovereignty" are individually difficult and their combination is harder than either. It is therefore positioned as the central structural claim that the entry vocabulary makes approachable. The audience that arrives because of the first two claims is correctly served at the level they can receive. The audience that recognizes the third claim is the actual target — and they recognize it without prompting.

**The 0.01 comprehension rate is the design, not a bug.** A perfect filter that filters out 99% of the population works precisely because it puts the right 1% into the room. The risk is not the filter; the risk is putting the filter in front of rooms where the 1% never appear at all (see distribution rules below). When the sigil is deployed in front of Mittelstand boards, German CIO conferences, family-business-owner offices, intellectual-Mittelstand environments, the math works perfectly. When it is deployed in front of mass-market AI-hype channels, the math produces the same observable signal as a broken filter. *The sigil is correct; channel selection is what makes it operate.*

### Three Adjacent Terms, One Treacherous Concept

The three claims also do *substantive* triangulation work, not only tactical Trojan Horse work. Each term captures a facet of the actual treacherous concept: *what it means to retain cognitive autonomy in an environment increasingly shaped by AI systems*. No single term holds the meaning. The three together bound the territory.

- **EU AI Act Literacy** captures the *regulatory-institutional* facet — knowing the rules of the game well enough to operate inside them without being captured by them.
- **Ethical AI Usage** captures the *practical-individual* facet — making real decisions about what to delegate to AI systems and what to retain as human judgment, in the moment, repeatedly, with the cards visible on the table.
- **Cognitive Sovereignty** captures the *structural-philosophical* facet — the underlying claim that human thinking is a domain that must be defended, both individually and collectively, against pressures that would reduce it to machine-readable processing.

The three facets do not reduce to each other. They are coordinate. The masterpiece term names the deeper claim, but the deeper claim is not *more important* than the others — it is *less accessible*, which is why it sits at the center where the entry terms can grant approach.

---

## Typography (full chapter)

| Role | Font | Inheritance |
|------|------|-------------|
| `--font-display` | GT Sectra (Black for sigil; Bold/Black for headlines) | cotoaga.net |
| `--font-primary` | Söhne | cotoaga.net |
| `--font-mono` | Pitch | cotoaga.net |

The AI Workshops are *fully cotoaga.net typographically*. No cotoaga.ai fonts appear in workshop materials. The cotoaga.ai inheritance is *semantic* (subject matter) not *typographic* (visual identity). This is what spillover means: dominant parent provides the visible identity; secondary parent provides the operational domain.

---

## Color System

Inherits cotoaga.net's preliminary palette. Gold sun-disc as the workshop's signature color, possibly graduating into cotoaga.net's formal accent slot. Body and text colors follow cotoaga.net guidance: warm near-black on white or off-white surface.

---

## Enemy Font

Inherits cotoaga.net's Enemy Font (inflated tracked Helvetica Neue Bold) — *with extra force.* The AI workshop market is saturated with consulting-deck-style materials that use exactly the kind of theatrical Helvetica Bold the cotoaga.net brand refuses. AI workshops in particular attract the McKinsey-deck aesthetic at maximum intensity, because the underlying material is often thin and the typography is being asked to compensate. The AI Workshops' GT Sectra Black sigil is the visible refusal of this entire register. *When the room is full of tracked-Helvetica AI workshop posters, the gold GT Sectra Black sigil does not need to argue. It is the argument.*

---

## Distribution Rules

Because the sigil's Trojan Horse mechanism depends on channel selection, the AI Workshops require deliberate distribution discipline:

- **Deploy the sigil in 1%-rooms.** Mittelstand boards, German CIO conferences, family-business-owner offices, intellectual-Mittelstand environments, Bonhoeffer reading groups, Steuerberater referrals, peer-network introductions among consultants who already operate in the cotoaga.net register.
- **Do not deploy the sigil in 0%-rooms.** Generic LinkedIn feed without targeting, mass-market AI-hype channels, content-marketing distribution to "AI-curious" audiences, anywhere the recognition vocabulary is absent.
- **For broader-channel deployment, use a second-tier accessible companion claim** that lets the 99% self-select out without feeling insulted while letting the 1% recognize the deeper claim underneath. Example: *"AI workshops for people who still want to think for themselves."* This is a 50% comprehension claim that points at the 1% claim without requiring it. The accessible phrase pulls the room toward the topic; the sigil with Cognitive Sovereignty in the center identifies who in the room is actually the audience. *This is the only sanctioned pattern for using the sigil in higher-volume channels.*

---
---

# Sovereign Outposts — Explicitly Outside the Design System

The following two entities exist within the cotoaga × KHAOS ecosystem but are **deliberately outside the design system**. They are documented here so the refusal is visible and load-bearing. Neither receives tokens, components, or design guidance.

The discipline of a multi-brand design system is strengthened by having brands explicitly outside it. Otherwise the system risks becoming the totalizing framework that the KHAOS doctrine spends its professional life critiquing.

---

## The Walkthrough

**Register:** Sovereign satirical universe
**Medium:** Substack
**Typography:** DIN Condensed Black / IBM Plex Mono / Eurostile Extended Bold
**Visual style:** Midjourney-generated paranoid-technical schematics — neurotransmitter cross-sections, oscilloscope traces, exploded pill capsules revealing tiny gears, blueprints of recursive feedback loops, laboratory distillation apparatus with game-controller-shaped flasks, faint ghost outlines of slot machine reel mechanisms
**Relationship to design system:** None. Deliberate separation.

**Why separate:** The Walkthrough is a satirical book operating as a sovereign literary universe. Bringing it under the cotoaga design system would (a) contaminate the consulting brand with the satirical one in directions that damage both, and (b) kill the Trojan Horse mechanism the satire depends on. Readers of The Walkthrough should be able to absorb its structural arguments without connecting the author to a consulting practice. The separation protects the satire's ability to function as its own vehicle.

**Typographic character:** Eurostile Extended Bold is the typeface of every 20th-century imagined future that didn't arrive — *2001: A Space Odyssey*, *Alien*, *Space: 1999*, Cold War control panels, NASA training films. Literally the font of broken futures. Combined with DIN Condensed Black (German engineering voice at maximum volume) and IBM Plex Mono (the sane technical layer), The Walkthrough's typographic universe is samizdat-sci-fi-conspiracy-zine territory — Pynchon meets Adams meets a 1970s pharmacology textbook someone scribbled in.

**Rule:** Do not harmonize The Walkthrough with any cotoaga brand. If a future collaborator attempts to "align" its visual language, stop them. The separation is the feature.

---

## kydroon.com

**Register:** Unbranded sovereign — personal voice
**Medium:** WordPress.com (Twenty Twenty-Five default theme)
**Typography:** WordPress defaults — not specified, not chosen, not relevant
**Tagline:** *Kydroon's Ebon Vault of Fractured Realms*
**Relationship to design system:** None. Deliberate refusal.

**Why separate:** kydroon.com is the unmediated voice. Every other brand in the cotoaga × KHAOS ecosystem is a *mode of operation* — mediated, positioned, deliberate. Kydroon is the unmediated source. Photography, food, wine, fusion reactors, Döner manifestos, Rioja travel notes, Dubai impressions. The refusal to design kydroon.com is itself load-bearing: it proves that the rest of the design system is a *chosen discipline*, not a totalizing reflex. The man with a sophisticated multi-brand design system also runs a default-theme blog about things he loves. That contradiction is coherent, not accidental.

**Rule:** Do not redesign kydroon.com. Do not apply cotoaga fonts. Do not add a brand chapter. The WordPress Twenty Twenty-Five default is the deliberate choice. The refusal is the entry.

**Relationship to Layer 0 (The Source):** kydroon.com is the digital surface of Layer 0. The Kydroon protagonist iconography, sigil, and tagline live here. The design system acknowledges Kydroon as substrate (Layer 0) and points to kydroon.com as its surface. Neither is styled. Both are honored by non-contamination.

---

---

# LAYER 3 — CROSS-BRAND ARCHITECTURE

---

## The System Map

| | cotoaga.ai | cotoaga.net | KHAOS | Be-Part-Of | APEX (spillover) | AI Workshops (spillover) |
|---|---|---|---|---|---|---|
| **Register** | Competence | Authority | Vision | Connection | Process | Authority + AI subject |
| **Display** | Space Grotesk | GT Sectra | Aviano Black/Sans | Space Grotesk | Space Grotesk | GT Sectra (Black for sigil) |
| **Body** | Inter | Söhne | Space Grotesk | Inter | Inter | Söhne |
| **Mono** | JetBrains Mono | Pitch | JetBrains Mono | JetBrains Mono | JetBrains Mono | Pitch |
| **Edges** | Sharp | Sharp | Sharp | Soft (TBD) | Sharp | Sharp + sun-disc for sigil |
| **Temperature** | Cool | Warm | Cold | Warm | Cool + Warm | Warm + AI-cool subject |
| **Pipeline** | Google Fonts CDN | Klim + Grilli Type web (≤5k/mo) | Licensed / desktop | Google Fonts CDN | Google Fonts CDN | Klim + Grilli Type web |
| **Primary Output** | Web applications | Print + PDF + proposals + web | Documents / thinking tools | Community / platform | Web application | Workshop materials + web |
| **Symbol** | Wordmark (cyan dot) | Wordmark (green dot) | Chaos Star + KHAOS-Pluto | Circumpunct | Triangle + Circumpunct | Gold sun-disc with three claims |
| **Enemy Font** | Computer Modern | Tracked Helvetica Bold | Papyrus / Trajan-abuse | TBD | (inherits cotoaga.ai) | (inherits cotoaga.net, with force) |
| **Parents (if spillover)** | — | — | — | — | cotoaga.ai × Be-Part-Of | cotoaga.net × cotoaga.ai |

Outside the system: **The Walkthrough** (sovereign satire, DIN Condensed / Plex Mono / Eurostile) and **kydroon.com** (unbranded sovereign, WordPress default).

---

## Spillover Products / Brand Intersections

*The structural pattern by which some products are born in the seam between two brands. Documented because the cotoaga × KHAOS ecosystem currently has two such products, and the pattern is likely to recur as the workshop expands.*

### The Pattern

Not every product belongs to a single sovereign brand. Some products *emerge in the seam* between two brands, and inherit asymmetrically from both parents. These are **Spillover Products**: brands in their own right (with their own chapter, own positioning, own deployment context) but whose identity tokens come from *two* sovereign parents in known proportions.

The pattern has three required properties:

1. **One dominant parent.** Provides the visible identity — typography, color, edge treatment, audience register. The product reads as belonging to this brand at first glance.
2. **One secondary parent.** Provides the operational domain — subject matter, semantic field, the *what* the product is about. The product addresses this brand's territory but in the dominant parent's voice.
3. **The intersection is structurally meaningful.** The product is not an arbitrary combination. It exists *because* the two brands' territories overlap in a way that creates a viable product the neither parent alone could deliver.

### Why Spillover Products Are Distinct From Inheritance

A product that simply inherits from a parent brand (e.g., a sub-page of cotoaga.ai using cotoaga.ai's tokens) is not a spillover product. It is a *child*, not an *intersection*. Spillover Products are distinct because they belong to **two** brand chapters at once, and the documentation must make both inheritance lines visible.

This matters for governance. If a spillover product changes, both parent brand chapters may need to be aware. If a parent brand changes, the spillover product may need to update accordingly. Single-parent inheritance is one-directional; spillover-product governance is bidirectional.

### Currently Documented Spillover Products

| Product | Dominant Parent | Secondary Parent | Intersection Logic |
|---|---|---|---|
| **APEX Recruiter** | cotoaga.ai (typography, color, dark theme chassis) | Be-Part-Of (circumpunct symbol, relational matching domain) | Geometric process containing authentic connection. Triangle holds circumpunct. |
| **AI Workshops** | cotoaga.net (typography, color, audience, register) | cotoaga.ai (subject domain, semantic field) | Authority register applied to AI subject matter. The Mittelstand-grade workshop on the technology of cognition. |

### Rules for Spillover Products

- **Document both parents explicitly.** The brand chapter must name dominant and secondary parent in the header. This makes the inheritance visible at a glance.
- **Use the dominant parent's tokens by default.** Typography, color, edge treatment, component patterns all inherit from the dominant parent unless the spillover product has a specific reason to override.
- **Use the secondary parent for semantic context, not visual identity.** The secondary parent contributes the *what* (subject matter, domain, audience use case), not the *how* (visual identity). Mixing visual tokens from both parents typically breaks coherence.
- **Allow product-mark exceptions to the dominant parent's edge rules.** A spillover product's symbol (APEX triangle, AI Workshops sun-disc) may have its own geometry that the parent's edge treatment does not govern. The edge treatment rule applies to UI surfaces, not to symbol containers.
- **Document the intersection logic.** Every spillover product chapter must explain *why* the intersection exists — what the product does that neither parent alone could do. Without this, the spillover risks becoming a clever taxonomic fiction rather than a real positioning advantage.

### When Not to Use the Spillover Pattern

The spillover pattern is for products that **structurally require both parents** to make sense. A product that could have been built under one parent alone, but happens to mention the other, is not a spillover product. It is the dominant parent's product, full stop. Misusing the spillover label dilutes the pattern.

Test: *Could I delete the secondary parent from this product's identity and still have a coherent product?* If yes, it is not a spillover; demote it to single-parent inheritance. If no, the spillover label is correct.

---

## Font Promotion Logic

Space Grotesk appears in multiple brands in different roles:

- **cotoaga.ai:** Display (headlines) — commands attention
- **KHAOS:** Primary (body) — everyday working language
- **APEX, Be-Part-Of:** Display (inheriting cotoaga.ai)

The promotion signals that what's headline-grabbing in the technical world becomes everyday language in the thinking layer. KHAOS metabolizes what cotoaga.ai announces.

GT Sectra appears in two contexts:
- **cotoaga.net:** Display (headlines, body authority)
- **AI Workshops:** Sigil (Black weight, three-claim triangulation)

The AI Workshops use is *not* a separate brand registration. It is cotoaga.net's GT Sectra deployed for a spillover product; the license, the typeface, and the brand association all stay with cotoaga.net. This is what makes the AI Workshops a spillover product rather than an independent brand: the typography stays in the parent's stack.

JetBrains Mono appears in five brands as mono — infrastructure, like the 8px grid. cotoaga.net and AI Workshops are the two contexts with a different mono (Pitch) because cotoaga.net's whole proposition is premium/licensed typography down to the mono slot.

No other typeface appears in the same role across brands.

---

## The Symbol Family

Six symbol lineages exist in the cotoaga × KHAOS ecosystem. Each carries load the typography alone cannot.

### 1. The Wordmark Pair — COTOAGA·NET / COTOAGA·AI

Two wordmarks in the same typographic structure with different colors on the single punctuation mark. cotoaga.net uses green; cotoaga.ai uses cyan. The wordmark pair is the *visual bridge* between the two brands — same family, two registers. When both wordmarks appear together, they function as a compound umbrella signal: *one consultancy, two modes*.

### 2. The KHAOS Chaos Star

Eight-pointed radial symmetry. Direct Moorcock/Elric lineage — the Chaos Star of Law and Chaos from the Eternal Champion cycle. Black on white (or white on black). Navigation through chaos. The wordmark beneath the symbol is set in **Aviano Black**; the subtitle ("Knowledge-Helping Artificial Optimization Specialist") is set in **Aviano Sans**, tracked small-caps.

### 3. KHAOS-Pluto (the friendly watchdog)

Sibling symbol to the Chaos Star. An eight-pointed chaos star with an eye/circumpunct fusion at the center — green outer ring, blue core, nested inside the KHAOS geometry. **Pluto's function is friendly oversight of the entire KHAOS and cotoaga.x gem ecosystem.** The name honors three resonances simultaneously:

- **Disney Pluto** — the watchdog, faithful companion, friendly face of vigilance
- **Pluto the planet** — the demoted outpost, the distant sentinel, the thing at the edge that astronomers couldn't decide whether to keep in the family
- **Pluto the god** — the underworld sovereign, god of the dead and of wealth (because gold comes from underground), the diagnostic excavator

The Disney face is the permitted softening of the actual Pluto. Both are true. Neither cancels the other. This is the same Trojan Horse pattern that governs the rest of the ecosystem: present the friendly face while carrying the deeper weight underneath.

Pluto does not own a product or a surface. Pluto is the overseer — the watchdog symbol for the entire gem collection. It belongs in the symbol family as the meta-symbol, the one that watches the others.

### 4. The APEX Triangle with Circumpunct Interior

Triangle = cotoaga.ai dark chassis (sharp edges, mathematical precision). Circumpunct interior = Be-Part-Of connection logic (green ring, blue core). The symbol documents structural intersection.

**Triangulation Pattern instance.** The three vertices are coordinate positions around the treacherous concept of *ethical matching*; the circumpunct interior is the meaning that forms in their structural relationship. See APEX Recruiter brand chapter and Layer 1 Triangulation Pattern.

### 5. The Be-Part-Of Circumpunct

Alchemical unity symbol. Outer green ring (organic), blue core (signal/focus), white field (breathing room). Sovereign to Be-Part-Of but also the nested element inside APEX's triangle.

### 6. The AI Workshop Sigil

Gold radiating sun-disc with three GT Sectra Black claims arranged vertically: **EU AI Act Literacy / Cognitive Sovereignty / Ethical AI Usage**. Cognitive Sovereignty positioned at the structural center as the masterpiece term. See AI Workshops brand chapter for full documentation.

**Triangulation Pattern instance.** The sigil deploys the three-adjacent-term strategy in the visual register: three coordinate claims, one centered masterpiece, no visual hierarchy among the three terms. The sun-disc geometry is a different visual encoding of the same triangulation pattern that the APEX triangle deploys — same move, different shape.

The sigil is the operational counterpart to the diagnostic instruments documented in Layer 4. Where Tegel/Fondaco/Antenna *diagnose* cognitive captivity, the AI Workshops *equip* individuals to defend cognitive sovereignty. The sigil marks materials produced by that defense-equipping function.

### Symbols Are Sovereign

A structural observation worth naming: **in the cotoaga × KHAOS ecosystem, symbols carry the cosmology and typography is the vehicle for delivering it.** This inverts the usual design hierarchy where logos are "typography crystallized." Here, the symbol system is primary and the typographic system serves it.

The Kydroon sigil (scorpion with DNA-helix spine, crowned S, red orbital geometry) is *not* in this family. It belongs to Layer 0 and never appears on cotoaga surfaces. The cotoaga symbol family is what the protagonist uses in the upper world; the Kydroon sigil is the protagonist's personal coat of arms at Layer 0.

---

## Artifact Flow Rules

When artifacts cross brand boundaries, they adopt the receiving brand's identity:

- KHAOS thinking → cotoaga.ai web deliverable = cotoaga.ai tokens
- KHAOS thinking → cotoaga.net print proposal = cotoaga.net tokens
- KHAOS thinking → AI Workshop materials = cotoaga.net tokens (because AI Workshops inherit dominant parent identity)
- cotoaga.ai analysis → Be-Part-Of community content = Be-Part-Of tokens
- APEX recruiter output → cotoaga.net proposal = cotoaga.net tokens

**Origin brand shapes *content structure*. Destination brand shapes *visual identity*. Dress code is set by the far shore.**

This is the Malinche principle applied to design system governance: the translator respects the ontology of the receiver. A document's content structure reflects where it was thought; its visual identity reflects where it is being read.

**Sovereign outposts do not participate in artifact flow.** Artifacts do not flow out of The Walkthrough into cotoaga brands, and cotoaga content is not adapted into The Walkthrough visual language. kydroon.com content stays on kydroon.com. The separation is absolute.

**Spillover products participate in artifact flow as their dominant parent.** APEX artifacts flow as cotoaga.ai. AI Workshop artifacts flow as cotoaga.net. The secondary parent's contribution is structural (the *intersection logic*), not stylistic.

---

## What NOT to Do

- Never mix brand tokens in a single view
- Never use cotoaga.ai green/blue/cyan in KHAOS, cotoaga.net, AI Workshops, or Kydroon contexts
- Never use dark theme tokens in brands without a defined dark theme
- Never exceed the cotoaga.net web license traffic tier (≤5k/month) without upgrading the tier first
- Never substitute one brand's type role for another's
- Never apply cotoaga tokens to kydroon.com or The Walkthrough
- Never use Kydroon's red/oxblood/bronze palette in any cotoaga brand
- Never use the scorpion, DNA-helix, or crowned-S sigil outside Kydroon/Layer 0
- Never decorate cotoaga materials with Kydroon imagery "to add depth"
- Never harmonize The Walkthrough's visual language with cotoaga brands
- Never render an Enemy Font reference without making the contrast visible
- Never escalate the Triangulation Pattern to four terms or collapse it to two — three is the form
- Never explain the Triangulation Pattern to the audience receiving it — the wooden horse must remain unlabeled
- Never deploy the AI Workshop Sigil in 0%-rooms (mass-market AI-hype channels, generic untargeted feeds)
- Never use a spillover product's tokens to mean its secondary parent — only the dominant parent's identity is visible

---

---

---

---

## What This Seed Does NOT Cover

This seed governs **visual identity, tokens, components, and brand discipline** for the cotoaga × KHAOS ecosystem. It is deliberately silent on:

- **Cosmological doctrine** — *why* the brands exist, what they defend, the multiverse physics, Cognitive Sovereignty as the central claim. → See `seed-multiverse.md`.
- **Kydroon / Layer 0 substrate** — the protagonist mythology beneath every brand. → See `seed-multiverse.md` Layer 0.
- **Technical integration patterns** — Supabase shapes, health endpoints, deployment infrastructure. → See `seed-supabase.md`, `seed-pluto.md`, `seed-health-endpoint.md`.
- **kydroon.com styling** — the unbranded sovereign, WordPress Twenty Twenty-Five by deliberate choice. Acknowledged in Layer 2 as Sovereign Outpost, not styled.
- **The Walkthrough styling** — sovereign satirical universe with its own typographic vocabulary. Acknowledged in Layer 2, not absorbed.
- **Content strategy, copy voice, editorial guidelines** — register guidance lives per-brand in Layer 2 but tone-of-voice writing rules are out of scope. The seed says *what fonts and colors a brand uses*, not *what sentences it produces*.
- **Marketing campaigns, asset pipelines, photography direction** — out of scope.
- **Font licensing procurement** — the seed names the fonts; legal acquisition for licensed faces (Aviano, Söhne, GT Sectra, Pitch, etc.) is the operator's responsibility per brand chapter.

---

## Open Structural Questions (Style)

These questions do not block use of the seed. They mark places where future refinement will sharpen the reference.

1. **KHAOS accent color.** Reserved slot `--khaos-accent` currently TBD. Candidate: electric violet (from the triple color convergence in KHAOS-Planes — see `seed-multiverse.md` Layer 4, Section VI). Formalize or remove the slot.

2. **Be-Part-Of Enemy Font.** Reserved slot pending first full design pass.

3. **cotoaga.net full color palette.** Preliminary guidance only. Open question: is the AI Workshop Sigil's gold the formal accent color for cotoaga.net? Decision pending full color system pass.

4. **AI Workshop Sigil — three vertices labeled?** The current documentation describes the APEX triangle's three vertices with placeholder labels (positioned process / matched candidate / honest engagement). The operator should confirm whether these are the actual three coordinate positions APEX is triangulating, or whether the labels need revision.

*Doctrine-side open questions (AI Workshops elevation, The Walkthrough's multiverse status) live in `seed-multiverse.md`.*

---

## Versioning Rules

- **Pin per project.** Projects record `Style Seed: v2.1` (or current) in their repo `CLAUDE.md` at project init.
- **Forward-only evolution.** New versions ship; old projects keep their pinned version until they deliberately upgrade.
- **Upgrade is an ADR.** Bumping a project's pinned Style Seed version is a deliberate decision documented in `docs/adr/`, not a passive sync.
- **Companion Seed coupling is loose.** `seed-style.md` and `seed-multiverse.md` evolve independently. Either can bump without forcing motion in the other. Cross-references resolve by name, not by version pinning.

---

*One foundation, four voices. Structure is shared. Identity is sovereign.*
*The bridge carries traffic both ways, but dress code is set by the far shore.*

**Closing line:** *Clean elegance, not heavy decoration. Space, light, and subtle interactions create the premium feel. Sharp edges create the mathematical identity. The dark scale adds depth without void.*

**— End of Style Seed v2.1 —**
