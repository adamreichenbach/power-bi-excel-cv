# Design System — Microsoft 365 Excel Desktop / Fluent 2

## 1. Target and scope

This specification targets the **current Microsoft 365 Excel desktop application in the light theme**, using the modern Office visual refresh and Fluent 2 design language.

It must **not** resemble the flat, strongly coloured Excel 2013–2019 interface.

The target appearance is characterised by:

- predominantly white and very light neutral application chrome;
- Excel green used selectively rather than as a full-width title-bar fill;
- softer, rounded controls and menus;
- more whitespace than older Office versions;
- compact monoline Fluent iconography;
- low-contrast borders and separators;
- subtle elevation for floating menus and overlays;
- restrained state changes based on neutral fills, strokes and small brand-colour accents;
- Segoe UI typography with compact desktop sizing.

The design should mimic the **recognisable composition and visual rhythm** of current Excel rather than reproducing every Microsoft control literally.

**How to read this file.** It is the style guide the agent worked from, written before the artwork existed, so it is deliberately broader than the report: it specifies states, tokens and controls the build never needed. Most of the chrome ended up as supplied artwork painted into the background, and the sections describing it (§9, §10, parts of §11–§19) record the look that artwork was drawn to, not live controls. What was actually built is in §8, §20–§20C and §24 here, and per visual in `visual_reference.md` §4.

The final report canvas is **2072 × 1020 px** and represents one maximised Excel-style window. *(Widened 1920 -> 2072 on 2026-09-21: measured from the publish-to-web full-screen render on a 1920 x 1080 screen, where the report was height-bound at scale 0.926 and left a 71 px white stripe each side. 1920 squared / 1778 = 2073.4; 2072 is the 4 px-grid value. The extra 152 px is inserted at the RIGHT -- left chrome is untouched, right chrome and the column-letter row shift +152.)*

---

## 2. Reference configuration

Use this as the visual baseline:

- Product: Microsoft Excel for Microsoft 365 desktop
- Platform: Windows 11-style desktop interface
- Office theme: White / light
- Ribbon: fully expanded
- Ribbon layout: current simplified Fluent-style visual treatment
- Window state: maximised
- Zoom/scaling assumption for source artwork: 100%
- Report theme: light only

The background artwork follows the Excel build used by the author, so its dimensions and colours match that build. Microsoft 365 UI details can change by update channel.

---

## 3. Differences from older Excel designs

Do not carry these older conventions into the report:

- no solid dark-green title bar spanning the full window;
- no large blocks of saturated green across the application chrome;
- no sharp rectangular buttons as the default control treatment;
- no dense ribbon with heavy separators and tightly packed icons;
- no bevelled controls, gradients or Windows 7-era shadows;
- no dark outlines around every control;
- no multi-colour legacy icon set;
- no large uppercase labels;
- no permanent Quick Access Toolbar dropdown arrow unless deliberately included;
- no old-style square File tab connected to a heavily coloured title bar.

Current Excel should feel lighter, calmer and more spacious.

---

## 4. Design principles

### 4.1 Neutral-first application chrome

The current Office visual refresh relies primarily on neutral surfaces. Excel green identifies the product and communicates active or selected states, but it should not dominate every region.

### 4.2 Four-pixel spacing system

Use a 4 px base grid. Preferred spacing values:

- 2 px: optical adjustment;
- 4 px: tightly related elements;
- 6 px: compact icon/text adjustment;
- 8 px: standard internal padding;
- 12 px: standard control/group gap;
- 16 px: major group separation;
- 20–24 px: large content separation;
- 32 px: major layout break.

### 4.3 Soft geometry

Use subtle corner radii rather than completely square controls:

- compact input/control: 4 px;
- ribbon button hover/selected surface: 4 px;
- dropdown slicer: 4–6 px;
- tooltip: 8 px;
- floating menu/callout: 8 px;
- large content card: 8 px maximum;
- sheet tabs: 4 px on exposed top corners only;
- File Backstage overlay: mostly planar; do not turn it into a rounded modal card.

### 4.4 Low visual noise

Prefer spacing and alignment over visible boxes. Use borders only where they clarify control boundaries or separate application regions.

### 4.5 State must not depend on colour alone

Selected controls should combine at least two of:

- neutral or pale-green fill;
- stronger text weight;
- green indicator/stroke;
- icon change;
- visible check/selection marker.

---

## 5. Typography

### 5.1 Font family

Primary interface font:

- **Segoe UI**

Fallback order:

1. Segoe UI
2. Segoe UI Variable
3. Arial
4. sans-serif

Use the same family throughout the simulated Excel chrome and report content unless a specific visual requires otherwise.

Do not distribute or embed font files.

**The ribbon and chrome text painted into `background.png` is Segoe UI Semilight** *(recorded
2026-09-17)*. It is baked into the artwork, not set by any visual, so nothing in the report layer
restates it and nothing in the report layer can change it — it moves only when the background is
re-shipped. Any live text authored to sit alongside the painted ribbon labels must match that
weight rather than the Regular used everywhere else, or it will read as a different font at the
same size.

### 5.2 Typography behaviour

- use sentence case;
- left-align long content;
- centre only short ribbon labels, icon captions and compact tab labels;
- use semibold sparingly for selected states and headings;
- avoid heavy bold text in the application chrome;
- maintain clear baseline alignment;
- do not use monospace for the formula bar unless explicitly desired—the real Excel UI uses the normal UI font.

### 5.3 Recommended sizes

Power BI may interpret font sizes differently from pixel dimensions. Treat these as visual targets and adjust after rendering.

| Element | Target size | Weight |
|---|---:|---|
| Quick Access / title-bar labels | 11–12 px | Regular |
| Ribbon tab names | 12 px | Regular; Semibold active |
| Ribbon command captions | 10–11 px | Regular |
| Ribbon group labels | 9–10 px | Regular |
| Dropdown values | 11 px | Regular |
| Formula bar | 11–12 px | Regular |
| Name box | 11 px | Regular |
| Sheet tabs | 11 px | Regular; Semibold active |
| Main content heading | 22–26 px | Semibold |
| Secondary content heading | 15–18 px | Semibold |
| Body text | 11–13 px | Regular |
| Bullet text | 11–12 px | Regular |
| Panel title | 13–14 px | Semibold |
| Panel metadata | 9–10 px | Regular |

**As built:** sheet titles 18 pt Semibold; matrix and table text 13; Exec summary prose 14; overlay headings 22 pt Semibold, overlay body 14 pt, overlay subheadings 16 pt Semibold; filter dropdown values 10; detail panel per §20.1.

---

## 6. Fluent 2 colour tokens

The palette below is a project-specific light-theme approximation aligned with Fluent 2 neutral-first principles and current Excel. Final artwork should be matched to the reference Excel build (§2).

### 6.1 Neutral surfaces

| Token | Hex | Use |
|---|---|---|
| `neutral_background_1` | `#FFFFFF` | Primary chrome and canvas |
| `neutral_background_2` | `#FAFAFA` | Secondary chrome / worksheet surround |
| `neutral_background_3` | `#F5F5F5` | Sheet-tab rail, subtle grouped surfaces |
| `neutral_background_4` | `#F0F0F0` | Pressed or stronger neutral state |
| `neutral_background_hover` | `#F5F5F5` | Standard hover fill |
| `neutral_background_pressed` | `#EBEBEB` | Pressed state |
| `neutral_background_selected` | `#EDEDED` | Neutral selected state |
| `neutral_overlay_scrim` | `#00000052` | Use only if a true dimmed overlay is required |

### 6.2 Neutral strokes

| Token | Hex | Use |
|---|---|---|
| `neutral_stroke_1` | `#D1D1D1` | Input and slicer outlines |
| `neutral_stroke_2` | `#E0E0E0` | Region borders |
| `neutral_stroke_3` | `#EBEBEB` | Very subtle separators |
| `neutral_stroke_accessible` | `#707070` | High-contrast focus/essential boundary |
| `neutral_divider` | `#E5E5E5` | Ribbon and formula-bar separators |

### 6.3 Text and icon neutrals

| Token | Hex | Use |
|---|---|---|
| `neutral_foreground_1` | `#242424` | Primary text/icons |
| `neutral_foreground_2` | `#424242` | Secondary controls |
| `neutral_foreground_3` | `#616161` | Metadata/group labels |
| `neutral_foreground_4` | `#707070` | Tertiary text |
| `neutral_foreground_disabled` | `#BDBDBD` | Disabled commands |
| `neutral_foreground_on_brand` | `#FFFFFF` | Text/icons on green brand surfaces |

### 6.4 Excel brand accents

Use Excel green only where it carries product identity or state.

| Token | Hex | Use |
|---|---|---|
| `excel_brand_primary` | `#107C41` | Current Excel product/brand accent |
| `excel_brand_hover` | `#0E6F3A` | Brand hover |
| `excel_brand_pressed` | `#0B5C31` | Brand pressed |
| `excel_brand_dark` | `#185C37` | Strong Backstage rail / selected brand area |
| `excel_brand_tint_10` | `#E9F5EE` | Selected control background |
| `excel_brand_tint_20` | `#D3EBDD` | Stronger selected background |
| `excel_brand_tint_30` | `#B9DFC9` | Optional highlighted border/fill |
| `excel_brand_link` | `#0F6CBD` | Fluent-style links where blue is appropriate |

Do not use the older `#217346` as the default full title-bar colour. It may appear only in imported artwork or where the reference build requires it.

### 6.5 Semantic colours

Use only for real semantic meaning, never decoration.

| Token | Hex | Use |
|---|---|---|
| `semantic_success` | `#107C10` | Success/status |
| `semantic_warning` | `#F7630C` | Important warning |
| `semantic_caution` | `#FCE100` | Caution indicator |
| `semantic_error` | `#D13438` | Error/validation |
| `semantic_information` | `#0F6CBD` | Help/information |
| `todo_verify_fill` | `#FFF4CE` | Optional background for `<TODO: VERIFY!>` content |
| `todo_verify_text` | `#8A6D1D` | Optional TODO text |

### 6.6 Worksheet table style

The sheet matrices and the Additional table are formatted like an Excel table in the blue style, not in brand green.

| Token | Hex | Use |
|---|---|---|
| `table_header_fill` | `#0F9ED5` | Column-header row, white text |
| `table_band_fill` | `#CFECF7` | Alternate (banded) rows; the other band is white |

---

## 7. Elevation and shadows

Current Fluent interfaces use restrained elevation. Windows-style controls often rely on strokes rather than obvious shadows.

### 7.1 Flat regions

Do not apply shadows to:

- title/QAT region;
- ribbon selector row;
- Home ribbon;
- formula bar;
- sheet-tab strip;
- main worksheet/content surface.

Use 1 px neutral separators instead.

### 7.2 Floating regions

Use subtle elevation for:

- dropdown menus;
- hover cards;
- small floating callouts.

Recommended Power BI approximation:

- shadow colour: `#000000`;
- opacity: 12–16%;
- blur: 8–16 px;
- vertical offset: 2–4 px;
- horizontal offset: 0 px.

Do not apply a large soft shadow to the File overlay. Excel Backstage behaves as a page-level surface, not a floating dialog.

---

## 8. Canvas geometry

Custom report page:

- width: **2072 px**
- height: **1020 px**

Use a 4 px grid.

### 8.1 Vertical zones

Measured from `background.png` on 2026-10-03.

| Zone | Y | Height | Notes |
|---|---:|---:|---|
| Quick Access / title / window controls | 0 | 60 | `#F0F0F0`; one surface with the tab row, so the split at 60 is nominal |
| Ribbon tab row | 60 | 38 | File and Home live; the other tab names are painted grey |
| Home ribbon | 98 | 124 | White rounded card with a soft shadow beneath |
| Formula bar | 222 | 64 | Name box, cancel / confirm / fx glyphs, formula box |
| Column-letter band | 286 | 29 | `#F0F0F0`, border at y 316; per-sheet band artwork (§20B) |
| **Main content (white grid)** | **315** | **638** | Sheet-specific content |
| Sheet tabs | 953 | 41 | Rule at 953, `#F0F0F0` strip 954–992, rule at 993 |
| Status bar | 994 | 26 | `#F5F5F5`; "Ready", selection summary, view icons |

*(Until 2026-10-03 this table carried the planning heights of the original 1536 × 816 layout — 30 / 30 / 112 / 36 / 107, with a 19 px scrollbar gutter above a 48 px tab strip. They never matched the artwork above y 315 or below y 953; the main-content row was always measured.)*

**Main content is `x 35, y 315, 2037 × 638`,** measured from `background.png`, re-measured
2026-09-21 after the widening.
The `208 / 764` row in earlier drafts was a planning estimate that overlapped the formula-bar
visual (238–298) and the column-header band; no sheet content may sit above y 315. Standard
inner padding is 24 px on all sides → usable box `x 59, y 339, 1989 × 590`. See §20A.1.

### 8.2 Horizontal margins and alignment

- external margin: 0 px for application chrome;
- content margin inside main worksheet: **24 px** (settled 2026-09-03, see §20A.1);
- ribbon group internal padding: 8–12 px;
- group gap: 12–16 px;
- standard icon/caption gap: 4 px;
- standard control gap: 8 px.

---

## 9. Quick Access Toolbar and title region

Everything in this region except the workbook title is painted into `background.png`. §9.4–§9.6 describe the look of that artwork; none of it is a live control, so the hover and disabled states listed there are not implemented.

### 9.1 Surface

Use:

- fill: `neutral_background_1` or `neutral_background_2`;
- bottom stroke: none or `neutral_stroke_3`;
- no full-width green background.

This is a major change from older Excel design.

### 9.2 Included items

The background artwork contains:

1. the project's own cell-grid app icon (§9.3);
2. AutoSave control, shown Off;
3. New icon;
4. Save icon;
5. window buttons: minimise, restore and close.

Project-specific exclusions:

- no workbook title in the artwork — the title is a live DAX-driven card, `txt_title_main`;
- no central search box or search icon;
- no warning icon;
- no user/avatar icon;
- no additional QAT commands;
- no QAT dropdown.

The New and Save icons and the window controls are muted neutral grey.

### 9.3 App icon

- the Excel product logo is **not** used anywhere in the report: it was painted out of both backgrounds on 2026-10-02;
- the title bar and the File overlay carry the project's original cell-grid icon, sources in `_input/_images/original/logo/`;
- displayed size approximately 20 × 20 px;
- the Recent row on the About pane uses the matching cell-grid file icon;
- do not reintroduce Microsoft's product logo.

### 9.4 AutoSave

Represent it as a compact modern control:

- small label;
- neutral text;
- subtle switch/toggle;
- low-emphasis state matching the reference build;
- 4–6 px corner radius;
- avoid the large old Office toggle style.

### 9.5 QAT command icons

- Fluent monoline appearance;
- 16 px icon within a 28–32 px command area;
- icon colour: `neutral_foreground_3`;
- disabled icon colour: `neutral_foreground_disabled`;
- hover surface: `neutral_background_hover`;
- 4 px corner radius.

### 9.6 Window controls

Place at the far right:

- minimise;
- layout/restore;
- close.

Use 12–14 px neutral icons in approximately 46 × 32 px hit regions.

For static background artwork, use grey neutral icons. Do not show a red close-button fill in the resting state.

---

## 10. Ribbon selector row

This row represents Excel's ribbon tabs.

Two entries are live:

- File
- Home

The artwork also paints Excel's other tab names (Insert, Draw, Page Layout, Formulas, Data, Review, View) in disabled grey. They are decoration and not clickable.

File and Home are supplied artwork (base and hover PNGs on `btn_ribbon_file` and `btn_ribbon_home`, §10.4). §10.1 and §10.2 describe the look that artwork was drawn to.

### 10.1 File

- clickable Power BI button;
- left aligned;
- Excel brand text or selected-like treatment;
- avoid the old large dark-green rectangular File tab;
- resting state may use green text on a neutral background;
- hover may use `excel_brand_tint_10`;
- 4 px rounded hover region.

File opens the Backstage-style overlay.

### 10.2 Home

- default and visually active;
- not clickable;
- primary text colour;
- semibold or regular with a small green underline/indicator;
- indicator height: 2 px;
- do not fill the whole tab with green.

### 10.3 Other tab names

Painted into the background in disabled grey, right of Home, for a believable tab row. No visual sits over them.

### 10.4 Dimensions

- row height: 38 px (y 60–98);
- as built: `btn_ribbon_file` `20,73 / 41×23` and `btn_ribbon_home` `70,73 / 67×23`, each with base and hover artwork;
- hover radius: 4 px.

---

## 11. Home ribbon

### 11.1 Surface

- background: `neutral_background_1`;
- bottom separator: 1 px `neutral_divider`;
- no heavy outer border;
- no gradient;
- no strong green fill;
- height: 124 px (y 98–222), a rounded card with a soft shadow beneath.

The modern ribbon should feel like one continuous light surface.

### 11.2 Ribbon groups

Groups:

1. View
2. Detail Level
3. Perspective
4. Filters
5. Other — Clear Filters, Help

Group labels sit at the bottom of each group in small neutral text.

**As-built separator geometry**, measured from `background.png` on 2026-09-21 (the pipes are
painted, not drawn by visuals). Group interiors run between consecutive pipes:

| Group | Pipes | Interior | Icons | Icon x |
|---|---|---:|---:|---|
| View | 9 · 189 | 180 | 2 | 30, 111 |
| Detail Level | 189 · 375 | 186 | 2 | 212, 293 |
| Perspective | 375 · 626 | 251 | 3 | 391, 472, 553 |
| Filters | 626 · 1162 | 536 | — (4 slicers + 4 icons) | see §15 |
| Other | 1162 · **1347** | **185** | 2 | **1184, 1265** |

Icons are 60 × 74 at **pitch 81** (60 + 21 gap), centred in the interior. Other was widened
89 → 185 on 2026-09-21 to take Clear Filters beside Help; at 185 it matches Detail Level's 186,
so both use 22 px side margins. Keep new commands on this pitch.

Use subtle vertical separators only where needed:

- colour: `neutral_stroke_3`;
- height: 64–72 px;
- width: 1 px;
- do not box each group.

### 11.3 Command layout

As built:

- icon-above-label commands, 60 × 74 artwork, for View, Detail Level, Perspective and Other;
- Excel-style dropdown selectors for Filters.

Large commands should still be visually compact. Modern Excel uses clean iconography and whitespace rather than heavy button chrome.

### 11.4 Hover and selected states

Default:

- transparent or white surface;
- neutral foreground.

Hover:

- `neutral_background_hover`;
- 4 px radius.

Pressed:

- `neutral_background_pressed`.

Selected/toggled:

- the ribbon slicers paint no selected surface; hover marks the unselected option as the clickable alternative (§12);
- sheet tabs and Backstage rail items show selection through their `_select` / `_selected` artwork.

---

## 12. Ribbon group — View

Purpose:

- toggle Story mode / List mode.

As built:

- two icon-above-label commands, Story and List, `60 × 74` at x 30 and 111, y 117 (§11.2);
- one single-select tile slicer, `sli_ribbon_view` on `tbl_view_modes`, sits over both icons as the hit target: transparent tiles, 4 px radius, `#F5F5F5` hover at 75% transparency;
- the slicer paints nothing on the selected option. Hover highlights only the other option, the one clickable alternative, and that is the state cue; the formula bar also reflects the current mode;
- each icon has a `_disabled` artwork variant, shown where the group greys (`context.md` §5.4).

The toggle changes only text representation. It must not change grouping, perspective or filters.

---

## 13. Ribbon group — Detail Level

Purpose:

- toggle Compact / Standard content density.

Same pattern as §12 View, so the two axes feel related and behave identically:

- two icon-above-label commands, Standard and Compact, `60 × 74` at x 212 and 293 (artwork brief: `_context/brief_ribbon_icons.md`);
- hit target `sli_ribbon_detail` on `tbl_detail_levels`, same tile treatment;
- `_disabled` variants where the group greys.

Detail Level changes only content density. It must not change grouping (Perspective), text mode (View), or filters. On sheets where it does not apply, follow the greyed-command treatment in `_context/context.md` §5.4.

---

## 14. Ribbon group — Perspective

Options:

- Timeline
- Skills
- Industries

Recommended design:

- three icon-above-label commands;
- `60 × 74` artwork at x 391, 472 and 553, under the `sli_ribbon_perspective` hit target;
- compact captions;
- same tile treatment as View / Detail Level.

Suggested Fluent metaphors:

- Timeline: timeline/calendar;
- Skills: spark/star/badge;
- Industries: building/factory.

Perspective changes grouping/order only. It is not bookmark-driven.

---

## 15. Ribbon group — Filters

Filters:

- Function
- Industry
- Skill
- Tool

These must mimic the current Excel font-family dropdown more than a standard dashboard slicer.

### 15.1 Control appearance

As built, each control is a Power BI dropdown slicer, `210 × 32`, in a 2 × 2 block (x 672 and 926, y 120 and 159):

- the slicer's container background, border, header and title are off;
- Segoe UI 10, `#242424`, item padding 4;
- compact one-line current value;
- no visible Power BI visual header.

### 15.2 Label treatment

The dropdowns carry no caption, as Excel's font selector carries none. A 16 × 16 icon left of each dropdown names its group on hover (an inert button over the icon supplies the tooltip), and the group caption "Filters" is painted beneath.

### 15.3 States

Hover:

- slightly darker stroke;
- `neutral_background_hover` in chevron area.

Focused/open:

- 2 px or visually stronger Excel-green focus stroke;
- floating dropdown menu with 8 px corner radius and subtle elevation.

Disabled/no applicable values:

- disabled foreground and pale neutral fill; as built, `slicer_all_disabled.png` covers each slicer and a `_disabled` icon replaces its filter icon where Filters grey.

### 15.4 Clear state

There is no per-dropdown clear icon. **Clear Filters** is a command in the Other group (§11.2) that resets all four slicers, and it greys with Filters.

---

## 16. Formula bar

### 16.1 Surface

- background: `neutral_background_1`;
- top and bottom separators: `neutral_divider`;
- height: 64 px (y 222–286);
- no shadow.

### 16.2 Name box

- the box is painted in the background; the live text is `vis_name_box` at `18,238 / 140×40`;
- 4 px radius;
- fill: white;
- short DAX-driven cell reference;
- examples: `A1`, `B4`, `C2` — never a sheet prefix (`context.md` §6).

Include a subtle dropdown chevron only if it matches the final reference image. It does not need to be functional.

### 16.3 fx control

The cancel, confirm and `fx` glyphs are painted into the background in neutral grey, as in Excel. They are static: no visual sits over them and they carry no tooltip. The formula bar is read-only.

### 16.4 Formula display area

- no heavy input border;
- use the formula-bar region's top/bottom separators;
- 8–12 px left padding;
- primary or secondary neutral text;
- one line with ellipsis;
- DAX-driven;
- no typing cursor;
- as built: `vis_formula_bar` at `288,238 / 1760×60`.

---

## 17. Main content / worksheet region

### 17.1 Surface

- main fill: `#FFFFFF`;
- optional worksheet surround: `neutral_background_2`;
- sheet lists are formatted as an Excel table (§6.6): blue header row, banded rows, no gridlines;
- avoid a visible card dashboard appearance;
- use alignment and whitespace to evoke worksheet organisation.

### 17.2 Worksheet cues

Permitted:

- faint column/row header cues;
- sparse 1 px rules;
- subtle cell-like alignment;
- content blocks aligned to a regular grid;
- light grey region labels.

Avoid:

- drawing a dense grid over the entire page;
- heavy Excel cell borders around all content;
- coloured KPI cards;
- generic Power BI dashboard tile layouts.

### 17.3 Story mode

- running prose;
- line length approximately 70–90 characters;
- body text 13 in the matrices, 14 on Exec summary;
- employer/role/project hierarchy clearly separated;
- 12–16 px paragraph spacing;
- minimal containers.

### 17.4 List mode

- reserved text area;
- word-wrapped bullets;
- consistent hanging indent;
- 4–6 px between bullets;
- stable layout that does not reflow other chrome;
- no visual regrouping caused by mode.

### 17.5 Selection and highlight

Nothing is dimmed. Filters remove non-matching rows and the sheet counter states the subset (§20A.2); Perspective regroups rows. Two highlights exist, both driven by `is_highlight` in the source:

- the row tint on Additional;
- the Exec summary highlight cell: pale green fill, green border (§20C).

Avoid saturated fills.

---

## 18. Sheet-tab strip

### 18.1 Strip surface

- fill: `#F0F0F0`;
- top border: `neutral_stroke_2`;
- height: 39 px (y 954–993), with the `#F5F5F5` status bar beneath (y 994–1020);
- no dark green footer bar.

### 18.2 Sheet tabs

Tabs:

1. Exec summary
2. Experience
3. Projects
4. Tech stack
5. Skills
6. Education
7. Additional

Inactive tab:

- transparent or `neutral_background_3`;
- neutral foreground;
- no heavy border;
- subtle hover fill;
- 4 px exposed top-corner radius.

Active tab:

- white fill;
- primary text;
- semibold label;
- subtle neutral outline;
- 2–3 px Excel-green top indicator;
- visually connected to the white worksheet surface.

### 18.3 Tab artwork

As built, each tab is three PNGs in `_input/_images/excel_derived/tabs/`: `_base`, `_hover` and `_select`.

- `btn_sheet_*` carries base and hover and fires the sheet bookmark;
- `img_sheet_*_select` shows the selected state for the active sheet;
- all sit at y 954, 39 high, from x 140, and each tab's bounding box is fixed.

---

## 19. File Backstage overlay

There is one navigable overlay, opened by File (plus the first-load dialog — `context.md` §8A).

It should resemble current Excel Backstage: a page-level replacement over the workbook, not a small modal window.

### 19.1 Overall composition

- full report-page coverage;
- white/light main content;
- left navigation rail;
- no translucent background showing the workbook through it;
- back arrow at top left;
- subtle entrance is optional, but no fake multi-step animation is required.

### 19.2 Back arrow

- circular or rounded-square hover region;
- 20 px left-arrow Fluent icon;
- neutral or white icon depending on rail treatment;
- as built: `btn_overlay_close` at `17,73 / 30×30`, tooltip "Back to the workbook";
- closes overlay without resetting sheet/ribbon state.

### 19.3 Left rail

As built, a light rail, matching the current white Backstage rather than the older green one:

- `#F0F0F0`, x 0–199 (200 px wide), neutral text;
- menu items: About, Contact, Help, plus **Special Thanks** bottom-anchored in the rail. The artwork reads "Special Thanks", a deliberate exception to the sentence-case rule in §5.2;
- each item is a `198 × 50` button with base and hover artwork, plus an `img_overlay_*_selected` image for the open pane.

### 19.4 Main overlay content

- white surface;
- large heading, 22 pt as built;
- concise supporting content;
- 16-24 px section spacing;
- modern neutral text;
- links in Fluent blue or Excel green;
- avoid heavy cards.

#### As-built pane grid (2026-09-22)

Measured from `overlay_background.png`, not estimated. The rail is `#F0F0F0` at `x 0-199`;
the content pane is `#F5F5F5` at `x 200, y 60, 1872 x 960`, under a full-width `#F0F0F0`
title strip at `y 0-59`.

```
heading        264, 116   1628 x 58   22pt Segoe UI Semibold  #242424
column A       264, 186   754 wide    primary content
column B      1138, 186   754 wide    secondary register
body text      14pt  #242424   ( #616161 for column B )
subheadings    16pt  Segoe UI Semibold  #242424
```

Left margin from the rail is 64 px, the gutter 120 px and the right margin 180 px. The heading
sits level with the first rail item, `btn_overlay_about` at `y 116`.

About uses both columns. Special Thanks is a single column A; Contact and Help are deliberately
off this grid (`_context/visual_reference.md` §4.1a).

**Hyperlinks:** a link inside text is a textbox run with a run-level `url`, coloured
`excel_brand_link`. A non-text target (the Exec highlight cell, the Contact rows) takes a
transparent `WebUrl` action button over it.

---

## 20. Role/project detail panel

A task-pane-style panel right of the Experience and Projects matrices (`context.md` §7). It is flat worksheet surface (white on the white grid, **no border, no shadow**), so it reads as part of the sheet rather than as a floating card.

### 20.1 Geometry (as built 2026-10-02, identical on both sheets)

The matrices are `59,415 / 1420 × 514` (second column 180, text column 780). The panel's title card is top-aligned with the matrix at y 415.

| Element | Position / size | Type |
|---|---|---|
| frame | 1495,403 / 553 × 544 | white, borderless |
| title | 1511,415 / 521 × 28 | 14D `#242424` |
| employer · period | 1511,445 / 521 × **44** | 10D `#616161` |
| industry · type · location | 1511,467 / 521 × 44 | 10D `#616161` |
| radar (`pivotTable`) | 1503,462 / 537 × 320 | image 470 × 283, column 478 |
| tools | 1511,779 / 521 × 64 | 11D `#242424` |
| highlight | 1511,827 / 521 × 68 | 11D `#242424` |
| footnote | 1511,879 / 521 × 60 | italic 10D `#616161` |
| empty-state prompt | 1511,653 / 521 × 24 | 11D `#616161`, centred |
| override (Experience) | 1511,467 / 521 × 60 | 11D `#242424` |
| cover button | 1495,403 / 553 × 544 | transparent, no action |

- **The radar overlaps the industry line on purpose.** Its drawing starts well below its box top (the painted-out header row plus the viewBox's top band), so the box sits under the text cards in z-order and the drawing starts ~20 px below the line.
- **Text gaps are ~25 px** between tools, highlight and footnote. Cards overlap where their text leaves room; later z sits on top.
- Sized for the current roles and projects at 521 px; type and heights grew on 2026-10-02, positions did not move. Re-check when `role_summary_short` / `project_summary_short` or tool lists change.
- The period card is 44 px, not 20: an empty 20 px card paints Power BI's grey no-data block (`visual_reference.md` §3.33).
- The `pivotTable` needs ≥ 44 px horizontal and ≥ 34 px vertical slack or it grows scrollbars (`visual_reference.md` §3.36).

### 20.2 Radar chart

- scale: 1–7; up to 8 axes (4 Technical + 4 Soft); one profile only;
- subtle neutral grid; Excel-green line and low-opacity fill;
- no legend block: axis labels carry the naming;
- blank scores must not plot.

**Axis labels** (approved treatment, reference `_wip/radar_axis_demo.py`): each axis is named by a label just beyond the outer ring on that axis's own angle. Anchor at radius `R + 17`; horizontal anchor `start` right of centre, `end` left, `middle` within 4 px of the vertical axis; `dy` +4 normally, +10 at the bottom vertex, −3 at the top; labels above 20 characters wrap to two lines at the space nearest the midpoint.

**DAX SVG measure `sel_radar_svg`**, hosted in a `pivotTable` cell. Power BI has no native radar; the SVG route keeps the build agentic-only and reproduces the label geometry exactly.

```
viewBox   21 45 478 304 (base)    centre 260,196    R 105
floor     1-7                     radius = R * (rel - 1) / 6
labels    R + 17, rgb(66,66,66)   10 base, 12 in the panel (pnl_radar_svg), tspan dy 11 -> 13
panel     viewBox 8 45 504 304 so 12 px labels fit (x 12..505 across all current items)
grid      rgb(224,224,224), outer ring rgb(200,200,200)
data      rgb(16,124,65), fill-opacity 0.14, stroke-width 1.75, 3 px vertices
```

Colours are `rgb()` and coordinates integers, both for hard technical reasons; see `visual_reference.md` §3.27 before changing anything here. **Re-run the label-extent simulation before changing R, the centre, the viewBox or the font size.**

---

## 20A. Sheet layout — Projects

**Approved 2026-09-03.** First sheet designed, and the template for the format. The pattern
below (heading, filter-state counter, one repeating-row visual, deliberate right-hand
whitespace) is the shape the other sheets follow (§20C).

### 20A.1 The drawable area

All sheet content lives inside the white grid rectangle painted by `background.png`:

```
grid box      x  35, y 315   2037 × 638
padding       24 px, all four sides
inner box     x  59, y 339   1989 × 590     right edge 2048, bottom edge 929
```

Measured from the artwork on 2026-09-03. The `208 / 764` figure in older drafts of
`context.md` §4 was an estimate and overlapped the formula bar; it has been corrected.
24 px is the §4.2 "large content separation" step — 16 px reads tight at this scale, 32 px
wastes vertical budget that the scrolling list wants.

### 20A.2 Objects

| Object | `X,Y / W,H` | Type | Treatment |
|---|---|---|---|
| `txt_projects_title` | `59,339 / 300,44` | textbox | "Projects" · Segoe UI 18pt Semibold · `neutral_foreground_1`. Height 44 because the glyphs bottom-crop below it |
| `vis_projects_count` | `59,381 / 500,18` | cardVisual | "N of M projects · grouped by X" · Segoe UI `9D` · `neutral_foreground_3`. Container-title binding per `visual_reference.md` §3.11 |
| `vis_projects_matrix` | `59,415 / 1420,514` | pivotTable | Bottom edge lands exactly on 929 |

**As-built, verified against disk 2026-09-09.** The `59,339 / 300,32` · `59,373 / 500,18` ·
`59,407 / 1120,522` figures in earlier drafts were the pre-build design estimate and never
matched the report.

The counter is the sheet's filter-state signal: when a Function / Industry / Skill / Tool
selection is active, "4 of 9 projects" tells the reader immediately that they are looking at
a subset. This is the sheet-level counterpart to the formula bar's filter term (§6 of
`context.md`), and it is why the counter sits directly under the heading rather than in a
corner.

### 20A.3 Matrix internal split

Total 1420 px, four columns summing to 1400. The 20 px of slack keeps the vertical
scrollbar from forcing a horizontal one.

| Column | Bound to | Width |
|---|---|---:|
| Group | `tbl_project_perspective_rows.group_label` | 160 |
| Project | `tbl_project_perspective_rows.project_identity` | 180 |
| Period, industry and function | `_measures.prj_key_facts` | 280 |
| *(blank header)* | `_measures.prj_body_text` | 780 |

Identity sits left of the prose rather than stacked above it. A pure vertical card would
force the matrix down to about 700 px wide and strand 1200 px of empty canvas; the
two-column shape uses the width without pushing line length past readability.

The body column is 780 px at 13, kept short of the full inner width so line length stays
readable; the detail panel (§20) takes the rest.

*(Widths were 160 / 260 / 220 / 760 before the detail panel arrived on 2026-09-26. The union
table supplies the grouping — see `visual_reference.md` §3.19.)*

### 20A.4 Right-hand space

On Projects and Experience the matrix ends at x 1479 and the detail panel (§20) fills
x 1495–2048, so the inner box is used edge to edge.

On the sheets without a panel the matrix or table stops short of the inner right edge at 2048
(Skills at 1079, Education at 1499, Additional at 1519, Tech stack at 1779), and the white
beyond it is deliberate. A real worksheet holds its data in the left columns and leaves the
rest of the grid empty, so the whitespace reads as authentic rather than as a gap. Do not fill
it to "balance" the layout.

### 20A.5 Scrolling is intended

The matrix is 514 px high and the full Story / Standard text does not fit it, so the list
scrolls. Confirmed intentional by the user 2026-09-03. It is Excel-authentic — worksheets
scroll — and it gives Compact a real job on this sheet rather than a decorative one.

### 20A.6 Row-header typography

Matrix font family and size apply to row headers as a whole, not per level; only font colour
and background accept conditional formatting per row. The shipped identity block is therefore
plain 13 Segoe UI row-header text, and the key-facts line is plain separated text
(`period · industry · function`). There are no chips and no SVG identity column.

---

## 20B. Column-letter band — per-sheet geometry

**The one place column widths are recorded.** When a sheet's layout is fixed, add its row here
and generate its band artwork to match; nothing else needs to know the numbers.

### 20B.1 Where the band sits

```
painted band      y 286 - 315, bottom border #ABABAB at y 316
letter glyphs     y 300 - 311
grid starts       y 317          row-header column ends x 36, column A starts x 37
```

Each sheet's artwork is one image visual, `img_grid_header_<sheet>`, placed at:

```
x 36, y 293, 2033 x 23
```

23 px tall because the asset carries its **own** `#ABABAB` bottom border on its last row, and
its glyphs sit on rows 6-17. Placed here they land on y 299-310 with the border on y 315 —
one pixel above the letters painted into `background.png` (y 300-311, border y 316). **That
one-pixel lift is deliberate**, set by eye on a real render 2026-09-12; do not "correct" it
back. The asset is opaque `#F0F0F0`, the band's own fill, so it covers the painted letters
rather than blending with them.

Width 2033 runs from x 36 to x 2068.

**One visual per sheet, toggled by the sheet bookmarks** — seven, all shipped. The band is
sheet-specific because column widths differ per sheet. Experience and Projects share one asset.

### 20B.2 As built — all seven sheets

Separator positions measured from the shipped assets on 2026-10-03, in asset x (canvas x =
asset x + 36). Column width in brackets. Every asset is 2033 × 23 with its border at 2032.

| Sheet | Asset | Columns |
|---|---|---|
| Exec summary | `grid_header_exec.png` | A 22 (23) · B 310 (288, portrait) · C 366 (56, gap) · D 1538 (1172, prose) · E 1563 (25, empty) · F 2011 (448, the highlight cell) · G to 2032 (21) |
| Experience / Projects | `grid_header_projects_exp.png` | A 30 (31) · B 190 (160, group) · C 370 (180, role or project) · D 650 (280, key facts) · E 1442 (792, body) · F 1474 (32, empty) · G 1995 (521, the detail panel) · H to 2032 (37) |
| Tech stack | `grid_header_tech.png` | A 30 (31) · B 290 (260, tool) · C 510 (220, group) · D 650 (140, proficiency) · E 870 (220, usage) · F 1742 (872, body) · G–I every 88 to 2006 · J to 2032 (26) |
| Skills | `grid_header_skills.png` | A 30 (31) · B 350 (320, skill) · C 490 (140, type) · D 630 (140, proficiency) · E 1042 (412, usage) · F–P every 88 to 2010 · Q to 2032 (22) |
| Education | `grid_header_edu.png` | A 30 (31) · B 390 (360, education) · C 590 (200, key facts) · D 1442 (852, body) · E–J every 88 to 1970 · K to 2032 (62) |
| Additional | `grid_header_additional.png` | A 30 (31) · B 230 (200, section) · C 370 (140, year) · D 830 (460, item) · E 1390 (560, detail) · F 1462 (72, link) · G–L every 88 to 1990 · M to 2032 (42) |

### 20B.3 How the widths are derived

- **Column A is 31, not 22.** 22 px is the gap from the grid edge to a matrix's left edge at
  x 59; the other 9 px are the matrix's own inner left padding, the white space before the first
  character of a row header. The letters line up with the text a reader sees, not with the
  visual's bounding box, so every boundary after A carries that offset too.
- Each later boundary is cumulative from the visual's column widths.
- **The last content column takes the visual's 20 px scrollbar slack, minus the 8 px right inner
  padding** — 792 for a 780 body column.
- Empty worksheet columns are 88 wide, the last one partial.
- Education's body column (852) is deliberately narrower than that rule gives (user, 2026-09-24).
- **Exec summary's prose column lines up with the text, not the visual.** The prose is a matrix cell and a matrix's row header cannot be turned off, so the text starts right of the visual's left edge; the band's D column (canvas x 402–1574) matches the text exactly.
- The band sits one pixel above the painted letters on purpose (§20B.1).

---

## 20C. Sheet layouts — the other sheets

**As built, verified against disk 2026-10-03.** Every sheet follows the Projects rhythm (§20A):

```
sheet title       y 339   h 44    every sheet
subtitle row      y 381   h 18    reserved even where a sheet has no counter
content           y 415 - 929     starts on the same pixel on every sheet
```

| Sheet | Title | Subtitle | Content |
|---|---|---|---|
| Exec summary | "Exec summary" | *(empty, reserved)* | portrait `59,415 / 288×353` (container `43,399 / 320×385`) · prose cell `377,384 / 1180×568`, 14D, ending at y 952 · highlight cell `1600,415 / 448×150` |
| Experience | "Experience" | "N of 11 roles · grouped by X" | matrix `59,415 / 1420×514`, identical to Projects |
| Tech stack | "Tech stack" | "N of 17 tools" | matrix `59,415 / 1720×514` — Tool 260 · Group 220 · Proficiency 140 · Usage 220 · text 860 |
| Skills | "Skills" | "N of 15 skills" | matrix `59,415 / 1020×514` — Skill 320 · Type 140 · Proficiency 140 · Usage 400 (widened 300 → 400 2026-09-26 so Usage never wraps; scrolling is accepted), Tech stack styling; ordered Technical then Soft, each by proficiency then `skill_id` (`skill_sort_key`, M-derived) *(added 2026-09-25)* |
| Education | "Education" | counter | matrix `59,415 / 1440×514` — Education 360 · key facts 200 · body 860 |
| Additional | "Additional" | "N items · N sections" | **table** (`tableEx`) `59,415 / 1460×514` (columns + 20, as on the matrices), columns Section 200 · Year 140 · Item 460 · Detail 560 · Link 80 *(built 2026-09-23)* |

- **Exec summary has no headings inside the content** — the sheet title, then the portrait, the
  prose and the highlight cell (`context.md` §6A). The prose is one table cell (14D) rather than a card: it wraps, honours line
  breaks and scrolls. The Excel World Championship 2024 prominence lives in the prose itself,
  written by the user.
- **Tech stack lists all 17 tools** and leaves the text column blank where a tool has no written
  content. Columns: Tool · Group · Proficiency `n/7` · Usage (`17 years 4 months · 6 roles ·
  3 projects`) · text. Ordered by proficiency, then `tool_id`. Skills are not on this sheet: per-item
  relevance lives in the detail panel's radar, current proficiency on the Skills sheet.
- Right-hand whitespace stays deliberate, as on Projects (§20A.4).
- **Additional is a `tableEx`, not a matrix** — the only sheet that is. Reason: the
  `is_highlight` row tint must cover the whole row, and a matrix cannot conditionally format its
  row headers. Section repeats on every row (user, 2026-09-23: a once-per-block label read as
  missing data). `url` renders as a link icon (`values.urlIcon`).

---

## 21. Fluent iconography

### 21.1 Style

Use Fluent system icons:

- simple literal metaphors;
- monoline or consistent filled style;
- one icon family throughout;
- neutral single-colour treatment;
- no legacy glossy Office icons.

### 21.2 Size

- 12 px: informational only, not interactive;
- 16 px: compact ribbon controls and QAT;
- 20 px: standard ribbon command;
- 24 px: prominent command/back arrow;
- 32 px+: only for major File overlay navigation or feature illustration.

Interactive icons must sit inside larger hit areas.

### 21.3 Colour

- default: `neutral_foreground_2` or `neutral_foreground_3`;
- hover/selected: Excel green where meaningful;
- disabled: `neutral_foreground_disabled`;
- use one solid colour for system icons;
- the Excel product icon is not used (§9.3).

### 21.4 Placeholders

No placeholder icons remain. If an icon is ever missing, use a plain circle or square in its final bounding box, named `shp_placeholder_*` / `img_placeholder_*` (`context.md` §11).

---

## 22. Interactions and micro-states

### 22.1 Hover

Modern Fluent hover states are subtle:

- neutral fill;
- no large movement;
- no glow;
- no dramatic colour inversion;
- optional tooltip after standard Power BI delay.

### 22.2 Pressed

- slightly darker neutral fill;
- optional 1 px inward stroke;
- do not simulate large physical depression.

### 22.3 Selected

- pale green fill or small green indicator;
- semibold text;
- optionally green icon;
- preserve high contrast.

### 22.4 Focus

Where Power BI supports keyboard focus:

- use a visible high-contrast outline;
- do not rely only on pale green.

### 22.5 Motion

Power BI has limited animation. Do not create multiple bookmark frames merely to fake motion. State changes should be immediate and stable.

---

## 23. Accessibility

- standard body text contrast: at least 4.5:1;
- large text contrast: at least 3:1;
- do not place light grey text on white if it becomes unreadable;
- add alt text to meaningful visuals and controls;
- establish logical tab order;
- provide visible selected states;
- use labels in addition to icons;
- keep interactive hit areas approximately 28 × 28 px or larger;
- do not use colour alone for Detail Level, Perspective or filter state;
- test at Fit to Page and Actual Size.

---

## 24. Asset-production guidance

As built, all artwork is PNG under `_input/_images/`, split by provenance:

1. `excel_derived/` — artwork based on the Excel interface, excluded from the MIT licence:
   - `background/background.png` (2072 × 1020): title and QAT region, ribbon tab row, Home ribbon card with its group separators and captions, formula bar, row and column headers, sheet-tab rail and status bar;
   - `grid_header/`: the per-sheet column-letter bands (§20B);
   - `tabs/`: the sheet tabs and the File / Home tabs, in base, hover and selected states;
   - `overlay/overlay_background.png`.

2. `original/` — the project's own artwork:
   - `icons/`: ribbon command and filter icons with their disabled variants;
   - `logo/`: the cell-grid app and file icons (§9.3);
   - `overlay/`: the Backstage rail buttons;
   - `pictures/`: the Clippy figure.

Do not bake dynamic text into the background:

- workbook title;
- Name Box value;
- formula-bar text;
- ribbon slicer values;
- active sheet state;
- contact details.

Interactive targets are Power BI visuals and buttons over the background.

---

## 25. Naming conventions

Use `snake_case`.

Visual prefixes in use:

- `grp_` — group;
- `img_` — image;
- `shp_` — shape;
- `btn_` — button;
- `sli_` — slicer;
- `txt_` — text;
- `vis_` — data visual;
- `bm_` — bookmark.

Examples:

- `btn_ribbon_file`
- `sli_filter_function`
- `vis_formula_bar`
- `btn_overlay_close`
- `img_grid_header_projects`
- `txt_projects_title`
- `bm_sheet_experience`

---

## 26. Quality-control checklist

Before approving the visual build:

1. The top chrome is light/neutral, not a solid legacy green title bar.
2. Excel green is confined to the app icon and a few accents (the dialog's Continue button, the Exec highlight cell), never a large surface.
3. Ribbon controls use modern rounded hover surfaces.
4. Iconography is monoline and visually consistent.
5. The ribbon feels spacious but compact, not dense.
6. Borders are subtle and used selectively.
7. The formula bar looks read-only and contemporary.
8. Filter slicers resemble current compact Excel dropdowns.
9. Sheet tabs resemble current light-theme Excel tabs.
10. File behaves as a modern Backstage page.
11. Floating surfaces use controlled elevation and rounded corners.
12. All technical object names use `snake_case`.
13. No legacy gradients, bevels, glossy icons or heavy outlines remain.
14. No interactive text is permanently baked into the background.
15. The design remains legible at 2072 × 1020 and Fit to Page.

---

## 27. Official references

This specification is based primarily on current Microsoft guidance:

- Microsoft Support — The new look of Office  
  https://support.microsoft.com/en-us/office/foundations-experiences/the-new-look-of-office

- Fluent 2 Design System — Home  
  https://fluent2.microsoft.design/

- Fluent 2 — Design principles  
  https://fluent2.microsoft.design/design-principles

- Fluent 2 — Colour  
  https://fluent2.microsoft.design/color

- Fluent 2 — Typography  
  https://fluent2.microsoft.design/typography

- Fluent 2 — Layout and spacing  
  https://fluent2.microsoft.design/layout

- Fluent 2 — Elevation  
  https://fluent2.microsoft.design/elevation

- Fluent 2 — Iconography  
  https://fluent2.microsoft.design/iconography

- Microsoft Support — Visual refresh in Excel / Office  
  https://support.microsoft.com/en-us/excel/what-s-new-in-excel-2021-for-windows

- Microsoft 365 Insider — Quick Access Toolbar and the Microsoft 365 visual refresh  
  https://techcommunity.microsoft.com/blog/microsoft365insiderblog/quick-access-toolbar-on-by-default/4219349

The Microsoft documentation describes the general Fluent refresh rather than publishing every pixel and colour used by a specific Excel Current Channel build. Therefore, this document distinguishes between:

- Microsoft-published Fluent principles; and
- project-specific visual tokens chosen to approximate the current Excel desktop light theme.

For exact matching, the final SVG must be calibrated against the project owner's reference Excel build.
