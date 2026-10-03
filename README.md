# Interactive Power BI CV

<p>
  <img src="https://img.shields.io/badge/Power_BI-F2C94C?style=for-the-badge" alt="Power BI" />
  <img src="https://img.shields.io/badge/Excel-107C41?style=for-the-badge" alt="Excel" />
  <img src="https://img.shields.io/badge/Built_with-Claude_Code-D97757?style=for-the-badge&logo=claude&logoColor=white" alt="Built with Claude Code" />
  <img src="https://img.shields.io/badge/Licence-MIT-616161?style=for-the-badge" alt="Licence: MIT" />
</p>

A CV built as a Power BI report that looks and behaves like an Excel workbook.

**The live version, with my real CV, is at [adamreichenbach.com](https://adamreichenbach.com).**
This repository holds the same report with **dummy data** in place of my CV (see
[The data](#the-data)).

## Why it exists

It started as a proof of concept for a Fabric community DataViz contest and grew into a way of
showing how I work rather than just listing what I have done. The Excel look is deliberate: the
ribbon, the formula bar and the sheet tabs are the interface most finance and analytics people
already use every day, so the CV needs no legend.

It was also an experiment in agentic development. The semantic model, the Power Query and every
visual were written as files on disk by an AI agent (Claude Code). Nothing was edited by hand in
Power BI Desktop or the Power Query editor. Desktop was only used to look at the result, refresh
it and give feedback. That constraint made some things slower and everything more disciplined.

## What is in the report

One report page, seven "worksheets": Exec summary, Experience, Projects, Tech stack, Skills,
Education and Additional. On top of them:

- **Home ribbon**: Story or List wording, Compact or Standard detail, a Timeline / Skills /
  Industries perspective, and four dropdown filters. Commands grey out on sheets where they do
  not apply, as in Excel.
- **Formula bar**: read-only. It writes the current sheet state out as a real Excel formula, and
  the name box shows the selected cell.
- **Detail panel**: select a role or project to see its skills radar, main tools and a highlight.
- **File menu**: About, Contact, Help and Special Thanks, in the style of Excel's Backstage view.

Under the hood: one Excel workbook as the only source, a star schema with dimension, content and
bridge tables, DAX measures for all state-dependent text, and bookmarks for navigation only. The
ribbon state and the perspective do not use bookmarks at all.

## The data

`_input/CV.xlsx` in this repository is a **dummy**. It has exactly the structure of my real
workbook, every table, column, row and ID, but not its content:

- **Personal and identifying text is replaced by bracketed placeholders**: names, employers,
  institutions, locations, contact details, URLs, achievements and all prose.
- **Everything else descriptive is invented**: role titles, project names, industries,
  functions, skills, tools and so on. It reads plausibly, but it is not my career.
- **All 1-7 scores are random.** The radar and the `n/7` ratings will look realistic and mean
  nothing.
- **Dates, IDs and links between tables are kept**, so filters, perspectives and the detail panel
  behave exactly as in the live report.

The published report is the real thing. If the two differ, the live one is right.

### Reading the dummy workbook

Without real content the tables are harder to read, so a few pointers:

- **One worksheet, `cv_input`, holds 26 Excel tables side by side**, one empty column between
  them. Power Query reads each table by name. `_context/context.md` §9 describes what every table
  is for.
- **Placeholders tell you which row they belong to.** `[Role 4 · List · Compact · bullet 2]` is the
  second bullet of role 4's List text at Compact detail. List placeholders keep the real number of
  bullets, and prose placeholders give the real text's rough length, so the layout still behaves
  as it does live.
- **Content tables are one row per item × view × detail level.** For example, `tbl_role_content`
  has a Story and a List version of each role, each at Standard and Compact. That grid is what the
  View and Detail Level buttons switch between.
- **Months are `YYYYMM` integers.** `999912` means "still ongoing" and `9999` is the same for
  year-only tables. Neither is ever shown in the report.
- **There are two different 1-7 scales.** `overall_proficiency_level` (on skills and tools) is a
  self-rating of current proficiency. `context_relevance_level` (on the role and project link tables) says how
  central a skill or tool was to one piece of work, and that is what the radar plots. Blank means
  not rated, never zero.
- **A few flags steer the layout.** `is_highlight` on an Additional row puts it on the Exec summary
  and at the top of Additional. `tooltip_override_text` on roles replaces the detail panel with a
  pointer to Projects.

## Opening it yourself

You need a recent Power BI Desktop (the model is at compatibility level 1606).

1. Clone the repository and open `_report/Adam Reichenbach CV.pbip`.
2. The data cache is not in the repository, so the visuals start empty. Point the
   `cv_source_path` parameter at your copy of `_input/CV.xlsx`: either Transform data → Edit
   parameters, or the one line in `SemanticModel/definition/expressions.tmdl`.
3. Refresh.

To use it for your own CV, fill in the workbook with your own content, keeping its tables and
columns.

## Repository layout

| Path | What it holds |
|---|---|
| `_report/` | The PBIP project: semantic model (TMDL) and report (PBIR JSON) |
| `_input/CV.xlsx` | The dummy source workbook |
| `_input/_images/excel_derived/` | Window chrome, column headers and tabs, based on the Excel interface (see *Licence and trademarks*) |
| `_input/_images/original/` | Ribbon icons, File menu buttons, the app and file icons, and Clippy, made for this project |
| `_context/` | The specification the agent built from, the design notes and a per-visual catalogue |
| `CLAUDE.md` | The agent's project instructions |

The instructions also refer to `_private/`. That is my working folder: the real workbook, the
session log, and my agent working method (operating rules, session commands, a validation hook and
the Power BI format lessons learned). It is deliberately not published, and nothing in the report
depends on it.

## Licence and trademarks

The MIT licence covers the code and the model (the TMDL, the PBIR JSON, the agent
instructions and the documentation) and the artwork in `_input/_images/original/`, which was made
for this project.

It does **not** cover the artwork based on the Microsoft Excel interface. The MIT licence grants
no rights in it, and any rights in the Excel interface remain with Microsoft. That is everything in
`_input/_images/excel_derived/`, and its copies in
`_report/*.Report/StaticResources/RegisteredResources/`:

- `background.png` and `overlay_background.png`
- `grid_header_*.png`
- the sheet tabs `01_*.png` to `07_*.png`, and `ribbon_file_*.png`, `ribbon_home_*.png`

The application icon painted into both backgrounds, and the file icon `ico_recent_xlsx.png`, are
original (`_input/_images/original/logo/`). The portrait in this repository is a placeholder.

Microsoft, Excel, Power BI and Fluent are trademarks of the Microsoft group of companies. This is a
personal, non-commercial project. It is not affiliated with, sponsored or endorsed by Microsoft.

## Thanks

To Maxim Anatsko for [pbir.tools](https://github.com/maxanatsko/pbir.tools), and to Kurt Buhler
(Data Goblins) for the
[Power BI agentic development](https://github.com/data-goblin/power-bi-agentic-development)
plugins. Both were pivotal in getting to a report built entirely by an agent.
