# Interactive Power BI CV — Context and Implementation Specification

## 1. Project objective

Build a single-page, interactive Power BI CV that visually resembles a modern Microsoft Excel workbook while remaining a genuine Power BI semantic-model and interaction showcase.

The report is not a conventional dashboard. It is a CV/portfolio experience that demonstrates:

- professional experience and projects;
- technical and business capabilities;
- Power BI modelling and UX discipline;
- Excel-inspired interaction design;
- credible qualitative storytelling without invented quantitative analytics.

The project will be maintained in **PBIP format**. The PBIP started as a blank skeleton to which the user added `dim_calendar` as the first step; everything else was built by the agent. On 2026-10-03 that calendar was replaced by an agent-built one (user), so the whole model is now agent-built.

## 2. Non-negotiable instructions

1. Use **snake_case** for all table names, column names, measures, variables where applicable, display folders, bookmark names, object names, and technical identifiers.
2. The source workbook is `_input/CV.xlsx` (one worksheet, `cv_input`). In the public repository it is a structurally identical dummy; the author's real workbook is private (§3, §10).
3. Source images, icons, backgrounds and separators are stored under `_input/_images/`, split by provenance into `excel_derived/` (artwork based on the Excel interface) and `original/` (2026-10-02).
4. Use the provided structured Excel tables exactly as named.
5. Do not rename source tables or columns without documenting and obtaining approval.
6. Do **not** draft Power Query or DAX in the documentation files. They are implemented directly in the PBIP.
7. Do not invent scores. The 1-7 scores are the user's own, entered in the workbook: `overall_proficiency_level` (tools and skills) is a self-rating of current proficiency (not career-wide; wording corrected 2026-10-02, user), `context_relevance_level` on the bridges is per-item relevance. Where a score is blank it stays blank — never inferred or defaulted. *(Updated 2026-09-26: this item originally said every score field was intentionally blank; that described the workbook before the user filled it.)*
8. Use `999912` as the open-ended `to_month_code`, and `9999` as the open-ended `to_year` on year-grain tables. In visuals, both must be rendered as blank/current rather than displayed literally.
9. `dim_calendar` is not used through a standard active range relationship for roles/projects. Date-range behaviour must be handled in DAX because roles and projects contain `from_month_code` and `to_month_code`.
10. The Excel World Championship achievement must be highly prominent in Executive Summary and at the top of Additional.
11. Contact details come only from `tbl_contact_methods` and are rendered as stored. The phone value is the string `#REF!` in both workbooks, a deliberate joke (§9, Contact). The e-mail addresses and the LinkedIn URL are real in the author's workbook and bracketed placeholders in the public dummy.
12. All image assets are supplied under `_input/_images/`. If an asset is ever missing, use a plain rectangle or circle named `shp_placeholder_*` / `img_placeholder_*` in its final bounding box (§11), never a broken image reference.
13. The report has one principal report page. Navigation uses bookmarks, slicers, buttons, visual states, one File overlay and one dismiss-only first-load dialog (§8A).
14. Sheet-tab bookmarks must remain independent from ribbon state. There must be no dual-bookmark state matrix.
15. No quantitative bar charts. No invented, estimated or inferred durations. A duration-by-tool calculation **is** permitted where every input is stated in the source and the arithmetic is exact — see §12.

## 3. Authoritative content source

The workbook is the single authoritative source for all CV content: table names, column names and every row. There is no other content source, and nothing is populated from memory or general knowledge.

- **Author's build:** the real workbook, kept outside the repository (`_private/CV_real.xlsx`). The `cv_source_path` Power Query parameter points the model at it.
- **Public repository:** `_input/CV.xlsx`, a dummy with identical tables, columns, rows and IDs (§10).

This specification holds for both. Where a statement depends on row content (counts, names, examples) it describes the real workbook, and the dummy mirrors its structure.

`<TODO: VERIFY!>` marks text that still needs the author's approval. None remain in either workbook or in the report. Any new unverified text gets the marker and keeps it until approved.

## 4. Report canvas and single-page architecture

Canvas size: **2072 × 1020 px**. *(Widened 1920 -> 2072 on 2026-09-21: measured from the publish-to-web full-screen render on a 1920 x 1080 screen, where the report was height-bound at scale 0.926 and left a 71 px white stripe each side. 1920 squared / 1778 = 2073.4; 2072 is the 4 px-grid value. The extra 152 px is inserted at the RIGHT -- left chrome is untouched, right chrome and the column-letter row shift +152.)*

Vertical zones, measured from `background.png` on 2026-10-03 (the earlier 30 / 30 / 112 / 36 / 107 chrome heights were planning figures from the original 1536 × 816 layout and never matched the artwork):

| Zone | Y | Height | Purpose |
|---|---:|---:|---|
| Title / Quick Access Toolbar | 0 | 60 | Workbook title and Quick Access Toolbar icons |
| Ribbon tab row | 60 | 38 | File button and non-clickable Home label |
| Home ribbon | 98 | 124 | View, Detail Level, Perspective, Filters and Other groups |
| Formula bar | 222 | 64 | Name box, fx icon and read-only DAX-driven formula text |
| Column-letter band | 286 | 29 | Fake column letters; painted by the background and the per-sheet band |
| **Main content canvas (white grid)** | **315** | **638** | All sheet content containers |
| Sheet-tab strip | 953 | 41 | Excel-style sheet tabs |
| Status bar | 994 | 26 | "Ready", selection summary (§6A), view icons |

**Corrected 2026-09-03 by measuring `background.png` directly.** The main content canvas is
`x 35, y 315, 2037 × 638` — the white, gridline-free rectangle in the artwork. The previous
`208 / 764` row was a planning estimate written before the background existed; it overlapped the
formula-bar visual (which occupies 238–298) and the column-header band beneath it, and no sheet
content may sit there.

Standard content padding inside the grid is **24 px on all four sides** → usable inner box
`x 59, y 339, 1989 × 590`, right edge 2048, bottom edge 929.

### Layering

1. Base workbook chrome/background.
2. Home ribbon and formula bar.
3. Sheet-specific content groups.
4. File overlay group above all sheet content.
5. First-load dialog above the overlay (§8A).

## 5. User interaction model

### 5.1 Sheet tabs — primary navigation

Static sheet tabs at the bottom:

1. Exec summary
2. Experience
3. Projects
4. Tech stack
5. Skills *(added 2026-09-25, user; `sheet_tab_id` 7, so existing ids stay stable)*
6. Education
7. Additional

Each tab triggers one bookmark:

- `bm_sheet_exec_summary`
- `bm_sheet_experience`
- `bm_sheet_projects`
- `bm_sheet_tech_stack`
- `bm_sheet_skills`
- `bm_sheet_education`
- `bm_sheet_additional`

Bookmark scope:

- capture display/visibility of sheet-specific content groups;
- do not capture ribbon slicers;
- do not capture Detail Level, Perspective, Story/List state or filters;
- do not capture the File overlay.

Each tab is a `btn_sheet_*` button with an `img_sheet_*_select` image for its selected state.

### 5.2 Ribbon selector — File / Home

The row above the Home ribbon contains:

- **File** — clickable and opens the File overlay;
- **Home** — visually selected by default and not clickable.

File must mimic Excel Backstage behaviour at a reduced scope. It opens an overlay with a left-side menu:

- About
- Contact
- Help
- Special Thanks, bottom-anchored in the rail

Close the overlay with the Excel-like left-pointing arrow, or with the Recent row on the About pane (§8).

### 5.3 Home ribbon groups

#### View

A binary Story/List control: a single-select ribbon slicer on `tbl_view_modes`.

- Story mode: running prose.
- List mode: bullet-point text with word wrapping in a reserved layout area.

Source: the `view` column (`Story` / `List`) and the `text` column of `tbl_role_content`, `tbl_project_content`, `tbl_tool_content` and `tbl_education_content`.

The two modes must not change grouping or filters. They only swap the textual representation and, where needed, the text-container sizing.

**View greys on Exec summary** (§5.4). The executive summary is running prose and stays prose in every state, so `tbl_exec_summary_content` carries no `view` axis — its grain is `detail_level` × `text`.

#### Detail Level

A binary Compact/Standard control: a single-select ribbon slicer on `tbl_detail_levels`, same segmented-toggle look as View.

- Compact mode: the shorter text authored for each item — the `detail_level = "Compact"` rows of the content tables.
- Standard mode: the full text — the `detail_level = "Standard"` rows.

Detail Level does **not** change:

- text mode (Story vs List remains as chosen);
- grouping or sort order (Perspective remains as chosen);
- filter state.

It swaps only the density of what is shown. Story × Compact is still Story mode, just fewer sentences per item; List × Standard is still List mode, just more bullets per item. The two axes are orthogonal, producing four states per sheet (Story-Compact, Story-Standard, List-Compact, List-Standard).

Compact is source-driven. Both densities are authored in the workbook (`tbl_role_content`, `tbl_project_content`, `tbl_tool_content`, `tbl_exec_summary_content`) and the control only selects between them. It never shows a different set of items, and nothing is truncated or summarised by rule. Blank fields stay blank.

Replaces the previous Viewer Type control. The supporting tables `tbl_viewer_types` and `tbl_audience_priority` have been **deleted** from the workbook (see §9). Nothing may reference them.

#### Perspective

Options:

- Timeline
- Skills
- Industries

Perspective primarily changes grouping and ordering, not text mode.

- Timeline: chronological grouping/order using month codes.
- Skills: group/order through role-skill and project-skill bridge tables.
- Industries: group/order by a single industry per item — roles inherit `tbl_employers[industry_id]`, projects carry their own `tbl_projects[industry_id]`. The `tbl_role_industry` / `tbl_project_industry` bridges were deleted (§9), so each item falls in exactly one group rather than several. Live counts: **6 groups across 11 roles** on Experience, **7 groups across 9 projects** on Projects.

  Known asymmetry, to solve in layout rather than in data: the two most recent roles (10 and 11) both group under their employer's own industry, so the range of client sectors those roles covered appears only on Projects. The two sheets are complementary; a reader who only groups Experience by industry will undercount sector breadth.

Story/List mode remains independent. Story mode does not regroup merely because it is Story mode.

Perspective is not bookmark-driven. The `sli_ribbon_perspective` slicer selects a row of `tbl_perspectives`, which filters two tables derived in Power Query — `tbl_role_perspective_rows` and `tbl_project_perspective_rows` — and the Experience and Projects matrices group on their `group_label` (§9). Sheet bookmarks never touch it.

#### Filters

Four dropdown slicers, multi-select:

- Function
- Industry — bound to `tbl_industries[industry_group]`, not `industry_name` *(2026-09-25, user)*: fewer dropdown items, and one group spans several industries. Perspective → Industries and the detail panel still use `industry_name`
- Skill
- Tool

They mimic Excel's compact font-selector dropdown as closely as Power BI permits; the as-built styling is in `_context/visual_reference.md` §4.

The categories are supported by:

- `tbl_functions`
- `tbl_industries`
- `tbl_skills`
- `tbl_tools`
- bridge tables for roles/projects.

Function is the filter that reaches both branches cleanly: `tbl_roles` and `tbl_projects` each carry their own FK into `tbl_functions`, so a selection propagates into Experience and Projects independently. Role Type is deliberately **not** a filter — it exists on roles only, and with no role-to-project relationship (§9) it could never reach Projects. It surfaces in the detail panel instead (§7).

### 5.4 Per-tab command availability

Not every ribbon group applies to every sheet. Follow Excel's native ribbon behaviour: commands that do not apply to the current sheet are **greyed out** (disabled foreground, no hover response, tooltip still readable), not hidden. Availability matrix:

|  | Exec | Experience | Projects | Tech stack | Skills | Education | Additional |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| View (Story / List) | ○ | ● | ● | ● | ○ | ● | ○ |
| Detail (Compact / Standard) | ● | ● | ● | ● | ○ | ○ | ○ |
| Perspective (Timeline / Skills / Industries) | ○ | ● | ● | ○ | ○ | ○ | ○ |
| Filters (Function / Industry / Skill / Tool) | ○ | ● | ● | ● | ● | ○ | ○ |

**Skills** *(2026-09-25)* carries no text content, so View and Detail grey. Filters stay live on the Tech stack logic: a Function, Industry or Tool selection keeps only the skills linked to matching roles or projects through `tbl_role_skill` / `tbl_project_skill`.

● active · ○ greyed

Rationale for the exceptions:

- **Additional** is context-less by design (a mixed bucket of awards, certifications, languages, membership) — every ribbon group greys.
- **View greys on Exec summary** (set 2026-09-03). The executive summary is running prose and must stay prose — a bulleted List rendering of it would not carry the message. Detail Level stays active, so Compact and Standard swap between a tighter and a fuller version of the same prose. This keeps one live control on the landing sheet, which matters: with View, Perspective and Filters all greyed, a fully dead ribbon on the sheet the viewer arrives at would read as broken rather than as honest greying.
- **Detail Level greys on Education** *(set 2026-09-18, user)*. The entries are short enough that a
  Compact rendering would differ from Standard by nothing, and inventing a shorter variant to
  justify the control is exactly the fabrication §2 item 7 forbids. `tbl_education_content`
  therefore has no `detail_level` column (dropped 2026-09-25, user): its grain is `education_id` ×
  `view` × `text`. **View stays active**, so Story and List both swap on Education; `edu_body_text`
  ignores the Detail Level ribbon.

- **Perspective** applies only to Experience and Projects, which have the skill bridges (`tbl_role_skill`, `tbl_project_skill`), a single-valued industry per side (§9), and the month-code range needed for real regrouping. Regrouping Exec / Tech stack / Education is either trivial or degenerate, so greying is honest.
- **Filters** stay active on Tech stack because filtering by Function meaningfully changes which tools are shown via the `tbl_role_tool` bridge — selecting a function keeps only the tools actually used in that kind of work. The filter therefore expresses "which tools I used doing this kind of work", not "which tools I know". Exec summary is curated content that should not respond to filters; Education has too few rows for filtering to be meaningful.

When any filter is active on a filterable sheet, the formula bar must indicate the active-filter state (see §6) so viewers do not mistake a filtered subset for the full inventory.

**Each filter's 16 × 16 icon carries a hover label** naming its group — Function, Industry, Skill, Tool *(added 2026-09-22)*. The dropdowns themselves are unlabelled by design (Excel's font selector carries no caption), so the icon is the only place the group name can live without adding chrome. Implemented as an inert button over each icon, because a Power BI image visual cannot show a tooltip: `_context/visual_reference.md` §3.40.

**Right-clicking a control is not recoverable, and cannot be blocked** *(established 2026-09-22)*. Power BI's canvas context menu — Include / Exclude / Clear selection on slicers, Drill up / Expand / Collapse on matrices — is built into the product and is not report-authorable; neither the visual capability schema nor `ExplorationSettings` exposes a switch. Excluding the selected member of a single-select ribbon slicer leaves it inert until the page is reloaded. A reload is therefore the only reset route. File → Help says so under *If anything gets stuck*: "Nothing you do here is saved. Reload the page and the CV returns to exactly how it started."

### 5.5 Other group — Clear Filters and Help

A fifth ribbon group, labelled **Other**, sits right of Filters. It holds two commands that act on
the report rather than carrying state:

- **Clear Filters** *(added 2026-09-21)* — resets the four filter slicers to no selection. It
  **greys with Filters**: active on Experience, Projects, Tech stack and Skills, greyed on Exec summary,
  Education and Additional. It is implemented as a button firing a bookmark scoped to those four
  slicers, deliberately **not** Power BI's page-wide "Clear all slicers" action, which would also
  empty the hidden `sli_sheet_state` slicer that carries the current sheet. Mechanism and the
  rejected alternative: `_context/visual_reference.md` §3.38.
- **Help** — never greys, on any sheet (§3.23 of the same file). **Since 2026-09-28 it opens File → Help on click** (`bm_overlay_help`); UAT testers clicked sheet tabs and never hovered it. It still carries a hover tooltip. *(Before that it only signposted File → Help in the tooltip and did
  not navigate.)*

## 6. Formula bar

The formula bar is display-only.

Components:

1. Name box: a small visual containing context-sensitive DAX output — a bare cell reference such as `A1`, `B4`, `C2`.

   **Corrected 2026-09-11 by the user: the Excel name box never renders the sheet name.** It used to read `EXP!B4`; `fbar_name_box` is now `[fbar_cell_ref]` alone. `fbar_sheet_code` survives unreferenced — it is still the natural source for a sheet prefix elsewhere, so it was left rather than deleted in the same step (`agent_rules.md` §3, in the author's private notes).
2. `fx` icon: static chrome, not interactive.
3. Formula text: DAX-driven read-only text (`fbar_formula`) rendering the current sheet state — view mode, detail level, perspective, active filters or the selected row — as an Excel formula (below). When any filter is active on a filterable sheet, the formula text must indicate this explicitly so viewers understand the visible content is a filtered subset — a filtered Tech stack sheet must never appear to claim the CV lists only two tools in total.

   *(History: until 2026-09-26 the text was a `VIEW(...) · DETAIL(...) · FILTERED(...)` chain. Its filter clause first enumerated the selected values, then counted them, because enumeration was unbounded against a container that clips silently. `_context/visual_reference.md` §3.37.)*

**Excel-syntax formula text** *(2026-09-26, user)*. `fbar_formula` now renders the sheet state as a real Excel 365 formula rather than the `VIEW(...) · DETAIL(...)` chain: `=XLOOKUP("Standard", Summary[Detail], Summary[Text])` on Exec, `=GROUPBY(Projects[Period|Skill|Industry], Projects[Story Standard], CONCAT, , 0, , <filter>)` on Experience/Projects, `=SORT(Tools[[Tool]:[Story Standard]], 3, -1)` on Tech stack, `=SORT(Skills, {2,3}, {-1,-1})` on Skills, `=SORTBY(...)` on Education and Additional. Active filters become `COUNTIF(FunctionPick,Projects[Function]) * ...` — named lists, never enumerated values, so the text stays bounded (worst case ~225 characters against a 1760 px container). The old `fbar_view` / `fbar_detail` / `fbar_perspective` / `fbar_filtered` measures were deleted 2026-09-26 (user).

**Row selection** *(2026-09-26)*. Clicking a row on Experience, Projects, Tech stack or Skills cross-filters only the name box, the formula bar and (Experience and Projects) that sheet's detail panel — every other data-bound visual is set to `NoFilter` from those four matrices in `page.json`. The name box then shows that row's cell (`B` + row for an item, `A` + row for a group cell on Experience/Projects, `A` + row on Tech stack/Skills; row 1 is the header), and the formula bar shows `=XLOOKUP("<item>", Projects[Project], Projects[Story Standard])`. Row numbers assume the matrix's default ascending sort (group label, then the sort key).

The formula bar reads state but never controls it.

## 6A. Status bar and Exec highlight

*(2026-09-26, user.)* **Zoom slider and zoom % removed from the painted status bar on 2026-09-28 (user)** — Power BI's own iframe zoom sat right below and confused UAT testers; Excel allows the same via Customize Status Bar. The view icons moved to the right edge (1729 → 1945) and `vis_status_bar` followed. `vis_status_bar` sits in the painted Excel status bar left of the view icons and reads like Excel's selection summary: `Count: n` on Experience, Projects, Education and Additional, `Average: x.x    Count: n` on Tech stack and Skills (average of the visible items' own ratings, blanks skipped as Excel's AVERAGE does). Blank on Exec. It ignores row selections.

Exec summary carries a highlight "cell" at `1600,415 / 448×150`: pale green fill, green border, the `is_highlight` achievement's name, year and detail, and a link line; the whole cell is a `WebUrl` button whose URL is the measure `exec_hl_url` (all data-driven from `tbl_additional`, the newest `is_highlight` row).

## 7. Detail panel with radar chart

Experience and Projects each carry a task-pane-style detail panel right of the matrix. Selecting a single role or project row fills it; with nothing selected it reads "Select a role to see its profile." / "Select a project to see its profile.". Nothing in the panel reacts to hover.

Content, top to bottom:

- title; employer · period;
- one line of industry · role/project type · location. Roles read `tbl_role_types` through `role_type_id`; projects have no such FK and are all `role_type_id` **2, "Project role"**, so the project-side type is that constant. Location is roles only, from `tbl_roles[location]`;
- radar over **skills only**: up to 4 Technical plus 4 Soft, 8 axes maximum, from `tbl_role_skill` / `tbl_project_skill`, plotted on `context_relevance_level`. Tools are **not** radar axes;
- up to **five** tools, ranked by the bridge's `context_relevance_level` and each labelled with `tbl_tools[overall_proficiency_level]` as `n/7` (a blank proficiency shows the name alone);
- one concise highlight line;
- an italic footnote stating the two 1-7 scales. The same explanation is in File → Help under "The 1 to 7 scales"; the panel repeats it because a reader meets the panel first.

Radar scale: **1-7**, shared across all skills and tools. Blank scores are deliberate: do not plot blanks and do not infer values. Keep the panel compact; no Story/List text.

### 7.1 Relevance versus proficiency

Two different 1-7 measures exist and must not be conflated:

- **`context_relevance_level`** on the skill and tool bridges: how central that skill was *to that role or project*. This is what the radar plots, because the radar describes one item.
- **`overall_proficiency_level`** on `tbl_skills` and `tbl_tools`: the self-assessment of current proficiency, independent of any single item (*not* career-wide, 2026-10-02, user). It stays out of the radar. Skill proficiency lives on the **Skills** sheet (Skill · Type · Proficiency `n/7` · Usage); tool proficiency on Tech stack and as the `n/7` label on the panel's tools.

### 7.2 Suppression via `tooltip_override_text`

Where `tbl_roles[tooltip_override_text]` is non-blank (the column keeps its source name), that text replaces the panel body below title and period: **radar, industry/type line, tools, highlight and footnote are all suppressed.** Populated for roles 10 and 11, whose detail lives in Projects. Suppression is source-driven and explicit, never derived from a "fewer than N skills" rule. It is implemented in DAX (the measures empty out and `vis_exp_panel_override` shows the text), not by hiding visuals.

### 7.3 Where the short summaries are used

**`role_summary_short` and `project_summary_short` feed only the panel's highlight line**, and are never read for an item carrying an override. The sheets render role and project text from `tbl_role_content` / `tbl_project_content` at the current `view` × `detail_level`, which is what gives View and Detail Level their effect there.

### 7.4 Implementation

- `sel_*` measures compute the item; each branches on `sel_entity`, which resolves the selected row to `"role"` or `"project"` and reads that side's own tables. `pnl_*` wrap them behind `pnl_selected` (exactly one project **or** role row selected). `pnl_hint` / `pnl_hint_role` are the empty-state prompts, `pnl_override` the override text.
- The radar is a DAX SVG measure (`sel_radar_svg`, `dataCategory: ImageUrl`) in a `pivotTable` cell; Power BI has no native radar. `pnl_radar_svg` raises its labels 10 → 12 and widens the viewBox so they fit.
- **The panel is filter-independent.** It describes the selected item, so an active Skill, Tool, Function or Industry filter does not trim its radar or tools.
- Each matrix cross-filters only its own panel, the name box and the formula bar (page-level `NoFilter` to everything else). Hover tooltips are off on both matrices, and a transparent action-less button covers each panel.
- Geometry in `design.md` §20; per-visual rows in `visual_reference.md` §4.

## 8. File overlay

There is one navigable overlay in the final report (plus the dismiss-only first-load dialog, §8A).

Left menu (top group):

- About
- Contact
- Help

plus **Special Thanks**, bottom-anchored in the rail.

Content:

- About: explanation of the interactive CV project and why it was created — a proof of concept inspired by the Fabric challenge, plus a secondary column on how the report is built. **The challenge link landed 2026-09-22 and the `<TODO: VERIFY!>` is cleared**: the word *this* in "inspired by this Fabric challenge" is a link run pointing at the Fabric community DataViz-contest blog post (§3.26 of `visual_reference.md` for the run shape and its fallback).
  **Repo line** *(added 2026-09-26, user)*: the last body paragraph reads "You can download this whole project from my first public repo here.", with **here** a run-level link to `https://github.com/adamreichenbach/power-bi-excel-cv`. The LinkedIn-article line goes below it later.
  **Date modified** *(stamp added 2026-09-24; reworked 2026-10-02, user, as Excel's native Recent list)*: the Recent row carries a "Date modified" column — header `txt_overlay_about_recent_date_heading`, value `vis_overlay_about_last_updated` reading "Sep 2026" via `about_last_updated`. The old "Last updated September 2026" line under the body is gone. Its only source is the `latest_publish_month` parameter (surfaced as `tbl_profile[latest_publish_month]`, since DAX cannot read an M parameter), so the stamp and the `999912` clamp always agree.
- Contact: composed in DAX from `tbl_profile` (name, title, location) and `tbl_contact_methods` (every address and URL), ordered by `display_order`.

  **Changed 2026-09-09.** Contact details used to be four columns on `tbl_profile` — `linkedin_url`, `email_placeholder`, `phone_placeholder`, `public_contact_note`. All four are deleted. A second e-mail address was needed, and widening the row would have modelled two addresses as two different *attributes* rather than as two rows of one fact; the third address would have forced the change anyway. `tbl_profile` is now the single source of truth for **identity**, `tbl_contact_methods` for **contact**. Same reasoning that deleted `tbl_links` on 2026-09-03: one fact, one home.

  `public_contact_note` was dropped outright rather than moved — the user judged it did not earn its place, and any standing note about placeholder values will be authored as overlay text or re-sourced later.
- Help: interaction guide — sheet navigation, the Home ribbon (View, Detail Level, Perspective, Filters and per-sheet greying), the read-only formula bar and the two 1 to 7 scales — beside the Clippy figure and its caption.
- Special Thanks: user copy landed 2026-09-24 — thanks to the authors of the two open-source Power BI agent toolsets the build relied on (named in `README.md` → *Thanks*), four run-level links, single column (the "And the tools" aside was removed). Capitalisation follows the button artwork, which reads "Special Thanks"; this is a deliberate exception to the sentence-case rule in `_context/design.md` §5.2.

**Pane authoring, set 2026-09-11.** Every pane is authored as report-layer text boxes except Contact, which is composed from the model. **`tbl_file_menu` was deleted 2026-09-11**, from the workbook and the model. Once the panes became text boxes nothing read it, its id 2 Help text still named the retired Viewer Type control and a Role Type filter that was never built, and a single `content_text` column could not carry what the panes need — subheadings, bullets, links.

Pane layout is a two-column grid inside the `x 200, y 60, 1872 × 960` content pane — column A at `x 264 w 754`, column B at `x 1138 w 754`, both starting at `y 186` under a heading at `y 116`. Right margin **180** *(448 → 264 → 180 across two passes on 2026-09-22, user)*. Contact and Help are deliberately off this grid. Full geometry and rationale in `_context/visual_reference.md` §4.1a.

The overlay must open over any current sheet/ribbon state and close without resetting that state.

**Landing pane** *(2026-09-28, user, after the first UAT round)*: the report opens on **File → About**, as Excel opens on its own Backstage. The About-pane visuals are visible on disk; Exec summary stays the default sheet underneath, so the close arrow lands on Exec summary with no special handling. The last About paragraph points first-time readers to Help. **Recent row** *(2026-09-29, user, UAT: testers did not find the back arrow; reworked 2026-10-02)*: a "Recent" label and one row — the file icon, **Adam Reichenbach CV.xlsx** and its Date modified — which closes the overlay through `bm_overlay_close` — the Exec summary on first load, the active sheet on any later visit, so its caption reads "Open the workbook" (2026-10-02, user), as Excel users open a recent file from Backstage. The back arrow also gained the tooltip "Back to the workbook".

## 8A. First-load dialog

*(Added 2026-09-24, user.)* A native-Excel-style modal is visible when the report opens, centred over the About pane (the landing view, §8); it sits above the overlay in z-order and neither bookmark set touches the other's visuals. It exists to make the illusion explicit to visitors who do not arrive knowing this is Power BI — a senior Power BI developer took the report for Excel.

- Chrome copies the Excel close-without-saving prompt: "Microsoft Excel" title, inert close ×, warning triangle, message *"Pick a cell, any cell. (Spoiler: it's not Excel.)"*, and two buttons — **Continue** (Excel green, white text) and **Cancel** (neutral).
- Both buttons do the same thing: fire `bm_dialog_dismiss`, which hides all sixteen dialog visuals. Nothing can show it again; a page reload does (Power BI keeps no per-visitor memory, so it appears on every load).
- Nine action-less buttons tile the whole canvas except the Continue and Cancel rectangles, blocking every other control while the dialog is open: four veil tiles around the window and five transparent tiles inside it. They carry a **45% white veil** *(2026-09-29, user, after UAT: a tester clicked Continue and expected something to happen)*, so the page behind reads as inactive and dismissing visibly reveals it. Power BI cannot blur; the veil is the closest honest equivalent. The veil tiles never overlap the window: a clicked visual is drawn on top until the click ends. Nothing sits under the two buttons, so the Service's render order cannot bury them.
- No other bookmark targets the dialog visuals, so sheet, overlay and Clear Filters bookmarks never touch it.

## 9. Semantic model overview

Reconciled against the workbook on 2026-10-03. It holds **26 structured tables** on the `cv_input` worksheet, all loaded. The model adds `dim_calendar` and three tables derived in Power Query (`tbl_additional_items`, `tbl_role_perspective_rows`, `tbl_project_perspective_rows`), plus `_measures`: 30 tables and the measures table. The workbook remains authoritative — where this section and the workbook disagree, the workbook wins and this section gets fixed.

### Core dimensions

- `tbl_employers`
- `tbl_role_types`
- `tbl_functions` *(renamed from the earlier `tbl_role_functions`)*
- `tbl_industries`
- `tbl_skills`
- `tbl_tool_groups`
- `tbl_tools`

### Core entities

- `tbl_roles`
- `tbl_projects`
- `tbl_education`
- `tbl_additional` — awards, certifications, languages and memberships in one table *(2026-10-03, user; replaced `tbl_achievements`, `tbl_certifications`, `tbl_languages` and the old category-only `tbl_additional`)*: `additional_id`, `section`, `section_order`, `title`, `detail`, `from_year`, `to_year` (`9999` open-ended; both blank for languages), `url`, `is_highlight`
- `tbl_profile` — identity only since 2026-09-09: `profile_id`, `full_name`, `professional_title`, `location`
- `tbl_contact_methods` — every way of reaching the candidate, one row per method

### Content tables

- `tbl_role_content`
- `tbl_project_content`
- `tbl_exec_summary_content` — grain `detail_level` × `text`, 2 rows. Renamed from `tbl_exec_summary_items` on 2026-09-03 when the sheet became running prose; View greys on Exec (§5.4), so there is no `view` axis here.
- `tbl_additional_items` — **derived in Power Query** *(added 2026-09-23)*: the Additional sheet's one list, read from `tbl_additional` *(single source since 2026-10-03; it used to append four tables)*. Grain: one row per item; `item_key` = (0 if `is_highlight` else 100000) + `section_order` × 1000 + rank within section (`from_year` descending), and `title` sorts by it — so highlighted items lead, then section, then year *(order set by the user 2026-09-23)*. `to_year = 9999` renders as `present`. `tbl_additional[is_highlight]` (source-driven) tints the row and floats the row to the top. Disconnected.
- `tbl_education_content` — grain `education_id` × `view` × `text`; no `detail_level` (§5.4).
- `tbl_tool_content` — grain `tool_id` × `view` × `detail_level` × `text`, matching the role and project content tables. Only a headline subset of tools carries rows; blanks stay blank. This is what gives View and Detail Level something to act on for the Tech stack sheet.

### Contact

`tbl_contact_methods` — grain: one row per way of reaching the candidate.

| column | notes |
|---|---|
| `contact_method_id` | int PK |
| `profile_id` | FK → `tbl_profile`, many-to-one |
| `method_type` | closed vocabulary: `email` / `phone` / `linkedin` |
| `method_label` | what distinguishes two of the same type — Primary / Secondary / Mobile / LinkedIn |
| `method_value` | the address or URL |
| `display_order` | drives `sortByColumn` on `method_label` (strict 1:1 holds) |
| `is_primary` | boolean; one TRUE **per `method_type`**, not one per table |

Four rows live: two e-mail, one phone, one LinkedIn. The phone is the string **`#REF!`**
(§2 item 11) — a deliberate joke, not a broken formula. It is stored in `CV.xlsx` behind Excel's
leading-apostrophe text marker, because entered bare it made Power Query treat the cell as a
genuine error; the apostrophe is a display marker and is **not** part of the value, so the model
loads exactly `#REF!`, five characters. Render it verbatim, but **never without its framing copy**
on the Contact pane — alone it reads as a bug rather than a joke.

`is_primary` is a `boolean` column (`type logical` in M).

### Bridges

- `tbl_role_skill`
- `tbl_project_skill`
- `tbl_role_tool`
- `tbl_project_tool`

**`tbl_role_industry` and `tbl_project_industry` no longer exist.** Industry is single-valued and reaches each side directly: roles inherit it from `tbl_employers[industry_id]`, projects carry their own `tbl_projects[industry_id]`. This was a deliberate simplification, not drift. It narrows what Perspective → Industries can group on — one industry per role or project rather than many — which is the intended behaviour.

**`tbl_links` was deleted on 2026-09-03.** It duplicated facts already owned elsewhere: the LinkedIn URL (now a `tbl_contact_methods` row), and achievement URLs live on `tbl_additional[url]` (formerly `tbl_achievements[url]`). It also carried a conflicting value for the same award. One fact, one home.

### Derived perspective tables

- `tbl_role_perspective_rows`, `tbl_project_perspective_rows` — **derived in Power Query**, not in the workbook. One row per perspective × group × item (`perspective_id`, `group_label`, `skill_id`, the item's id and its display columns), so an item appears once under Timeline and Industries and once per linked skill under Skills. The Experience and Projects matrices are built on them (§5.3).

### Interaction/configuration tables

- `tbl_sheet_tabs`, `tbl_view_modes`, `tbl_detail_levels` — disconnected, read by slicer and measure
- `tbl_perspectives` — related to the two perspective tables above; that relationship is what lets the Perspective slicer regroup the matrices

`tbl_viewer_types` and `tbl_audience_priority` were deleted with the Viewer Type control they supported (§5.3). Nothing may reference them.

### Relationships

Use one-to-many relationships from dimensions and entities into their children and bridges wherever there is an unambiguous key.

Active relationships (24 in total):

- `tbl_employers` → `tbl_roles`
- `tbl_role_types` → `tbl_roles`
- `tbl_functions` → `tbl_roles`
- `tbl_functions` → `tbl_projects`
- `tbl_industries` → `tbl_employers`
- `tbl_industries` → `tbl_projects`
- `tbl_roles` → `tbl_role_content`
- `tbl_projects` → `tbl_project_content`
- `tbl_tools` → `tbl_tool_content`
- `tbl_exec_summary_content` is **disconnected** — 2 rows keyed only by `detail_level`, read by measure, not by relationship
- `tbl_profile` → `tbl_contact_methods`
- `tbl_roles` / `tbl_projects` → their respective skill and tool bridges
- `tbl_skills` / `tbl_tools` → their respective bridges
- `tbl_tool_groups` → `tbl_tools`
- `tbl_education` → `tbl_education_content`
- `tbl_perspectives` → `tbl_role_perspective_rows` and `tbl_project_perspective_rows`
- `tbl_roles` → `tbl_role_perspective_rows`; `tbl_projects` → `tbl_project_perspective_rows`
- `tbl_additional`, `tbl_additional_items`, `tbl_sheet_tabs`, `tbl_view_modes` and `tbl_detail_levels` are **disconnected**

Deliberately **not** relationships:

- **`tbl_employers` → `tbl_projects` is lookup-only.** `tbl_projects[employer_id]` exists for attribution and display, and must NOT be wired as an active relationship. Doing so creates a diamond — `industries → employers → projects` alongside `industries → projects` — and reintroduces exactly the ambiguity the single-valued industry design removes. It is absent from `relationships.tmdl`; keep it that way.
- **`tbl_roles` → `tbl_projects` no longer exists in any form.** `parent_role_id` was dropped from `tbl_projects`: projects can span two roles at the same employer (two projects straddle the two most recent roles), so a single-role FK would have forced a false assignment. Nothing in the model consumes a role-to-project link — projects reach their dimensions through their own FKs and bridges, and Timeline orders on the project's own month codes.
- **`dim_calendar` has no active range relationship** to role or project month codes. Date-range behaviour is DAX-side, per §2 item 9.

Treat interaction and configuration tables as disconnected unless a specific implementation genuinely needs otherwise.

Use numeric integer IDs for roles, projects and bridge-table keys.

## 10. Input workbook conventions

**The public `_input/CV.xlsx` is a dummy** *(2026-09-30)*: same tables, columns, rows, IDs, month codes and links as the real workbook, with identifying text as bracketed placeholders (`[Employer 3]`, `[Role 4 · List · Compact · bullet 2]`), the other descriptive text invented, and every 1-7 score randomised. Placeholders for List text keep the real bullet count and prose placeholders the real rough length, so layouts behave as live. Reading guide in `README.md` → *The data*.

- One worksheet: `cv_input`.
- Every entity is a formally named Excel structured table.
- Tables are placed horizontally with exactly one empty column between them.
- Tables may grow downward; their column structure should remain stable.
- All source names use snake_case.
- New records must preserve unique numeric IDs.
- Month codes use six-digit integers in `YYYYMM` form.
- Open-ended end month is `999912`.
- **Open-ended end year is `9999`** — the year-grain sentinel, used where a table carries
  `from_year` / `to_year` rather than month codes (currently `tbl_additional`). It gets the
  same treatment as `999912`: never displayed, rendered as blank / "current" / "present".

## 11. Placeholder visual assets

Every asset is supplied and the report uses no placeholder shapes. Should an asset be missing or pending:

- use a plain rectangle or circle in its place;
- name it `shp_placeholder_*` / `img_placeholder_*`, saying what the final asset will be;
- give it the final asset's bounding box, so the swap is trivial.

## 12. Content cautions

- Do not display `999912`, and do not display `9999` where it appears as an open-ended `to_year` (`tbl_additional`).
- Do not manufacture skill scores, tool scores, or impact metrics.
- **Durations by skill/tool: derived-exact only.** Manufacturing a duration is banned — estimating intensity, assuming a period that the source does not state, or presenting a guess as a figure. Deriving one is allowed: if a tool's linked roles and projects each carry stated `from_month_code` / `to_month_code`, counting the distinct `dim_calendar` months those periods cover invents nothing. Overlaps collapse by construction and non-consecutive gaps are excluded rather than spanned, so the result is exact rather than approximate.

  Two conditions attach to displaying it:

  1. `999912` must be clamped to the `latest_publish_month` Power Query parameter before counting, or coverage runs to year 9999.
  2. The duration must appear beside its evidence denominator (`x roles · n projects`) so the reader sees what the figure rests on. A bare "17 years 4 months of Excel" overstates what a role-level linkage can support; "17 years 4 months · 6 roles · 3 projects" does not.

  *(Relaxed 2026-08-02 with user approval. The original blanket ban was written against an earlier estimate-based proposal and does not describe this mechanism.)*
- Preserve original CV wording wherever available.
- Unverified text carries `<TODO: VERIFY!>` and stays visibly traceable until the author approves it (§3). None remain today.
- Keep the 2024 Microsoft Excel World Championship Finalist achievement prominent.
- Use the Additional tab for awards, certifications, languages and membership. No personal-interest section is required.
