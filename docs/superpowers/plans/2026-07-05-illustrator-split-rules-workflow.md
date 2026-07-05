# Split 02-IllustratorsStyles into Rules + Workflow Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace `02-IllustratorsStyles/` with two top-level pages — `02-FigureRules/` (the standard, as a Do/Do-not table) and `03-IllustratorWorkflow/` (the expanded how-to) — renumbering Blender to 04 and Fonts to 05.

**Architecture:** Pure documentation restructure in a Git repo of Markdown pages. Three commits: (1) `git mv` renames of the trailing folders plus the link fixes those renames break, (2) the split itself (two new READMEs, assets moved, old folder removed), (3) root README / 01-Plotting / 05-Fonts cross-reference updates. Every commit leaves the repo with zero dangling intra-repo links.

**Tech Stack:** Markdown, git. No build step, no code, no tests — verification is `grep` for stale folder references and reading the rendered Markdown.

## Global Constraints

- Spec: `docs/superpowers/specs/2026-07-04-illustrator-split-rules-workflow-design.md`.
- Every rule value is stated in exactly one place: `02-FigureRules/README.md`. The workflow page links to rules, never restates numbers.
- No new rules invented — all values come verbatim from the current README/01/02 content (tick labels 6pt, axis labels 7pt, legend 6pt, annotations 7pt, subplot labels 8pt bold, tick length 2pt, tick width 0.5pt, axis width 0.5pt, plot line 0.75pt, artboards 256pt/512pt, grid 2pt, rasterize 500ppi, Overleaf 50MB/file + 100MB/project).
- Group-specific workflow steps not currently documented get an explicit `<!-- TODO(Greg): ... -->` marker — never invented content. The full marker list is reported to the user at the end.
- All folder renames/moves use `git mv` so history follows.
- Verification greps must exclude `.superpowers/` and `docs/superpowers/` (specs/plans legitimately mention old names).
- Commit messages end with `Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>`.

---

### Task 1: Renumber trailing folders (Blender 03→04, Fonts 04→05)

**Files:**
- Rename: `03-BlenderBoilerPlates/` → `04-BlenderBoilerPlates/`
- Rename: `04-Fonts/` → `05-Fonts/`
- Modify: `README.md` (Pages table rows + repo-layout tree)
- Modify: `TODO.md` (two section headers)
- Modify: `02-IllustratorsStyles/README.md` (one link — folder is deleted in Task 2, but each commit must be link-clean)

**Interfaces:**
- Produces: folders `04-BlenderBoilerPlates/` and `05-Fonts/` at their final paths; Tasks 2–3 link to `../05-Fonts` and list `04-BlenderBoilerPlates/` in the tree.

- [ ] **Step 1: Rename the folders (order matters — free the 04- name first)**

```bash
cd /Users/greg/Documents/Lib/JQInanophotonics/ScientificGraphicDesign
git mv 04-Fonts 05-Fonts
git mv 03-BlenderBoilerPlates 04-BlenderBoilerPlates
```

- [ ] **Step 2: Update the two Pages-table rows in `README.md`**

Replace:

```markdown
| [03 — Blender boilerplates](03-BlenderBoilerPlates/) | Ready-made renders and reusable assets for photonics-style figure illustrations |
| [04 — Fonts](04-Fonts/) | Recommended fonts for figures, and why we don't bundle font files here |
```

with:

```markdown
| [04 — Blender boilerplates](04-BlenderBoilerPlates/) | Ready-made renders and reusable assets for photonics-style figure illustrations |
| [05 — Fonts](05-Fonts/) | Recommended fonts for figures, and why we don't bundle font files here |
```

- [ ] **Step 3: Update the repo-layout tree in `README.md`**

Replace:

```
├── 03-BlenderBoilerPlates/
│   ├── README.md
│   ├── Assets/
│   └── MiscModels/
└── 04-Fonts/
    └── README.md
```

with:

```
├── 04-BlenderBoilerPlates/
│   ├── README.md
│   ├── Assets/
│   └── MiscModels/
└── 05-Fonts/
    └── README.md
```

- [ ] **Step 4: Update `TODO.md` section headers**

`## 03-BlenderBoilerPlates` → `## 04-BlenderBoilerPlates`
`## 04-Fonts` → `## 05-Fonts`

- [ ] **Step 5: Update the Fonts link in `02-IllustratorsStyles/README.md`**

In the "Graphics design consistency" section, replace `(see [Fonts](../04-Fonts) for the full recommendation list)` with `(see [Fonts](../05-Fonts) for the full recommendation list)`.

- [ ] **Step 6: Verify no stale references to the old numbers remain**

```bash
grep -rn --include="*.md" -e "03-BlenderBoilerPlates" -e "04-Fonts" . | grep -v ".superpowers" | grep -v "docs/superpowers"
```

Expected: no output.

- [ ] **Step 7: Commit**

```bash
git add -A
git commit -m "Renumber Blender boilerplates to 04 and Fonts to 05, making room for the 02-IllustratorsStyles split

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 2: Create 02-FigureRules and 03-IllustratorWorkflow, remove 02-IllustratorsStyles

**Files:**
- Create: `02-FigureRules/README.md`
- Create: `03-IllustratorWorkflow/README.md`
- Rename: `02-IllustratorsStyles/ColorPalettes/` → `03-IllustratorWorkflow/ColorPalettes/`
- Rename: `02-IllustratorsStyles/MyPapers.joboptions` → `03-IllustratorWorkflow/MyPapers.joboptions`
- Delete: `02-IllustratorsStyles/README.md`

**Interfaces:**
- Consumes: `../05-Fonts` path from Task 1.
- Produces: section anchors in `03-IllustratorWorkflow/README.md` that `02-FigureRules/README.md` links to: `#document-setup`, `#working-with-the-grid`, `#color-palettes-and-swatches`, `#placing-plots-from-01-plotting`, `#bitmap-handling`, `#colorblind-proofing`, `#export`. Task 3 links to `02-FigureRules/` and `03-IllustratorWorkflow/`.

- [ ] **Step 1: Move the assets with history**

```bash
mkdir 03-IllustratorWorkflow
git mv 02-IllustratorsStyles/ColorPalettes 03-IllustratorWorkflow/ColorPalettes
git mv 02-IllustratorsStyles/MyPapers.joboptions 03-IllustratorWorkflow/MyPapers.joboptions
```

- [ ] **Step 2: Create `02-FigureRules/README.md` with exactly this content**

````markdown
# Figure Rules

This page is the standard: what a finished figure must look like, stated
once, with the numbers. Every figure in a paper should pass every row of
the table below before it ships — and because the whole point is
consistency, "close enough" on one figure quietly breaks all the others.
The tutorials show *how* to get there: [01 — Plotting
data](../01-Plotting/) for the Python side, [03 — Illustrator
workflow](../03-IllustratorWorkflow/) for the finishing side.

The format is borrowed from Nature's [Guide to designing
figures](https://www.nature.com/documents/natrev-artworkguide.pdf) —
worth reading on its own, since their art team's principles (hierarchy,
clarity, accessibility) are the same ones behind these rules.

| Topic | Do | Do not |
|---|---|---|
| **Color mode** | Work in RGB — figures are read on screens ([setup](../03-IllustratorWorkflow/README.md#document-setup)). | Work in CMYK; convert a copy to CMYK only if a print-quality version is explicitly requested. |
| **Canvas** | Artboard at final printed size: 256 pt wide for a single-column figure, 512 pt for double-column ([setup](../03-IllustratorWorkflow/README.md#document-setup)). | Design at an arbitrary size and scale later — scaling changes every font size and stroke width off-spec at once. |
| **Grid** | Work on a 2 pt grid: axes, schematics, and text sit on-grid ([grid habits](../03-IllustratorWorkflow/README.md#working-with-the-grid)). | Place gridable elements off-grid — only tick marks, tick labels, and curve anchor points are exempt. |
| **Typography** | One family, true black, everywhere — Helvetica Neue (alternatives in [05 — Fonts](../05-Fonts/)): tick labels 6 pt, axis labels 7 pt, legend 6 pt, annotations 7 pt, subplot labels 8 pt **bold**. | Mix fonts or sizes across panels or figures of the same paper; use italics, color, or size for emphasis. |
| **Lines** | Tick length 2 pt; tick, axis width 0.5 pt in true black; plot lines 0.75 pt. | Keep a plotting library's default stroke widths; thicken a curve except to deliberately emphasize it. |
| **Color use** | Pull from the group palettes — IBM for categorical colors, Scientific Colour Maps (`batlow`, `oslo`) for 2D maps ([palettes](../03-IllustratorWorkflow/README.md#color-palettes-and-swatches)); same color = same sample/dataset across *all* figures; pass a Protanopia/Deuteranopia proof check before calling a figure done ([proofing](../03-IllustratorWorkflow/README.md#colorblind-proofing)). | Rely on red/green alone to separate curves; reuse a color with a different meaning in the next figure; invent one-off colors. |
| **Simplicity** | One point per plot; keep only elements that carry information. | Surrounding box, background grid, fine tick spacing, or any decoration that answers no question a reader has. |
| **Bitmaps** | Crop to what is shown; keep the file **linked**, not embedded, in the `.ai`; rasterize at 500 ppi, RGB, transparent background ([bitmap handling](../03-IllustratorWorkflow/README.md#bitmap-handling)). | Mask instead of cropping (the hidden pixels ship with the file); hand-embed a linked image — PDF export embeds it automatically. |
| **Export** | Vector format (PDF preferred), via the MyPapers preset ([export](../03-IllustratorWorkflow/README.md#export)); each figure well under Overleaf's 50 MB-per-file cap (whole project under 100 MB). | Ship rasterized output from a vector source; export a figure that needs `\includegraphics[width=...]` to fit — if it does, it wasn't made at final size. |
````

- [ ] **Step 3: Create `03-IllustratorWorkflow/README.md` with exactly this content**

````markdown
# Illustrator Workflow

How we actually work in Adobe Illustrator, from blank document to
publication-ready PDF. This page is the *how*; the *what* — every font
size, stroke width, and export requirement — is stated once in
[02 — Figure rules](../02-FigureRules/) and linked from each section
rather than repeated here.

## Document setup

Three settings before drawing anything:

1. **Color mode**: File > Document Color Mode > RGB Color (see the
   [color-mode rule](../02-FigureRules/) for why RGB).
2. **Artboard**: set the width to the final column width from the
   [canvas rule](../02-FigureRules/) — you can type `256 pt` or `512 pt`
   directly into the Width field of the Artboard tool (`Shift+O`).
   Height is whatever the figure needs; you'll shrink it to fit at the
   end.
3. **Grid**: Illustrator > Settings > Guides & Grid (macOS; Edit >
   Preferences on Windows). Set *Gridline every* to the value in the
   [grid rule](../02-FigureRules/) and *Subdivisions* to 1. Then turn on
   View > Show Grid (`Cmd+'`) and View > Snap to Grid (`Shift+Cmd+'`).

<!-- TODO(Greg): do you start from a template .ai file with these presets
baked in? If one exists, link it here so nobody sets this up by hand. -->

## Working with the grid

**DO NOT WORK OUTSIDE OF THE GRID** (or at least as much as possible).
The grid is your friend: it aligns elements across panels and across
figures, and it is what makes a multi-panel figure look designed instead
of assembled.

What sits on-grid vs. what can't (per the
[grid rule](../02-FigureRules/)):

- On grid: the axes of every plot, schematics and their building blocks,
  every text block.
- Necessarily off-grid: tick marks and tick labels (they follow the data
  spacing), curve anchor points (they *are* the data).

Practical habits:

- With Snap to Grid on, dragging an imported plot by its axis corner
  snaps the axis frame to the grid — do that first, before any cleanup,
  so everything after inherits a clean origin.
- Align panels to each other by their axis frames, not their bounding
  boxes — tick labels make bounding boxes lie.
- When two subplots share an axis dimension, give them identical on-grid
  sizes; don't eyeball it.

<!-- TODO(Greg): any layer-organization convention (e.g. one layer per
panel, separate layer for annotations)? The current page doesn't say. -->

## Color palettes and swatches

The group palettes live in [`ColorPalettes/`](./ColorPalettes/) as `.ase`
swatch files. To load one: Window > Swatches, then from the panel's menu
choose Open Swatch Library > Other Library… and select the `.ase` file.

- **[IBM color palette](https://github.com/IBM-Design/colors/blob/master/ibm-colors.ase)**
  — our categorical palette, from the IBM design language (worth
  scrolling through on its own to see what a unified color system looks
  like). Each hue comes in numbered shades; this matches the `ibm` class
  in [`pyprettyplot`](https://github.com/JQInanophotonics/pyprettyplot)
  (e.g. `ibm.cerulean(shade=60)`), so a color picked in Python is
  reproducible in Illustrator and vice versa.
- **[Scientific Colour Maps](./ColorPalettes/ScientificColorMap/)** —
  Fabio Crameri's perceptually-uniform maps, redistributed here under
  their [open license
  terms](https://www.fabiocrameri.ch/ws/media-library/ce2eb6eee7c345f999e61c02e2733962/readme_scientificcolourmaps.pdf#Acknowledgement)
  — when you use these in a figure, credit Crameri's work as described
  there. Recommended for 2D maps: `batlow`, `oslo` (or `oslo_r`);
  `lipari` is also excellent but not bundled — get it from the link
  above.

Which palette to use where, and the accessibility requirements, are in
the [color rule](../02-FigureRules/).

## Placing plots from 01-Plotting

If the plot was made correctly in [01 — Plotting](../01-Plotting/) —
right physical size, right font sizes, right stroke widths — the
Illustrator pass is assembly and annotation, not repair.

- Bring the exported vector file (PDF/SVG) in with File > Place, or open
  it directly and copy the plot into your figure document.
- Snap the axis frame to the grid immediately (see
  [grid habits](#working-with-the-grid)).
- Text should stay text. If your labels arrive as outlined paths you
  can't edit, fix the export on the Python side instead of retyping
  labels by hand.

<!-- TODO(Greg): plotly SVG exports often need a specific cleanup
(released clipping masks, deleted background rects, font substitution
dialogs). Document the exact steps you actually do here. -->

## Bitmap handling

Most figure content should be vector, but photos (a sample micrograph, a
setup photo) are bitmaps. Per the
[bitmap rule](../02-FigureRules/):

- **Crop, don't mask.** A mask hides pixels but ships them; cropping
  (select the image, then Object > Crop Image) actually removes them and
  shrinks the file.
- **Link, don't embed.** Keep the bitmap linked in the `.ai` file
  (i.e. don't use Properties > Quick Actions > Embed Image). Illustrator
  then stores only a reference plus a low-res preview, keeping the
  working file small and responsive. You lose nothing at publication:
  PDF export embeds the linked raster data automatically, so the
  deliverable is self-contained.
- To stop Illustrator from silently embedding a preview of every linked
  image each time you save, uncheck **"Create PDF Compatible File"** in
  the save dialog.

To shrink an oversized bitmap (or rasterize an effect-heavy vector
object): select it, Object > Rasterize, with Resolution 500 ppi,
Background Transparent, Color Mode RGB, Anti-aliasing Art Optimized.
500 ppi is usually right; if the result looks pixelated, try 1000 ppi —
but that grows the file fast, so treat it as the exception. The file-size
ceiling this all serves is in the [export rule](../02-FigureRules/).

## Colorblind proofing

The last check before export, required by the
[color rule](../02-FigureRules/): View > Proof Setup > Color blindness —
Protanopia-type, then again with Deuteranopia-type. Look at the proofed
figure and ask: can every pair of curves still be told apart? If not, fix
it with different line styles, direct labels, or different colors — not
by hoping.

## Export

Save as PDF unless you have a specific reason not to — it has come a
long way and is the format LaTeX embeds most cleanly.

One-time setup: install the custom preset
[`MyPapers.joboptions`](./MyPapers.joboptions). On macOS, put the file in
`~/Library/Application Support/Adobe/Adobe PDF/Settings` (in Finder,
`Cmd+Shift+G` and paste the path). Then File > Save As > Adobe PDF and
pick **MyPapers** from the preset dropdown.

What the preset does, and why:

- *Standard: None, Compatibility: Acrobat 5 (PDF 1.4)* — maximum
  compatibility with LaTeX toolchains.
- *Preserve Illustrator Editing Capabilities: off* — that option stuffs
  the whole `.ai` document into the PDF; your editable source is the
  `.ai` file, not the deliverable.
- *Embed Page Thumbnails: off, Optimize for Fast Web View: on* — smaller
  file, faster loading.
- *Compression to 300 ppi* — print resolution for any raster content.
- *Convert colors to destination RGB, include profile* — consistent
  color on screens.

Sanity checks on the exported file, per the
[export rule](../02-FigureRules/): the file is nowhere near the 50 MB
cap (if it is, a bitmap wasn't rasterized properly or the plot carries
far more points than it shows), and it drops into LaTeX at final size —
if you need `\includegraphics[width=\textwidth]` to make it fit, the
figure wasn't made at the right size and something upstream needs fixing.
````

- [ ] **Step 4: Remove the old page**

```bash
git rm 02-IllustratorsStyles/README.md
```

Then confirm the folder is gone:

```bash
ls 02-IllustratorsStyles 2>&1
```

Expected: `No such file or directory` (git mv in Step 1 + git rm here empty it fully; if `.DS_Store` or similar lingers, `rm` it).

- [ ] **Step 5: Verify anchors and links**

Every anchor linked from 02 must exist in 03:

```bash
grep -o "IllustratorWorkflow/README.md#[a-z-]*" 02-FigureRules/README.md | sort -u
grep -n "^## " 03-IllustratorWorkflow/README.md
```

Expected: the anchor list is exactly `#document-setup`, `#working-with-the-grid`, `#color-palettes-and-swatches`, `#placing-plots-from-01-plotting`, `#bitmap-handling`, `#colorblind-proofing`, `#export`, and each matches a `##` heading (GitHub anchor = heading lowercased, spaces → `-`).

```bash
grep -rn --include="*.md" "02-IllustratorsStyles" 02-FigureRules 03-IllustratorWorkflow
```

Expected: no output.

- [ ] **Step 6: Commit**

```bash
git add -A
git commit -m "Split Illustrator styles into 02-FigureRules and 03-IllustratorWorkflow

The rules (every font size, stroke width, color and export requirement)
now live once, as a Do/Do-not table modeled on Nature's figure-design
guide. The workflow page carries the expanded how-to and links back to
the rules instead of restating them. ColorPalettes/ and
MyPapers.joboptions move with the workflow.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 3: Cross-reference updates (root README, 01-Plotting, 05-Fonts)

**Files:**
- Modify: `README.md` (rules-section intro, rule 5 link, Pages table, repo-layout tree)
- Modify: `01-Plotting/README.md` (Illustrator link)
- Modify: `05-Fonts/README.md` (back-link)

**Interfaces:**
- Consumes: `02-FigureRules/` and `03-IllustratorWorkflow/` paths from Task 2; `04-BlenderBoilerPlates/`, `05-Fonts/` from Task 1.

- [ ] **Step 1: Update the rules-section intro in `README.md`**

Replace:

```markdown
The details live in [02 — Illustrator styles](02-IllustratorsStyles/) and
[01 — Plotting data](01-Plotting/); the short version:
```

with:

```markdown
The full standard — every number, as a do/do-not table — is
[02 — Figure rules](02-FigureRules/); the short version:
```

- [ ] **Step 2: Update the rule-5 link in `README.md`**

In rule 5 ("Save vector"), replace `(see [02 — Illustrator styles](02-IllustratorsStyles/))` with `(see [03 — Illustrator workflow](03-IllustratorWorkflow/))`.

- [ ] **Step 3: Replace the 02 row of the Pages table in `README.md` with two rows**

Replace:

```markdown
| [02 — Illustrator styles](02-IllustratorsStyles/) | Document settings, grids, color palettes, handling bitmaps, and export presets for Adobe Illustrator |
```

with:

```markdown
| [02 — Figure rules](02-FigureRules/) | The standard every figure must meet — fonts, lines, colors, grid, export — as one do/do-not table |
| [03 — Illustrator workflow](03-IllustratorWorkflow/) | Working in Adobe Illustrator: document setup, grids, palettes, placing plots, bitmaps, proofing, export |
```

(The 04-Blender and 05-Fonts rows already exist from Task 1.)

- [ ] **Step 4: Update the repo-layout tree in `README.md`**

Replace:

```
├── 02-IllustratorsStyles/
│   ├── README.md
│   └── ColorPalettes/
```

with:

```
├── 02-FigureRules/
│   └── README.md
├── 03-IllustratorWorkflow/
│   ├── README.md
│   └── ColorPalettes/
```

- [ ] **Step 5: Update `01-Plotting/README.md`**

Replace:

```markdown
Some tips for Adobe Illustrator are provided in [02 — Illustrator styles](../02-IllustratorsStyles).
```

with:

```markdown
The standard your figure must meet is [02 — Figure rules](../02-FigureRules/), and the Illustrator side of getting there is [03 — Illustrator workflow](../03-IllustratorWorkflow/).
```

- [ ] **Step 6: Update `05-Fonts/README.md`**

Replace:

```markdown
design tips more broadly, see [../02-IllustratorsStyles](../02-IllustratorsStyles).
```

with:

```markdown
design tips more broadly, see [02 — Figure rules](../02-FigureRules/) and [03 — Illustrator workflow](../03-IllustratorWorkflow/).
```

- [ ] **Step 7: Final verification — no stale folder references anywhere**

```bash
grep -rn --include="*.md" -e "02-IllustratorsStyles" -e "03-BlenderBoilerPlates" -e "04-Fonts" . | grep -v ".superpowers" | grep -v "docs/superpowers"
```

Expected: no output.

```bash
ls -d 01-Plotting 02-FigureRules 03-IllustratorWorkflow 04-BlenderBoilerPlates 05-Fonts
```

Expected: all five listed, no error.

- [ ] **Step 8: Commit**

```bash
git add -A
git commit -m "Point root README, 01-Plotting, and 05-Fonts at the new rules and workflow pages

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

- [ ] **Step 9: Report the TODO(Greg) markers**

```bash
grep -rn "TODO(Greg)" 03-IllustratorWorkflow/README.md
```

List each marker for the user: the `.ai` template question (document
setup), the layer-organization convention (grid section), and the plotly
SVG cleanup steps (placing plots). These are the group-specific details
only Greg can fill in.
