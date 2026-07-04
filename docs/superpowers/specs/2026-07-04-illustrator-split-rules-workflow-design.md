# Split 02-IllustratorsStyles into rules + workflow pages — design

Date: 2026-07-04

## Context

`02-IllustratorsStyles/README.md` currently mixes two different kinds of
content: the fixed figure standards (font sizes, tick lengths, RGB vs CMYK,
line weights) and the click-by-click Illustrator workflow (menu paths,
rasterize settings, export presets). Neither gets enough room — the rules
are buried in prose, and the workflow sections are a few bullets each where
a real walkthrough is needed.

Model for the rules page: [Nature's "Guide to designing
figures"](https://www.nature.com/documents/natrev-artworkguide.pdf) — one
page, one two-column Do/Do-not table, one row per topic, all numbers stated
inline, independent of any tutorial on how to use the tools.

## Goals

- Split `02-IllustratorsStyles/` into two top-level numbered pages:
  a standards page (what to do / what not to do) and an Illustrator
  workflow page (how to do it, in more detail than today).
- State every rule in exactly one place (the rules page); the workflow page
  explains how to achieve them and links back rather than restating values.
- Expand each workflow section from bullets into a real walkthrough,
  flagging spots where only Greg can supply the specifics instead of
  inventing steps.
- Fix all cross-references (root README, 01-Plotting, renumbered folders).

## Non-goals

- No PDF/build pipeline — plain Markdown rendered by GitHub.
- No new rules invented — the rules page codifies what the repo already
  says (README "rules in one screen", 01, 02).
- No changes to 01-Plotting content beyond link fixes.
- No Blender/3D rules — scoped to 2D figures, matching current coverage.
- No before/after example images in this pass — can be added later if
  wanted; the split and the expanded workflow text are the deliverable.

## Target structure

```
ScientificGraphicDesign/
├── 01-Plotting/            (unchanged)
├── 02-FigureRules/         (new)
│   └── README.md
├── 03-IllustratorWorkflow/ (new)
│   ├── README.md
│   ├── ColorPalettes/      (moved from 02-IllustratorsStyles/)
│   └── MyPapers.joboptions (moved from 02-IllustratorsStyles/)
├── 04-BlenderBoilerPlates/ (was 03-BlenderBoilerPlates/)
└── 05-Fonts/               (was 04-Fonts/)
```

`02-IllustratorsStyles/` is removed; its content is redistributed across
the two new pages. Folder renames done with `git mv` so history follows.

Naming note: the rules page is `02-FigureRules` (not `02-IllustratorRules`)
because several rules (typography sizes, tick/line specs, simplicity)
equally govern the Python plotting stage in 01 — the page is the standard
for the figure, whatever tool it's in.

## `02-FigureRules/README.md`

Short title + 1-2 sentence intro (this page is the standard; see 01 for
plotting and 03 for Illustrator technique), then one Do / Do-not table:

| Topic | Do | Do not |
|---|---|---|
| **Color mode** | Work in RGB (File > Document Color Mode > RGB) — figures are read on screens. | Work in CMYK; convert to CMYK only if a print-quality version is explicitly requested. |
| **Canvas** | Artboard 256 pt (single-column) / 512 pt (double-column); design at final printed size. | Design at arbitrary size and scale later — element sizes (fonts, strokes) stop matching the spec. |
| **Grid** | 2 pt grid; keep axes, schematics, and text on-grid. | Place gridable elements off-grid — only tick marks/labels and curve anchor points are exempt. |
| **Typography** | Helvetica Neue, true black, everywhere: tick labels 6 pt, axis labels 7 pt, legend 6 pt, annotations 7 pt, subplot labels 8 pt bold. | Mix fonts or sizes across panels or figures in the same paper; use italics or color for emphasis. |
| **Lines** | Tick length 2 pt, tick width 0.5 pt, axis width 0.5 pt, axis true black, plot lines 0.75 pt. | Heavier plot lines except to deliberately emphasize one curve; library-default stroke widths. |
| **Color use** | Pull from the group palettes (IBM, Scientific Colour Maps — `batlow`/`oslo` for 2D maps); same color = same sample/dataset across all figures; pass a Protanopia/Deuteranopia proof check before calling it done. | Rely on red/green alone to separate curves; reuse a color with a different meaning in the next figure; invent one-off colors. |
| **Simplicity** | One point per plot; keep only elements that carry information. | Surrounding box, background grid, fine tick spacing, or any decoration that carries no information. |
| **Bitmaps** | Crop to what's shown; keep linked (not embedded) in the `.ai`; rasterize at 500 ppi, RGB, transparent background, art-optimized anti-aliasing. | Mask instead of cropping; hand-embed linked images (PDF export embeds them automatically). |
| **Export** | Vector format (PDF preferred, via the MyPapers preset); each figure well under Overleaf's 50 MB/file cap. | Rasterized-only output from vector sources; a figure that needs `\includegraphics[width=...]` to fit — that means it wasn't exported at final size. |

Each Do cell links to the section of 01 or 03 that shows how (e.g. the
Bitmaps row links to the workflow's rasterize walkthrough).

## `03-IllustratorWorkflow/README.md`

The how-to, restructured so each current section becomes a real
walkthrough. Section list and what each expands to cover:

1. **Document setup** — color mode, artboard sizing for column widths,
   grid preferences (Edit > Preferences > Guides & Grid, 2 pt / 1
   subdivision), with the actual menu paths for macOS.
2. **Working with the grid** — snapping behavior, what goes on-grid vs.
   off, aligning imported plot axes to the grid, day-to-day habits.
3. **Color palettes and swatches** — installing `.ase` files (the
   `ColorPalettes/` folder lives here), IBM palette + shade logic and how it
   matches `pyprettyplot`'s `ibm` class, Crameri maps with the CC-BY
   attribution note carried over from the current page.
4. **Placing plots from 01-Plotting** — importing the exported
   SVG/PDF from the plotting stage, what cleanup is and isn't expected if
   the plot was made correctly, keeping text editable.
5. **Bitmap handling** — crop vs. mask and why, linking vs. embedding,
   the "Create PDF Compatible File" checkbox trick, Object > Rasterize
   settings (500 ppi / transparent / RGB / art-optimized), the 500→1000 ppi
   quality/size tradeoff.
6. **Colorblind proofing** — View > Proof Setup > Color Blindness
   walkthrough as the final pre-export check.
7. **Export** — installing `MyPapers.joboptions` (macOS path incl. the
   cmd+shift+G tip), what each preset setting does and why, the
   size sanity checks (under 50 MB, no `[width=...]` needed in LaTeX).

Drafting rule: expand from the existing content plus standard Illustrator
practice; where a step is group-specific and not currently documented
(e.g. layer organization habits, keyboard shortcuts Greg actually uses),
insert an explicit `<!-- TODO: Greg -->` marker rather than inventing it.
A short list of those markers is reported back at the end of
implementation so they can be filled in.

Rules values are not restated here — sections reference
`../02-FigureRules/` (e.g. "set type per the [typography
rules](../02-FigureRules/README.md)").

## Cross-reference updates

- **Root `README.md`**: "The rules, in one screen" section keeps its
  summary but points to `02-FigureRules/` as the canonical reference; the
  Pages table gains/renames rows for 02 and 03 and renumbers Blender and
  Fonts; the repo-layout tree is updated.
- **`01-Plotting/README.md`**: the link to "02 — Illustrator styles"
  becomes links to 02-FigureRules / 03-IllustratorWorkflow as appropriate.
- **`04-BlenderBoilerPlates/`, `05-Fonts/`**: folder renames; any
  intra-repo links updated (Fonts is referenced from the typography
  content, which now lives in 02's table and 03's setup section).
- **`TODO.md`**: folder references updated to new numbers.

## Git plan

Small stack of focused commits:
1. `git mv` renames (03→04 Blender, 04→05 Fonts) + link fixes for the
   renames only.
2. Create 02-FigureRules and 03-IllustratorWorkflow from the split of
   02-IllustratorsStyles (content move + expansion), move
   `ColorPalettes/` and `MyPapers.joboptions`, remove the old folder.
3. Root README / 01-Plotting / TODO.md cross-reference updates.
