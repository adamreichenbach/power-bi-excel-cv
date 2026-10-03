# CLAUDE.md — Interactive Power BI CV

Project instructions for a coding agent working on this repository. Read `_context/context.md` and
`_context/design.md` (§8 and §6) before making implementation decisions.

## 1. What this project is

An **interactive, single-page Power BI CV** built in **PBIP** format that visually mimics a modern Microsoft Excel workbook while functioning as a genuine Power BI semantic-model and interaction-design showcase.

- Canvas: **2072 × 1020 px**, one principal report page. *(Widened 1920 -> 2072 on 2026-09-21: measured from the publish-to-web full-screen render on a 1920 x 1080 screen, where the report was height-bound at scale 0.926 and left a 71 px white stripe each side. 1920 squared / 1778 = 2073.4; 2072 is the 4 px-grid value. The extra 152 px is inserted at the RIGHT -- left chrome is untouched, right chrome and the column-letter row shift +152.)*
- Source of truth for content: `_input/CV.xlsx` (single worksheet `cv_input`, one structured Excel table per entity). **In the public repo it is a dummy** (since 2026-09-30): identical tables, columns, rows and IDs, but placeholder and invented text and random scores — see `README.md` → *The data*. The author's real workbook is `_private/CV_real.xlsx`, which the local model reads; use it for row content, either file for table and column names.
- Source of truth for visuals/behaviour: `_context/design.md` and `_context/context.md`.
- The PBIP lives under `_report/` (model + report, all seven sheets built). `dim_calendar` is the only calendar: month grain, spanning the first role/project month to `latest_publish_month`. The user's original version was replaced by an agent-built one on 2026-10-03 (user), so no part of the model is hand-made.

### 1.1 Agentic-BI-only build (project-level non-negotiable)

This project is **also a proof-of-concept that a complex Power BI dashboard can be built end-to-end by an agent — with zero manual edits to data or visuals in Power BI Desktop**. The user's role is to:

- Open Desktop to view, refresh, and verify what the agent has produced.
- Supply a reference file when the agent needs a JSON shape it has not seen — a separate sample PBIP
  or an exported example, never an object authored inside THIS project in the Desktop UI.
- Approve, redirect, and correct.

**All model and visual changes go through TMDL / PBIR JSON on disk (or MCP/TOM edits) — never via the Desktop authoring UI.** This holds even for edits that would be trivial by hand: moving a shape one pixel, changing a fill colour, renaming a field. The point is precisely to prove those "just do it in Desktop" cases can be automated. If the agent proposes "you can quickly fix this in Desktop", that's a discipline break — do it programmatically instead.

Implication: expect more Desktop reload cycles than a Desktop-native build would need. That's the cost of the proof; accept it.

## 2. Files to read

1. `_context/context.md` — full functional spec (semantic model, interactions, ribbon, detail panel, overlay)
2. `_context/design.md` **§8 and §6** — canvas geometry and colour tokens

**Read on demand:**

- `_context/visual_reference.md` — per-visual catalogue (§4). Grep for the object's row when touching any visual, bookmark, or interaction
- `_context/design.md` §9-§22, §20A, §20B — Fluent 2 control specs and as-built sheet geometry, when authoring new chrome or a sheet layout
- `_input/CV.xlsx` — the only source for table names, column names, and row content. Read it when a task needs any of those, at that moment rather than upfront, so you get the current workbook and not a mid-edit snapshot; there is no inventory file standing in for it
- `_input/_images/` — split by provenance (2026-10-02): `excel_derived/` (`background/`, `grid_header/`, `tabs/`, `overlay/overlay_background.png`; artwork based on the Excel interface, excluded from the MIT licence) and `original/` (`icons/`, `logo/`, `overlay/`, `pictures/`; the portrait source lives in `_private/portrait/`). New artwork goes in the matching half. Use placeholders only for assets genuinely still missing

## 3. Non-negotiable rules

These are drawn verbatim from the handoff prompt and `_context/context.md` §2. Do not relax without explicit user approval.

- Use **snake_case** for every technical name (tables, columns, measures, variables, display folders, bookmarks, visual/group/shape/image object names).
- Use the **exact** source table and column names from `_input/CV.xlsx`. Do not rename.
- `dim_calendar` is the model's only calendar (agent-built since 2026-10-03) — **extend it rather than adding another**.
- Roles/projects carry `from_month_code` and `to_month_code` as 6-digit `YYYYMM` integers.
- `999912` = open-ended end month; `9999` = open-ended end year on year-grain tables (`tbl_additional`). Neither must **ever** be shown to the user — render as blank / "current" / "present".
- Date-range logic must live in **DAX**, not in an active range relationship on `dim_calendar`.
- Skill and tool scores use a shared **1-7** scale. **Blanks stay blank** — do not invent, infer, or default.
- No quantitative bar charts.
- **No invented, estimated or inferred durations.** A duration may be shown only where every input is stated in the source and the arithmetic is exact — e.g. counting the `dim_calendar` months covered by a tool's linked roles and projects, using their own `from_month_code` / `to_month_code`. Nothing may be assumed about intensity, overlap, or unstated periods. Where a derived duration is displayed, render it beside its evidence denominator (`x roles · n projects`), never as a bare "N years of X" claim. *(Rule relaxed 2026-08-02 with user approval — the original blanket ban was written against a different, estimate-based proposal.)*
- No bookmark combination matrix for sheet × ribbon states. **Sheet bookmarks capture sheet content visibility only**; ribbon state, view mode, detail level, perspective, filters, and the File overlay are **outside** bookmark scope.
- **Perspective must not be bookmark-driven** — it runs on `tbl_perspectives` and the two Power Query perspective tables (§6).
- There is one navigable overlay (**File**), plus one dismiss-only first-load dialog (`*_dialog_*`, approved by the user 2026-09-24). No other overlays.
- **Story/List mode swaps text only** — it does not regroup content and it does not change filters.
- Preserve original CV wording. Retain every `<TODO: VERIFY!>` marker verbatim until the user approves removal.
- All image assets are supplied. Should one go missing → use plain rectangles/circles as placeholders, named clearly (`shp_placeholder_*`, `img_placeholder_*`) so they are trivial to swap.
- **Every visual gets a meaningful `snake_case` name in the Selection pane.** Never leave a default `Rectangle 42` / `Button 3` / `Image 12` — the name must say what the object is (`btn_sheet_experience`, `img_kpi_headline_bg`, `shp_formula_bar_divider`).
- Excel World Championship 2024 achievement must be prominent in Executive Summary and at the top of Additional.
- Contact details come only from `tbl_contact_methods`. The phone is the string `#REF!` in both workbooks, a deliberate joke; e-mail and LinkedIn are real in the author's workbook and bracketed placeholders in the public dummy.
- Do not draft Power Query or DAX inside documentation — implement it directly in the PBIP.

## 4. Report architecture (summary — see `_context/context.md` §4-§8 for detail)

Vertical zones on the 2072 × 1020 canvas, measured from `background.png` on 2026-10-03 (the earlier 30 / 30 / 112 / 36 / 107 chrome heights were planning figures from the original 1536 × 816 layout and never matched the artwork):

| Zone | Y | H |
|---|---:|---:|
| Title / Quick Access Toolbar | 0 | 60 |
| Ribbon tab row (File / Home) | 60 | 38 |
| Home ribbon | 98 | 124 |
| Formula bar | 222 | 64 |
| Column-letter band | 286 | 29 |
| **Main content (the white grid)** | **315** | **638** |
| Sheet tabs | 953 | 41 |
| Status bar | 994 | 26 |

**Main content is `x 35, y 315, 2037 × 638`** — measured from `background.png`, re-measured on
2026-09-21 after the widening (2035 × 636 by a strict pure-white test; the 2 px is the border
antialiasing, and the established 35/315/638 convention is kept). Not
notional. It is the white, gridline-free rectangle the artwork paints, and it is the only region
sheet content may occupy. The earlier `208 / 764` figure was a planning estimate that ignored the
formula-bar visual (238–298) and the column-header band below it.

Standard content padding inside that box is **24 px on all four sides**, giving a usable inner box
of `x 59, y 339, 1989 × 590` (right edge 2048, bottom edge 929).

**Sheet bookmarks** (seven, sheet-content visibility only; Skills added 2026-09-25 with user approval):

- `bm_sheet_exec_summary`
- `bm_sheet_experience`
- `bm_sheet_projects`
- `bm_sheet_tech_stack`
- `bm_sheet_skills`
- `bm_sheet_education`
- `bm_sheet_additional`

**Ribbon groups**: View (Story / List) · Detail Level (Compact / Standard) · Perspective (Timeline / Skills / Industries) · Filters (Function / Industry / Skill / Tool) · Other (Clear Filters · Help). Per-tab availability is greyed-out where a group does not apply; see `_context/context.md` §5.4 for the matrix. Every group greys on **Additional**.

**Ribbon selector**: File (clickable, opens overlay) · Home (selected default, not clickable).

**File overlay menu**: About · Contact · Help, plus Special Thanks bottom-anchored in the rail. Close via the left-pointing arrow, or the Recent row on About, without resetting current state.

**Formula bar**: Name box (a bare cell reference, e.g. `A1` — never the sheet name, `context.md` §6) + `fx` icon + read-only DAX-driven context text.

**Detail panel** (Experience and Projects): selecting a role or project row fills a panel right of the matrix: title, period, industry / type (/ location) line, radar of linked Technical and Soft skills on the 1-7 relevance scale (blanks not plotted), up to five tools, one highlight line, a scale footnote.

## 5. Naming conventions

Object-name prefixes in the Selection pane:

- `grp_` groups
- `shp_` shapes
- `btn_` buttons
- `sli_` slicers
- `txt_` text boxes
- `vis_` visuals
- `img_` images
- `bm_` bookmarks

Every object — placeholder or final — must be named clearly. Placeholders (`shp_placeholder_*`, `img_placeholder_*`) get names that reveal what the final asset will be, so swap-in is trivial. Real visuals get names that say what they are (`btn_ribbon_file`, `sli_filter_function`, `vis_formula_bar`). Selection-pane defaults (`Rectangle 42`, `Button 3`) are never acceptable — see §3.

## 6. Semantic model (see `_context/context.md` §9)

Dimensions, entities, content, bridges, derived tables and interaction/config tables are enumerated there; `_input/CV.xlsx` is authoritative for the actual table and column names. Key expectations:

- One-to-many from dimensions/entities into children and bridges.
- No role-to-project relationship: `parent_role_id` was dropped, because a project can span two roles.
- `tbl_view_modes`, `tbl_detail_levels` and `tbl_sheet_tabs` are **disconnected**. `tbl_perspectives` relates to the two Power Query perspective tables (`tbl_role_perspective_rows`, `tbl_project_perspective_rows`) that the Experience and Projects matrices group on. `tbl_viewer_types` and `tbl_audience_priority` are deleted; nothing may reference them.
- Numeric integer IDs for roles, projects and bridge keys.

## 7. Design tokens (see `_context/design.md`)

- Typography: Segoe UI (fallback: Segoe UI Variable, Arial, sans-serif).
- Excel accent: `#107C41` primary, `#185C37` dark, `#E9F5EE` tint (full set in `design.md` §6.4). `#217346` is the legacy green — do not use it as the default.
- Neutrals: `#FFFFFF`, `#FAFAFA`, `#F5F5F5`, `#F0F0F0`; strokes `#D1D1D1`, `#E0E0E0`, `#E5E5E5`.
- Text: `#242424` primary, `#616161` secondary, `#BDBDBD` disabled.
- Worksheet table style: `#0F9ED5` header, `#CFECF7` band.
- Grid: 4 px unit, 24 px content padding inside the main grid, 1 px separators.

## 8. Validation checklist (run before declaring the build "done")

From the handoff prompt §"Model validation":

1. All numeric IDs and relationships wired correctly.
2. Every source table loaded.
3. All seven sheet tabs work.
4. Ribbon state persists across sheet changes.
5. File overlay opens from every sheet and closes without resetting state.
6. Story/List swaps content correctly.
7. Detail Level (Compact / Standard) swaps content density without changing text mode, grouping, or filters. Compact is a source-driven selection of `detail_level` content, never invented and never a different set of items.
8. Perspective groups/orders correctly and is not bookmark-driven.
9. All four filters affect applicable roles and projects.
10. Neither `999912` nor an open-ended `9999` `to_year` appears in user-facing visuals.
11. Excel World Championship achievement prominent in Executive Summary and Additional.
12. Detail panel is compact, no invented radar scores.
13. `<TODO: VERIFY!>` markers still identifiable.
14. Missing image assets use shapes rather than broken references.
