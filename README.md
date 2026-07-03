# Scientific Graphic Design

## Forewords

A good figure does a lot of the persuading in a paper before anyone reads
a word of the text — and a bad one (mismatched fonts, inconsistent colors,
axes that don't line up figure to figure) undercuts even solid results.
This repo is the group's guide to the tools and habits behind
publication-quality figures: plotting data in Python/Plotly, finishing and
styling in Adobe Illustrator, and 3D rendering in Blender, plus the fonts
and color palettes that tie them together.

Read the pages in order the first time — each one builds on the last; use
them as a checklist afterwards — same spirit as
[ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement).
If you're only after a boilerplate or an example file, jump directly into
the relevant folder without reading the tutorial.

Slide-deck and presentation-specific guidance (structuring a talk,
animating figures, Beamer) lives in
[ScientificPresentations](https://github.com/JQInanophotonics/ScientificPresentations)
— this repo covers the figures themselves, that one covers assembling them
into a talk.

Feel free to tweak and contribute via pull requests. If you use tips or
assets from here, let Greg Moille know by [email](mailto:gmoille@umd.edu)
or any other [means](https://srinivasan.jqi.umd.edu/people/gregory-moille)
— always glad to hear it's being put to good use.

## The rules, in one screen

The details live in [02 — Illustrator styles](02-IllustratorsStyles/) and
[01 — Plotting data](01-Plotting/); the short version:

1. **Clarity, simplicity, consistency.** One point per plot, no superfluous elements (no box/grid/fine ticks you don't need), and the same style across every figure in a given paper.
2. **One font family, one set of sizes, everywhere.** Default is Helvetica Neue: tick labels 6pt, axis labels 7pt, annotations 7pt, subplot labels 8pt bold — all in true black.
3. **One set of line rules, everywhere.** Tick length 2pt, tick width 0.5pt, axis width 0.5pt, plot line width 0.75pt unless you're emphasizing a curve.
4. **Design for colorblindness** — don't rely on red/green alone to distinguish curves; check Illustrator's proof-colors view (Protanopia/Deuteranopia) before calling a figure done.
5. **Save vector** (PDF/SVG/EPS). Keep any bitmap **linked**, not embedded, in the `.ai` file — PDF export embeds the raster data into the PDF automatically, so there's no need to embed it by hand (see [02 — Illustrator styles](02-IllustratorsStyles/)).
6. **Keep figures under ~10MB.** If you're over that, your bitmap isn't rasterized properly or your data isn't processed down to what the plot actually needs.

## Pages

| Page | What it covers |
|------|-----------------|
| [01 — Plotting data](01-Plotting/) | Plotting principles, color palettes/colormaps, the `pyprettyplot` Python package, and a worked example |
| [02 — Illustrator styles](02-IllustratorsStyles/) | Document settings, grids, color palettes, handling bitmaps, and export presets for Adobe Illustrator |
| [03 — Blender boilerplates](03-BlenderBoilerPlates/) | Ready-made renders and reusable assets for photonics-style figure illustrations |
| [04 — Fonts](04-Fonts/) | Recommended fonts for figures, and why we don't bundle font files here |

## What's in this repo

```
ScientificGraphicDesign/
├── README.md
├── TODO.md
├── 01-Plotting/
│   ├── README.md
│   ├── pyprettyplot/
│   ├── ExampleDataSet/
│   └── Notebook.ipynb
├── 02-IllustratorsStyles/
│   ├── README.md
│   └── ColorPalettes/
├── 03-BlenderBoilerPlates/
│   ├── README.md
│   ├── Assets/
│   └── MiscModels/
└── 04-Fonts/
    └── README.md
```

## Basic Graphic Design

We won't go over the basics of graphic design in depth here. The two
things worth emphasizing:

1. Learn how to use a [color wheel](https://www.canva.com/colors/color-wheel/).
2. Make sure you understand the [meaning of color](https://webflow.com/blog/color-meanings) (e.g., red = bad, green = good, blue = calm/modern, etc.). Don't use a red color for something you want to highlight as good.
3. Learn [how to use a grid](https://uxplanet.org/grids-in-graphic-design-a-quick-history-and-5-top-tips-29c8c0650d18).
4. Know that not everybody sees things like you do — make sure your design is inclusive and accessible for color blindness. Use tools (protanopia or deuteranopia mode in Adobe Illustrator) and don't rely solely on color to distinguish curves.

Beyond that, if you want to go deeper, we recommend the following resources (and if you know Greg, you can probably borrow a copy):
- **Basic knowledge of graphic design**:
    - The Elements of Graphic Design by Alex W. White
    - Beautiful Evidence by Edward Tufte
- **Layout and structure**
    - Making and Breaking the Grid, Third Edition: A Graphic Design Layout Workshop
    - Grid Systems by Josef Müller-Brockmann
- **Color Theory**
    - Interaction of Color by Josef Albers
    - Color by Betty Edwards
    - Color Index by Jim Krause
    - The Pantone book, browsed whenever you need it
- **Psychology of graphic design**
    - 100 Things Every Designer Needs to Know About People by Susan Weinschenk
    - The Design of Everyday Things by Don Norman
    - Book of Branding by Radim Malinic
- **Scientific Display**
    - The Visual Display of Quantitative Information by Edward Tufte
- **Typography**
    - Thinking with Type by Ellen Lupton
    - The Elements of Typographic Style by Robert Bringhurst

## See also

Sibling repos in the [JQInanophotonics](https://github.com/JQInanophotonics) org: [ScientificPresentations](https://github.com/JQInanophotonics/ScientificPresentations) — talk design and slide decks, [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement) — data collection and archival, [ScientificWriting](https://github.com/JQInanophotonics/ScientificWriting) — paper composition and citation practices, [InstrumentControl](https://github.com/JQInanophotonics/InstrumentControl) — lab equipment operation, [3DPrintedLabParts](https://github.com/JQInanophotonics/3DPrintedLabParts) — shared fabricated part designs.
