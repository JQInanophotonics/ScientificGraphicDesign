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
| **Typography** | One family, true black, everywhere — Helvetica Neue (alternatives in [05 — Fonts](../05-Fonts/)): tick labels 6 pt, axis labels 6 pt, legend 5 pt, annotations 6 pt, subplot labels 8 pt **bold** — [01 — Plotting](../01-Plotting/) sets these automatically. | Mix fonts or sizes across panels or figures of the same paper; use italics, color, or size for emphasis. |
| **Lines** | Tick length 1.5 pt; one line width everywhere — ticks, axes, plot lines — 0.5 pt, axes and ticks in true black — [01 — Plotting](../01-Plotting/) sets these automatically. | Keep a plotting library's default stroke widths; thicken a curve except to deliberately emphasize it. |
| **Color use** | Pull from the group palettes — IBM for categorical colors, Scientific Colour Maps (`batlow`, `oslo`, `lipari`) for 2D maps ([palettes](../03-IllustratorWorkflow/README.md#color-palettes-and-swatches)); same color = same sample/dataset across *all* figures; pass a Protanopia/Deuteranopia proof check before calling a figure done ([proofing](../03-IllustratorWorkflow/README.md#colorblind-proofing)). | Rely on red/green alone to separate curves; reuse a color with a different meaning in the next figure; invent one-off colors. |
| **Simplicity** | One point per plot; keep only elements that carry information. | Surrounding box, background grid, fine tick spacing, or any decoration that answers no question a reader has. |
| **Bitmaps** | Crop to what is shown; keep the file **linked**, not embedded, in the `.ai`; rasterize at 500 ppi, RGB, transparent background ([bitmap handling](../03-IllustratorWorkflow/README.md#bitmap-handling)). | Mask instead of cropping (the hidden pixels ship with the file); hand-embed a linked image — PDF export embeds it automatically. |
| **Export** | Vector format (PDF preferred), via the MyPapers preset ([export](../03-IllustratorWorkflow/README.md#export)); each figure well under Overleaf's 50 MB-per-file cap (whole project under 100 MB). | Ship rasterized output from a vector source; export a figure that needs `\includegraphics[width=...]` to fit — if it does, it wasn't made at final size. |
