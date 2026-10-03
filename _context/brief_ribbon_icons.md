# Brief for Claude design — Ribbon icons

Read `_context/design.md` first. This document only adds the semantic context and delivery expectations that don't fit inside a formatting spec.

## Scope

Design **ribbon command icons only**. Everything else is done: workbook chrome, sheet tabs, ribbon-tab PNGs (File, Home), backgrounds. Do not touch them.

## What to deliver

| Group | Icons |
|---|---|
| **View** (binary toggle) | `story`, `list` — designed as an obviously opposed pair |
| **Detail Level** (binary toggle) | `compact`, `standard` — designed as an obviously opposed pair; may be text-only if a compelling icon idea doesn't emerge |
| **Perspective** (3 peer commands, icon-above-label) | `timeline`, `skills`, `industries` |
| **Filters** | Not needed — the down-chevron and clear-filter icon are Fluent stock (see design.md §15) |
| **QAT** (optional, if capacity) | `new`, `save`, `autosave_on`, `autosave_off` (see design.md §9) |

## Semantic context — the modes and views

The three interactive groups are **conceptually different** and the icons must reinforce that so the user's mental model stays clean.

### View — same content, different presentation

Story vs List is one **text-treatment toggle**. Same items, same order, same filters — only prose vs bullets. Not two features.

Design as a segmented pair with a clear A↔B opposition that reads at 16 px:
- Story — one flowing paragraph line (article/running text)
- List — three stacked short lines with bullets/dashes

### Detail Level — same content, different density

Not a filter, not a persona switch. Detail Level swaps how much of each item is shown across every applicable sheet — Compact truncates to headline/hero content only, Standard shows the full CV as designed. Story vs List still applies inside either density (Story-Compact = fewer sentences per item; List-Standard = more bullets per item).

Design as a segmented pair with a clear A↔B opposition that reads at 16 px:
- Compact — condensed / few-lines metaphor (short list, or "collapsed" block, or a small dense card)
- Standard — expanded / full-page metaphor (longer list, or "expanded" block, or a full card)

Watch-out: do not reuse the Story ↔ List visual grammar (one paragraph line vs three stacked bulleted lines) — Detail Level is orthogonal to View and its icons must not read as another prose-vs-bullets toggle. If a clean, distinct icon idea doesn't emerge, a text-only segmented control is acceptable (design.md §13).

### Perspective — same content, different grouping

Perspective changes **grouping and sort order only** — not text mode, not filters. "Story mode + Timeline" is still Story mode; content is just chronologically grouped.

- Timeline — chronology (calendar with a line, or dotted timeline)
- Skills — capability (spark, star with facets, or badge)
- Industries — sector/domain (small cluster of buildings, or factory)

## Delivery format

- **Monoline SVG**, Fluent 2 style, one file per icon.
- 20 × 20 viewBox, safe drawing area ~16 × 16, pixel-hinted where possible. Must read cleanly at 16 px as well as 20 px.
- Single-colour geometry using `currentColor` on strokes (no baked fill colour). The rendering layer swaps to Excel green on selected state — do not bake state colours in.
- Deliver **base geometry only**. Hover / pressed / selected variants are produced at the visual layer, not as separate SVGs.
- Consistent stroke weight, corner radii, and negative-space rhythm across the whole set — the group must look like one family.
- Filenames snake_case, one per icon: `ico_view_story.svg`, `ico_detail_compact.svg`, `ico_perspective_timeline.svg`, `ico_qat_save.svg`, etc.

## Watch-outs (from design.md, worth restating because they bite icon design specifically)

- No legacy glossy or multi-colour Office icons; no isometric; no emoji; no photos.
- No decorative colour per audience/perspective/mode — semantic colour use only.
- No text-in-icon, no numerals-in-icon.
- Palette is fixed in design.md §6 — do not propose new colours.

## Non-goals

- File overlay iconography (deferred to a later brief).
- Sheet-tab icons — already delivered as PNGs.
- Any icon that carries live data (KPIs, charts) — those are visuals, not icons.
