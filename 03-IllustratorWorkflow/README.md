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

A starter template `.ai` file with all three presets baked in will live
in this folder — until it lands, set them by hand as above.

<!-- TODO(Greg): make the template .ai and link it in the paragraph
above. -->

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
- Organize layers **one per panel**: each panel — its plot, labels, and
  annotations — lives on its own layer, so you can lock or hide finished
  panels while working on the next.

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
- Remove every unnecessary element the import drags along: empty groups,
  invisible rectangles, leftover masks — anything that carries no
  information.
- Release every clipping mask (Object > Clipping Mask > Release), then
  make each element individual: clip the previously-masked lines to the
  old mask boundary or delete them outright. Shape Builder (`Shift+M`)
  with the released mask still selected is the quick way to do the
  clipping.

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
[export rule](../02-FigureRules/): the file is nowhere near the size
cap (if it is, a bitmap wasn't rasterized properly or the plot carries
far more points than it shows), and it drops into LaTeX at final size —
if you need `\includegraphics[width=\textwidth]` to make it fit, the
figure wasn't made at the right size and something upstream needs fixing.
