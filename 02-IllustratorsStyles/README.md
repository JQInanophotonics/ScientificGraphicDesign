# Illustrator Styles & Setup

# Document Settings

- **Always work in RGB color mode**. Since most of your figures in a manuscript will be displayed on a screen, it is better to work in RGB color mode. If you need to have high quality printout, then you can convert the figure to CMYK later.  To set it up, go to File > Document Color Mode > RGB Color.
- Set the artboard size to the size of the figure you want to create: 
    - a single-column figure would be 256 pts
    - a double-column figure would be 512 pts
- Set the grid step to 2pt:
    - Go to Edit > Preferences > Guides & Grid
    - Set the gridline every to 2pt and subdivisions to 1
- **DO NOT WORK OUTSIDE OF THE GRID** (or at least as much as possible). The grid is your friend. It will help you to align your elements and make your figure look more professional.
    - What should be on grid: 
        - Keep the schematic on grid
        - Keep the axis of your figure on the grid. 
        - Keep the text on the grid 
    - Of course some things can't be on grid; 
        - tick marks and label 
        - curves anchor points        

# Colors Palettes

- [IBM colorpalette](https://github.com/IBM-Design/colors/blob/master/ibm-colors.ase): comes from the IBM design language which to be honest is a must to scroll through to understand how to make unified good design. It is also consistent with the [prettyplot class](../01-Plotting/pyprettyplot/), which include the `ibm` class to retrieve the colors (for instance `ibm.cerulean(shade = 60)`). 
- [ScientificColorMap](./ColorPalettes/ScientificColorMap/) (`.ase` swatches): Fabio Crameri's [Scientific Colour Maps](https://www.fabiocrameri.ch/colourmaps/), redistributed here under their [open license terms](https://www.fabiocrameri.ch/ws/media-library/ce2eb6eee7c345f999e61c02e2733962/readme_scientificcolourmaps.pdf#Acknowledgement) — when you use these in a figure, credit Crameri's work as described there. Recommended for 2D map plots: `batlow`, `oslo` (or `oslo_r`); `lipari` is also excellent but isn't among the palettes bundled here — get it separately from the link above if you want it.


# Handling bitmap figures (png, jpg, etc.)  

Of course, most of your figures will be in vector format. But sometimes you'll need to put some bitmap in your file (e.g. a photo of your sample). To do so, you can use the following steps:
- Do not mask the bitmap figure. Instead crop it, as it will reduce its size.
- Keep the bitmap **linked**, not embedded, in the `.ai` file. Linking (instead of Properties > Quick Actions > Embed Image) keeps your working file small and responsive — Illustrator only stores a reference plus a low-res preview, not the full image data. You don't lose anything at publication time: when you export/save to PDF, Illustrator embeds the linked raster data into the PDF automatically, so the final deliverable is self-contained.
- To stop Illustrator from silently embedding a preview of linked images every time you save the `.ai` file, uncheck **"Create PDF Compatible File"** in the save dialog.

To reduce the size of the bitmap figure, you can use the following steps:
- Select the object you want to rasterize
- Go to Object > Rasterize
- In the Rasterize window, select the following options:
    - Resolution: 500 ppi
    - Background: Transparent
    - Color Mode: RGB
    - Anti-aliasing: Art Optimized

This will probably give you good results. If you find that the figures became too pixelated, you can try to increase the resolution to 1000 ppi. But be careful, this will increase the size of the file, so try to find a tradeoff between size and quality.


# Graphics design consistency

- **Use the same font for all your figures**. For info, here is what we use (see [Fonts](../04-Fonts) for the full recommendation list): 
    - tick labels: Helvetica Neue Regular 6pt
    - axis labels: Helvetica Neue Regular 7pt
    - legend: Helvetica Neue Regular 6pt
    - Annotation: Helvetica Neue Regular 7pt
    - Subplot label: Helvetica Neue Bold 8pt
    - All in the same color: true black
- **Be consistent in your plotting**. For instance, all our plots have the same rules: 
    - tick length: 2pt
    - tick width: 0.5pt
    - axis width: 0.5pt
    - axis color: true black
    - unless emphasis, plot line width: 0.75pt
- **Be consistent in your colors**. Use the same colors if it is from the same sample/dataset. 


# Saving 

Unless you have a very specific reason, the figure would be better off saved as a vector-compatible format (pdf, svg, eps, etc.). The PDF format has come a long way and is now highly preferable for embedding with LaTeX.

To save a pdf that is optimized for publication in a paper, install the [custom Adobe PDF Presets](./MyPapers.joboptions). In MacOS, go to ~/Library/Application Support/Adobe/Adobe PDF/Settings (if you don't know cmd+shift+G in Finder and paste the location) and put the file there. Then, when you save a pdf, you can select the custom preset "MyPapers" in the dropdown menu. This will create a pdf with the following settings: 
- Standard: None
- Compatibility: Acrobat 5 (PDF 1.4)
- Preserve Illustrator Editing Capabilities: unchecked
- Embed Page Thumbnails: unchecked
- Optimize for Fast Web View: **checked**
- Preserve Hyperlinks: unchecked
- Compression to 300ppi
- Convert color to destination RGB and include profile

# Advices

- Keep your figures well under Overleaf's hard 50MB-per-file limit for git-synced projects — and since Overleaf also recommends keeping the whole project under 100MB total, budget each figure well below its share of that, not right up against the ceiling. If a single figure is anywhere near it, you either didn't rasterize your bitmap properly or didn't process your data properly (e.g. way too many points that are useless). 
- Keeping the size of your figure as it will appear in the manuscript helps you keep consistency over the element size (font, line stroke, etc.). After saving and while embedding the figure **YOU SHOULD NOT HAVE TO USE `[width=\textwidth]` in the `\includegraphics` command**. If you do, it means that your figure is not the right size, and you messed up somewhere. 