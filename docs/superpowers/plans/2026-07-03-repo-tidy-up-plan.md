# ScientificGraphicDesign Repo Tidy-Up Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Tidy up the `ScientificGraphicDesign` repo so it reads as a clean, group-wide teaching resource: remove content that belongs elsewhere or shouldn't be redistributed, restructure the top level into a numbered reading path, and fix broken references — while explicitly deferring anything that would require writing new teaching content or resolving in-progress code.

**Architecture:** This is a documentation/file-reorg task, not a code change — there is no application to build or test suite to run. Each task is a self-contained git commit; "tests" are verification shell commands (file existence checks, `grep` for stale references, `git status`) run after each edit instead of unit tests.

**Tech Stack:** Git, Markdown, shell (bash/find/grep). No application code is modified except a single URL string in `Plotting/setup.py`.

## Global Constraints

These come directly from the approved spec at
`docs/superpowers/specs/2026-07-03-repo-tidy-up-design.md`. Every task below must respect them:

- Do NOT resolve the `pyprettyplot`/`_pyprettyplot` duplication in `Plotting/` or edit that package's code/logic. Leave `Plotting/pyprettyplot/{__init__.py,colors.py,dispersion.py}`, the untracked `Plotting/_pyprettyplot/`, `Plotting/pyprettyplot/matplotlibrc`, and `Plotting/pyprettyplot/fonts/` untouched (only their parent folder gets renamed in Task 3).
- Do NOT write new teaching content for empty stub sections (typography deep-dive, Blender basics, etc.) — log gaps in `TODO.md` instead.
- Do NOT modify or trim the `ScientificColorMap` `.ase` files — they are properly licensed (CC-BY), only the attribution wording changes.
- Do NOT perform the actual GitHub org transfer (`gregmoille/ScientificGraphicDesign` → `JQInanophotonics`) — that's a manual administrative step, only tracked in `TODO.md`.
- Land the work as 4 separate commits (not one big diff), in the order given below.
- Follow the harness's standing git commit convention: append a `Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>` trailer to each commit message (matching the design-doc commit already on this branch).

---

### Task 1: Remove accidental/obsolete content and OS cruft

**Files:**
- Delete: `3Dprinting/` (entire directory, contains its own nested `.git`)
- Delete: `SlideDeckFigures/` (already has README.md deleted in working tree; remove the now-empty directory)
- Delete: `SlidesWithBeamer/` (same)
- Delete: `img/` (empty, unreferenced root folder)
- Delete: all `.DS_Store` files tracked or untracked anywhere in the repo
- Delete (stage existing working-tree deletions): `Plotting/pyprettyplot/__pycache__/*.pyc` (5 files, already removed from disk, just need staging)

**Interfaces:** N/A — no code interfaces, this is filesystem cleanup only.

- [ ] **Step 1: Confirm starting state matches expectations**

Run: `git status --short`

Expected output (order may vary):
```
 M Plotting/pyprettyplot/__init__.py
 D Plotting/pyprettyplot/__pycache__/__init__.cpython-310.pyc
 D Plotting/pyprettyplot/__pycache__/__init__.cpython-313.pyc
 D Plotting/pyprettyplot/__pycache__/colors.cpython-310.pyc
 D Plotting/pyprettyplot/__pycache__/dispersion.cpython-310.pyc
 D Plotting/pyprettyplot/__pycache__/gregplot.cpython-310.pyc
 M Plotting/pyprettyplot/colors.py
 M Plotting/pyprettyplot/dispersion.py
 D SlideDeckFigures/README.md
 D SlidesWithBeamer/README.md
?? 3Dprinting/
?? Plotting/_pyprettyplot/
?? Plotting/pyprettyplot/fonts/
?? Plotting/pyprettyplot/matplotlibrc
```

If this doesn't match (e.g. `Plotting/_pyprettyplot/` is gone, or `3Dprinting/` is gone), STOP and check with the user before proceeding — the described mid-refactor WIP may have changed.

- [ ] **Step 2: Remove the accidental nested-repo folder**

```bash
rm -rf 3Dprinting
```

- [ ] **Step 3: Remove the now-empty slide-deck folders**

```bash
test -z "$(ls -A SlideDeckFigures)" && rmdir SlideDeckFigures
test -z "$(ls -A SlidesWithBeamer)" && rmdir SlidesWithBeamer
```

Expected: both commands succeed silently (empty check passes, `rmdir` succeeds). If either directory is NOT empty, STOP — there's content here beyond the README that needs a decision.

- [ ] **Step 4: Remove the empty root img/ folder**

```bash
test -z "$(ls -A img)" && rm -rf img
```

Expected: succeeds silently. If `img/` is not empty, STOP and check with the user.

- [ ] **Step 5: Remove all .DS_Store files**

```bash
find . -name ".DS_Store" -not -path "./.git/*" -delete
```

- [ ] **Step 6: Stage only the intended changes**

**Do NOT run a bare `git add -A`** — this repo has unrelated modified tracked files (`Plotting/pyprettyplot/{__init__.py,colors.py,dispersion.py}`) that a bare `-A` would sweep into this commit, violating the Global Constraints. Stage explicitly instead:

```bash
git add SlideDeckFigures/README.md SlidesWithBeamer/README.md
git add Plotting/pyprettyplot/__pycache__
```

- [ ] **Step 6b: Confirm the unrelated WIP was NOT staged**

```bash
git status --short | grep -E "^ M Plotting/pyprettyplot/(__init__|colors|dispersion)\.py"
```

Expected: all 3 lines present with a leading space before the `M` (unstaged modification) — NOT `M ` or `MM` (which would mean it got staged). If any of these three files shows as staged, run `git restore --staged <file>` on it before continuing.

- [ ] **Step 7: Verify the staged diff**

Run: `git status --short`

Expected: shows `D` for `SlideDeckFigures/README.md`, `D` for `SlidesWithBeamer/README.md`, `D` for the 5 `__pycache__/*.pyc` files, and NO entry at all for `3Dprinting/`, `img/`, or any `.DS_Store` (they were untracked/ignored, so their removal produces no git status line — confirm with `ls` instead):

```bash
test ! -e 3Dprinting && test ! -e img && test ! -e SlideDeckFigures && test ! -e SlidesWithBeamer && echo "CLEAN"
```

Expected: `CLEAN`

Note: `Plotting/_pyprettyplot/`, `Plotting/pyprettyplot/fonts/`, and `Plotting/pyprettyplot/matplotlibrc` must still show as untracked (`??`) in `git status --short` — do NOT stage or remove them, per the Global Constraints.

- [ ] **Step 8: Commit**

```bash
git commit -m "$(cat <<'EOF'
Remove accidental nested repo, migrated slide content, and OS cruft

3Dprinting/ was an accidental nested clone of the sibling 3DPrintedLabParts
org repo. SlideDeckFigures/ and SlidesWithBeamer/ move to the dedicated
ScientificPresentations org repo. img/ was empty and unreferenced.

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 2: Font licensing cleanup

**Files:**
- Delete: `Fonts/CooldFonts/` (entire directory, ~73 font binary files)
- Modify: `Fonts/README.md` (full rewrite)
- Modify: `IllustratorsStyles/README.md:47-51` (font list)
- Modify: `Plotting/README.md:47-48` (font references)

**Interfaces:** N/A.

- [ ] **Step 1: Delete the bundled font binaries**

```bash
rm -rf Fonts/CooldFonts
```

- [ ] **Step 2: Rewrite Fonts/README.md**

Replace the entire contents of `Fonts/README.md` with:

```markdown
# What font should I use for my figures?

For a scientific figure you want a font that ticks most of these boxes:
- Readable (a sans-serif or slab font)
- Available glyphs to render Greek letters and mathematical symbols
- Multiple weights available (at least a bold option)

Recommended fonts, in order of what we actually use:

- **Standard kerning** (default choice):
    1. **Helvetica Neue** by Max Miedinger — our default. Available through
       an Adobe Creative Cloud license; must be installed/activated
       locally (see [TODO.md](../TODO.md) for install steps). Also the
       easiest choice if you're publishing with Nature Portfolio, since
       their production team will likely convert to it anyway.
    2. [IBM Plex Sans](https://www.ibm.com/plex/) by IBM — SIL Open Font
       License, fully free to install and redistribute.
    3. [Cooper Hewitt](https://www.cooperhewitt.org/open-source-at-cooper-hewitt/cooper-hewitt-the-typeface-by-chester-jenkins/)
       by Chester Jenkins — SIL Open Font License.
- **Condensed font** (for tight axis/legend labels):
    1. [IBM Plex Condensed](https://www.ibm.com/plex/) by IBM — SIL Open
       Font License.
    2. [Inter](https://rsms.me/inter/) by Rasmus Andersson — open source.
    3. Aktiv Grotesk Condensed by Dalton Maag — Adobe CC only, not
       redistributed in this repo.
    4. Bebas Neue Pro by Ryoichi Tsunekawa/Dharma Type — Adobe CC only, not
       redistributed in this repo.

We don't bundle any font files in this repo — most of the fonts above are
commercial typefaces, and redistributing them would violate their license.
Free/open options (IBM Plex, Inter, Cooper Hewitt) can be installed
directly from their own sites; Adobe CC options are activated through your
Creative Cloud account, not downloaded from here.

Designing a font is a genuinely hard, specialized craft — that's why the
good ones aren't free.

# Want to go down the rabbit hole?

A deeper typography write-up (serif vs. slab vs. sans-serif, kerning and
width, glyph coverage, and how to pick a font as a scientist) hasn't been
written yet — tracked in [TODO.md](../TODO.md). For scientific graphic
design tips more broadly, see [../IllustratorsStyles](../IllustratorsStyles).
```

Note: this step happens *before* Task 3's folder renames, so the relative links above (`../TODO.md`, `../IllustratorsStyles`) use pre-rename paths. Task 3 Step 4 corrects them to `../01-Plotting`-style numbered paths — actually this file only links to `../TODO.md` and `../IllustratorsStyles`, neither of which involves this file's own folder number, so re-check in Task 3 whether these need updating (they do: `../IllustratorsStyles` becomes `../02-IllustratorsStyles` once folders are renamed — this is handled explicitly in Task 3 Step 5).

- [ ] **Step 3: Update font references in IllustratorsStyles/README.md**

Find this exact block (around line 46-51):

```
- **Use the same font for all your figures**. For info, here is what I use: 
    - tick labels: Decima Regular 6pt
    - axis labels: Aktiv Grotesk Condensed Regular 7pt
    - legend: Aktiv Grotesk Condensed Regular  6pt
    - Annotation: Aktiv Grotesk Condensed Regular 7pt
    - Subplot label: Aktiv Grotesk Condensed Bold 8pt
```

Replace with:

```
- **Use the same font for all your figures**. For info, here is what we use (see [Fonts](../Fonts) for the full recommendation list): 
    - tick labels: Helvetica Neue Regular 6pt
    - axis labels: Helvetica Neue Regular 7pt
    - legend: Helvetica Neue Regular 6pt
    - Annotation: Helvetica Neue Regular 7pt
    - Subplot label: Helvetica Neue Bold 8pt
```

- [ ] **Step 4: Update font references in Plotting/README.md**

Find this exact block (around line 44-48):

```
1. Axis with a linewidth of 0.5 pt with color #2E3440  
2. Tick with a linewidth of 0.5 pt with color #2E3440, and a length of 2 pt  
3. No top and right spines  
4. Tick labels using Decima (fallback to default if not installed) 7 pt  
5. Axis title labels using Aktiv Grotesk Condensed Regular (fallback to default if not installed) 8 pt  
```

Replace with:

```
1. Axis with a linewidth of 0.5 pt with color #2E3440  
2. Tick with a linewidth of 0.5 pt with color #2E3440, and a length of 2 pt  
3. No top and right spines  
4. Tick labels using Helvetica Neue (fallback to default if not installed) 7 pt  
5. Axis title labels using Helvetica Neue (fallback to default if not installed) 8 pt  
```

- [ ] **Step 5: Verify no stale font references remain in Plotting or IllustratorsStyles docs**

```bash
grep -rn "Aktiv Grotesk\|Decima" Plotting/README.md IllustratorsStyles/README.md
```

Expected: no output (both files now clean of these references). `Fonts/README.md` is expected to still mention "Aktiv Grotesk Condensed" once, as a documented alternative — that's fine, don't grep-check that file.

```bash
test ! -d Fonts/CooldFonts && echo "FONTS REMOVED"
```

Expected: `FONTS REMOVED`

- [ ] **Step 6: Commit**

```bash
git add -A Fonts IllustratorsStyles/README.md Plotting/README.md
git commit -m "$(cat <<'EOF'
Remove bundled commercial font files, document Helvetica Neue as default

Fonts/CooldFonts/ shipped ~73 font binaries, several of them commercial
typefaces (Aktiv Grotesk, Atlas Grotesk, Bebas Neue Pro) that shouldn't be
redistributed from a public repo. Fonts/README.md now documents Helvetica
Neue (via Adobe CC, installed locally) as the default, with free/open
alternatives listed but not bundled.

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 3: Numbered restructure, README rewrite, new TODO.md

**Files:**
- Rename: `Plotting/` → `01-Plotting/`
- Rename: `IllustratorsStyles/` → `02-IllustratorsStyles/`
- Rename: `BlenderBoilerPlates/` → `03-BlenderBoilerPlates/`
- Rename: `Fonts/` → `04-Fonts/`
- Modify: `01-Plotting/setup.py` (one URL string)
- Modify: `02-IllustratorsStyles/README.md` (one relative path)
- Modify: `04-Fonts/README.md` (one relative path, from Task 2)
- Modify: `README.md` (full rewrite)
- Create: `TODO.md`

**Interfaces:** N/A.

- [ ] **Step 1: Rename the four top-level folders**

```bash
git mv Plotting 01-Plotting
git mv IllustratorsStyles 02-IllustratorsStyles
git mv BlenderBoilerPlates 03-BlenderBoilerPlates
git mv Fonts 04-Fonts
```

`git mv` on a directory containing a modified-but-uncommitted tracked file stages only the rename (old content at the new path) — the content modification itself stays unstaged at the new path. This is exactly what we want: it preserves the pyprettyplot WIP as an uncommitted diff, just relocated. Untracked files/directories inside the renamed folder move on disk automatically and remain untracked.

- [ ] **Step 1b: Confirm the pyprettyplot WIP moved but stayed uncommitted/untracked**

```bash
git status --short | grep pyprettyplot
```

Expected output (paths now under `01-Plotting/`, each modification still unstaged — leading space before `M` on the second column of the `RM` entries, and `??` for the untracked ones):
```
RM Plotting/pyprettyplot/__init__.py -> 01-Plotting/pyprettyplot/__init__.py
RM Plotting/pyprettyplot/colors.py -> 01-Plotting/pyprettyplot/colors.py
RM Plotting/pyprettyplot/dispersion.py -> 01-Plotting/pyprettyplot/dispersion.py
?? 01-Plotting/pyprettyplot/fonts/
?? 01-Plotting/pyprettyplot/matplotlibrc
```
(exact ordering may vary; `01-Plotting/_pyprettyplot/` shows as its own untracked line elsewhere in `git status --short`, not matched by the `pyprettyplot` grep pattern above — check for it separately with `git status --short | grep _pyprettyplot`).

If any of these show as fully staged (`M ` in the second column with no `R`, or the diff below is non-empty in `--cached`), STOP — do not proceed to Step 8's commit without first running `git restore --staged <file>` on the offending file(s).

- [ ] **Step 2: Fix the stale path in setup.py**

In `01-Plotting/setup.py`, find:

```python
    url="https://github.com/gregmoille/ScientificGraphicDesign/tree/main/Plotting/pyprettyplot",
```

Replace with:

```python
    url="https://github.com/gregmoille/ScientificGraphicDesign/tree/main/01-Plotting/pyprettyplot",
```

(Keep the `gregmoille` org name here — the actual GitHub org transfer is a separate manual step tracked in `TODO.md`; don't assume it's happened yet.)

- [ ] **Step 3: Fix the cross-folder relative path in IllustratorsStyles README**

In `02-IllustratorsStyles/README.md`, find:

```
- [IBM colorpalette](https://github.com/IBM-Design/colors/blob/master/ibm-colors.ase): comes from the IBM design language which to be honest is a must to scroll through to understand how to make unified good design. It is also consisten with the [prettyplot class](../Plotting/pyprettyplot/), which include the `ibm` class to retrieve the colors (for instance `ibm.cerulean(shade = 60)`). 
```

Replace with:

```
- [IBM colorpalette](https://github.com/IBM-Design/colors/blob/master/ibm-colors.ase): comes from the IBM design language which to be honest is a must to scroll through to understand how to make unified good design. It is also consisten with the [prettyplot class](../01-Plotting/pyprettyplot/), which include the `ibm` class to retrieve the colors (for instance `ibm.cerulean(shade = 60)`). 
```

- [ ] **Step 4: Fix the cross-folder relative path in Fonts README**

In `04-Fonts/README.md` (written in Task 2 Step 2), find:

```
Designing a font is a genuinely hard, specialized craft — that's why the
good ones aren't free.

# Want to go down the rabbit hole?

A deeper typography write-up (serif vs. slab vs. sans-serif, kerning and
width, glyph coverage, and how to pick a font as a scientist) hasn't been
written yet — tracked in [TODO.md](../TODO.md). For scientific graphic
design tips more broadly, see [../IllustratorsStyles](../IllustratorsStyles).
```

Replace with:

```
Designing a font is a genuinely hard, specialized craft — that's why the
good ones aren't free.

# Want to go down the rabbit hole?

A deeper typography write-up (serif vs. slab vs. sans-serif, kerning and
width, glyph coverage, and how to pick a font as a scientist) hasn't been
written yet — tracked in [TODO.md](../TODO.md). For scientific graphic
design tips more broadly, see [../02-IllustratorsStyles](../02-IllustratorsStyles).
```

- [ ] **Step 5: Rewrite the root README.md**

Replace the entire contents of `README.md` with:

```markdown
# Scientific Graphic Design

This repository is the entry point for making publication-quality figures
in the Srinivasan Group at JQI. It covers the tools and habits used across
the group to turn data into clear, professional figures: plotting in
Python/Plotly, finishing in Adobe Illustrator, and 3D rendering in
Blender.

Feel free to tweak and contribute via pull requests. If you use tips or
assets from here, let Greg Moille know by [email](mailto:gmoille@umd.edu)
or any other [means](https://srinivasan.jqi.umd.edu/people/gregory-moille)
— always glad to hear it's being put to good use.

# What is it about?

A compilation of tips, tricks, and boilerplate assets for making scientific
figures, gathered over years of preparing figures for publication.

# How to use this repository?

This repository is organized by graphic design aspect, numbered in the
suggested reading order for someone new to the group. If you're only
after boilerplates or example files, jump directly into the relevant
folder without reading the tutorials.

# Basic Graphic Design

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

# Figure for Publication

A compilation of tips and tricks for plotting, tweaking, and creating figures for scientific publication, in suggested reading order:

1. [Plotting data](./01-Plotting/)
2. [Adobe Illustrator styles and tips](./02-IllustratorsStyles)
3. [Blender boiler-plate files and assets](./03-BlenderBoilerPlates)
4. [Fonts](./04-Fonts)

# Making slides

Slide-deck and presentation guidance now lives in the
[ScientificPresentations](https://github.com/JQInanophotonics/ScientificPresentations)
repo.

# Related repos

Part of the [JQInanophotonics](https://github.com/JQInanophotonics) org:
- [ScientificPresentations](https://github.com/JQInanophotonics/ScientificPresentations) — talk design and slide decks
- [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement) — data collection and archival
- [ScientificWriting](https://github.com/JQInanophotonics/ScientificWriting) — paper composition and citation practices
- [InstrumentControl](https://github.com/JQInanophotonics/InstrumentControl) — lab equipment operation
- [3DPrintedLabParts](https://github.com/JQInanophotonics/3DPrintedLabParts) — shared fabricated part designs
```

- [ ] **Step 6: Create TODO.md**

Create `TODO.md` at the repo root with:

```markdown
# TODO

Tracked follow-up work deliberately deferred during the 2026-07 repo
tidy-up (see `docs/superpowers/specs/2026-07-03-repo-tidy-up-design.md`).

## pyprettyplot (01-Plotting)
- Resolve the duplicate package state: tracked `pyprettyplot/` vs
  untracked `_pyprettyplot/` — decide on one name/location and remove the
  other.
- Update the package's default font references (currently Aktiv Grotesk
  Condensed / Decima) to Helvetica Neue, matching the new repo-wide
  default documented in `04-Fonts/README.md`.
- Clean up untracked stray files (`matplotlibrc`, `fonts/`) once the
  package structure is finalized.

## 03-BlenderBoilerPlates
- `Assets/Assests.blend` referenced by the old README is missing —
  recreate the asset file, or remove the "Assets Available" section for
  good if it's not coming back.
- Write descriptions for `Ring3DDisperiveGlass_Gears.blend` and
  `Sync3Drender.blend` (currently just listed by filename).
- Fill in the "What is it?" / "How to use it?" / "Quick reminder for
  Blender and Settings" sections.

## 04-Fonts
- Document the local Helvetica Neue install/activation steps via Adobe
  Creative Cloud.
- Write the typography deep-dive (serif vs. slab vs. sans-serif, kerning
  and width, glyph coverage, picking a font as a scientist).

## Repo administration
- Transfer this repo from `gregmoille/ScientificGraphicDesign` to the
  `JQInanophotonics` GitHub org (Settings → Transfer ownership).
```

- [ ] **Step 7: Verify the new structure and links**

```bash
ls -d 01-Plotting 02-IllustratorsStyles 03-BlenderBoilerPlates 04-Fonts TODO.md
```

Expected: all five paths listed, no error.

```bash
test -d 01-Plotting/pyprettyplot && test -f 01-Plotting/README.md && test -f 02-IllustratorsStyles/README.md && test -f 03-BlenderBoilerPlates/README.md && test -f 04-Fonts/README.md && echo "STRUCTURE OK"
```

Expected: `STRUCTURE OK`

```bash
grep -c "Plotting/pyprettyplot\"" 01-Plotting/setup.py
```

Expected: `0` (old un-numbered path string is gone — the new one has `01-Plotting/pyprettyplot` which won't match this exact grep pattern)

```bash
grep -n "\.\./Plotting/pyprettyplot" 02-IllustratorsStyles/README.md
grep -n "\.\./IllustratorsStyles" 04-Fonts/README.md
```

Expected: no output from either (both stale relative paths fixed).

- [ ] **Step 8: Commit**

**Do NOT run a bare `git add -A`** — it would stage the pyprettyplot WIP's unstaged content modifications (confirmed unstaged in Step 1b). Exclude those exact paths explicitly:

```bash
git add -A -- . \
  ':!01-Plotting/pyprettyplot/__init__.py' \
  ':!01-Plotting/pyprettyplot/colors.py' \
  ':!01-Plotting/pyprettyplot/dispersion.py' \
  ':!01-Plotting/_pyprettyplot' \
  ':!01-Plotting/pyprettyplot/fonts' \
  ':!01-Plotting/pyprettyplot/matplotlibrc'
git status --short | grep pyprettyplot
```

Expected: same `RM`/`??` pattern as Step 1b — none of the six excluded paths became fully staged. If any did, `git restore --staged <file>` it before committing.

```bash
git commit -m "$(cat <<'EOF'
Restructure into numbered sections, rewrite README, add TODO.md

Plotting -> 01-Plotting, IllustratorsStyles -> 02-IllustratorsStyles,
BlenderBoilerPlates -> 03-BlenderBoilerPlates, Fonts -> 04-Fonts, giving
new members a suggested reading order. Root README reframed for a
group-wide audience; slide-deck section now points to the
ScientificPresentations repo. TODO.md tracks work deferred during this
cleanup (pyprettyplot duplication, missing Blender assets, font install
docs, org transfer).

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 4: Per-folder fixes (Blender broken links/typos, Illustrator banner/credit)

**Files:**
- Modify: `03-BlenderBoilerPlates/README.md` (full rewrite)
- Delete: `03-BlenderBoilerPlates/MiscModels/RingCartoon.blend1`
- Modify: `02-IllustratorsStyles/README.md` (banner → title, add ScientificColorMap credit)

**Interfaces:** N/A.

- [ ] **Step 1: Rewrite 03-BlenderBoilerPlates/README.md**

Replace the entire contents of `03-BlenderBoilerPlates/README.md` with:

```markdown
# Blender Boilerplates

Boilerplate Blender assets and ready-made renders for photonics-style
figure illustrations (ring resonators, waveguides, etc.).

_"What is it?" / "How to use it?" / general Blender settings walkthrough
hasn't been written yet — tracked in [TODO.md](../TODO.md)._

## Assets Available

This section previously documented an `Assets.blend` file with reusable
geometry (straight waveguide ring resonator, pulley-coupled ring
resonator, photonic crystal ring resonator, single soliton in a
microring). That file is no longer present in the `Assets/` folder —
tracked in [TODO.md](../TODO.md) to recreate it or remove this section.

## Ready-made models

Four rendered `.blend` files are provided and can be tweaked as needed:
- [RingCartoon.blend](./MiscModels/RingCartoon.blend): toon-like render of
  a microring resonator. Useful for papers — gives a neat, professional
  render without the fuzz of a pseudo-realistic render. Handy to bring
  into Adobe Illustrator to annotate parameters on the system. Background
  is transparent. Rendered in Eevee.
- [CladdingRing.blend](./MiscModels/CladdingRing.blend): a more realistic
  render with some fuzz and extra detail. Rendered in Eevee.
- [Ring3DDisperiveGlass_Gears.blend](./MiscModels/Ring3DDisperiveGlass_Gears.blend)
  and [Sync3Drender.blend](./MiscModels/Sync3Drender.blend): additional
  renders — descriptions not yet written, see [TODO.md](../TODO.md).

## Creating an image

Play with the camera angle until you get the shot you want, then go to
Render → Render Image, and save as PNG/TIFF/whatever format you need.

## Some tutorials

[![Demo CountPages alpha](https://img.youtube.com/vi/rj1BpFC8rmU/0.jpg)](https://youtu.be/rj1BpFC8rmU?si=9B-U8giXcsKT2VlK)
```

- [ ] **Step 2: Remove the stray Blender autosave file**

```bash
git rm 03-BlenderBoilerPlates/MiscModels/RingCartoon.blend1
```

- [ ] **Step 3: Fix the banner and add ScientificColorMap credit in 02-IllustratorsStyles/README.md**

Find:

```
# 🏗️ Under Constructions🏗️

# Document Settings
```

Replace with:

```
# Illustrator Styles & Setup

# Document Settings
```

Then find:

```
# Colors Palettes

- [IBM colorpalette](https://github.com/IBM-Design/colors/blob/master/ibm-colors.ase): comes from the IBM design language which to be honest is a must to scroll through to understand how to make unified good design. It is also consisten with the [prettyplot class](../01-Plotting/pyprettyplot/), which include the `ibm` class to retrieve the colors (for instance `ibm.cerulean(shade = 60)`). 
```

Replace with:

```
# Colors Palettes

- [IBM colorpalette](https://github.com/IBM-Design/colors/blob/master/ibm-colors.ase): comes from the IBM design language which to be honest is a must to scroll through to understand how to make unified good design. It is also consisten with the [prettyplot class](../01-Plotting/pyprettyplot/), which include the `ibm` class to retrieve the colors (for instance `ibm.cerulean(shade = 60)`). 
- [ScientificColorMap](./ColorPalettes/ScientificColorMap/) (`.ase` swatches): Fabio Crameri's [Scientific Colour Maps](https://www.fabiocrameri.ch/colourmaps/), redistributed here under their [open license terms](https://www.fabiocrameri.ch/ws/media-library/ce2eb6eee7c345f999e61c02e2733962/readme_scientificcolourmaps.pdf#Acknowledgement) — when you use these in a figure, credit Crameri's work as described there. Recommended for 2D map plots: `batlow`, `oslo` (or `oslo_r`), `lipari`.
```

- [ ] **Step 4: Verify the fixes**

```bash
grep -n "RingCaroon" 03-BlenderBoilerPlates/README.md
```

Expected: no output (typo fixed — old text used `RingCaroon.blend`, correct filename is `RingCartoon.blend`).

```bash
grep -n "Assests.blend" 03-BlenderBoilerPlates/README.md
```

Expected: no output (typo fixed).

```bash
test ! -e 03-BlenderBoilerPlates/MiscModels/RingCartoon.blend1 && echo "AUTOSAVE REMOVED"
```

Expected: `AUTOSAVE REMOVED`

```bash
grep -n "Under Constructions" 02-IllustratorsStyles/README.md
```

Expected: no output.

```bash
grep -n "Scientific Colour Maps" 02-IllustratorsStyles/README.md
```

Expected: one match (the new credit line).

```bash
find 03-BlenderBoilerPlates/MiscModels -type f
```

Expected:
```
03-BlenderBoilerPlates/MiscModels/RingCartoon.blend
03-BlenderBoilerPlates/MiscModels/CladdingRing.blend
03-BlenderBoilerPlates/MiscModels/Ring3DDisperiveGlass_Gears.blend
03-BlenderBoilerPlates/MiscModels/Sync3Drender.blend
```
(order may vary — 4 files, no `.blend1`)

- [ ] **Step 5: Commit**

```bash
git add -A 03-BlenderBoilerPlates 02-IllustratorsStyles
git commit -m "$(cat <<'EOF'
Fix broken Blender references and Illustrator README banner/credit

BlenderBoilerPlates README pointed to a filename typo (RingCaroon vs
actual RingCartoon) and a missing Assets.blend file; both now documented
accurately, with gaps tracked in TODO.md. Removed a stray Blender
autosave file (.blend1). IllustratorsStyles README's stale
'Under Construction' banner replaced with a real title, and the
ScientificColorMap .ase files now have an explicit CC attribution line.

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

## Final full-repo verification (after all 4 tasks)

- [ ] Run the following and confirm no errors:

```bash
git log --oneline -4
git status --short
ls -d 01-Plotting 02-IllustratorsStyles 03-BlenderBoilerPlates 04-Fonts TODO.md README.md
test ! -e 3Dprinting && test ! -e img && test ! -e SlideDeckFigures && test ! -e SlidesWithBeamer && test ! -d Fonts && test ! -d Plotting && test ! -d IllustratorsStyles && test ! -d BlenderBoilerPlates && echo "ALL CLEAR"
```

Expected: `git status --short` shows only the pre-existing untouched WIP (`Plotting/` no longer exists so these paths become `01-Plotting/pyprettyplot/...` modified/untracked entries — same WIP, new path), 4 new commits in `git log`, and `ALL CLEAR` printed.

- [ ] Report back to the user with the final `git log --oneline -5` and ask whether to push, and remind them the GitHub org transfer (`gregmoille/ScientificGraphicDesign` → `JQInanophotonics`) is still a manual step tracked in `TODO.md`.
