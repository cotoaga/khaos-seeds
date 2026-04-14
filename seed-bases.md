# 🌱 SEED: Obsidian Bases Pattern

**Version:** 1.0
**Source Truth:** Extracted from live debugging session on `Obsidian 2026` vault (Obsidian v1.12.4)
**Purpose:** When Claude Code works with `.base` files in any Obsidian vault, it follows THIS pattern. No guessing, no generic assumptions — this is what actually works.

---

## The One Rule That Matters Most

**`filters:` is plural. Always.**

`filter:` (singular) is silently ignored by Obsidian Bases. No error, no warning — just all vault files showing up in the view. This has burned us once. It won't happen again.

---

## .base File Format

Obsidian Bases files are YAML. Obsidian may auto-convert older JSON formats to YAML — always work in YAML.

### Minimal working example

```yaml
from: '"Folder/Subfolder"'
views:
  - type: table
    name: My View
    filters:
      and:
        - file.inFolder(this.file.folder)
        - file.ext == "md"
        - file.path != this.file.path
    properties:
      - property_one
      - property_two
```

### Full structure

```yaml
from: '"Folder/Path"'          # folder scope hint — but NOT reliable on its own (see below)
sort:                           # top-level default sort
  - property: property_name
    direction: asc              # asc | desc
views:
  - type: table                 # table is the main type
    name: View Name
    id: view-id                 # kebab-case, used for linking
    sort: []                    # per-view sort overrides top-level
    filters:                    # PLURAL — singular is silently ignored
      and:
        - formula_condition
        - another_condition
    properties:                 # columns to display
      - property_name
```

---

## Filter Syntax

Filters are **formula strings in a list** — NOT property/operator/value YAML objects.

### Correct ✅

```yaml
filters:
  and:
    - file.inFolder(this.file.folder)
    - active == true
    - file.ext == "md"
```

### Wrong — silently ignored ❌

```yaml
filter:
  and:
    - property: active
      operator: is
      value: true
```

---

## Folder Scoping

The `from:` field is unreliable for actually filtering results (observed in v1.12.4 — may be fixed in future versions). **Always add explicit folder filters** to each view.

### Scope to the .base file's own folder

```yaml
- file.inFolder(this.file.folder)   # includes subfolders
```

or for exact folder match (no subfolders):

```yaml
- file.folder == this.file.folder
```

### Exclude the .base file itself

```yaml
- file.path != this.file.path
```

### Scope to a specific named folder

```yaml
- file.inFolder("Folder/Subfolder")
```

### Standard folder-scoped view boilerplate

```yaml
filters:
  and:
    - file.inFolder(this.file.folder)
    - file.ext == "md"
    - file.path != this.file.path
```

Add this to every view in a folder-scoped `.base` file.

---

## Common Filter Formulas

| Goal | Formula |
|------|---------|
| Files in same folder (incl. subfolders) | `file.inFolder(this.file.folder)` |
| Files in exact folder only | `file.folder == this.file.folder` |
| Exclude the .base file itself | `file.path != this.file.path` |
| Markdown files only | `file.ext == "md"` |
| Property equals value | `active == true` |
| Property equals string | `status == "Active"` |
| Property not empty | `tool_name != ""` |
| Files linking to this base | `file.hasLink(this)` |
| Property contains string | `tags.contains("keyword")` |

---

## Property Discrimination Pattern

When a `.base` file shows all vault files despite folder filters, add a discriminating property to the target notes:

```yaml
# In each target note's frontmatter:
db_source: my-database-name
```

Then filter on it:

```yaml
- db_source == "my-database-name"
```

This is a workaround for broken `from:` scoping — use it as a fallback, not as the primary approach.

---

## Checklist: Creating a New .base File

- [ ] Use `filters:` (plural) — never `filter:`
- [ ] Add `file.inFolder(this.file.folder)` to every view that should be folder-scoped
- [ ] Add `file.path != this.file.path` to exclude the .base file from results
- [ ] Add `file.ext == "md"` if the folder might contain non-markdown files
- [ ] Test: does it show only the target notes, or all vault files?
- [ ] If still showing all files: add a `db_source` discriminator property to target notes

---

## Known Quirks (v1.12.4)

- `from:` folder scoping is unreliable — Obsidian may rewrite it but still show all files
- Obsidian auto-rewrites `.base` files — don't fight the format it reverts to
- The YAML format `'"Folder/Path"'` (single-quoted string containing double-quoted path) is what Obsidian writes; this is correct YAML for passing a DQL-style quoted string
- `filter:` singular produces zero errors and zero results filtering — a silent failure

---

*The seed is planted. filters: is plural. Always was, always will be.*
