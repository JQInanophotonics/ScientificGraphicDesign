# ScientificGraphicDesign repo tidy-up — design

Date: 2026-07-03

## Context

`ScientificGraphicDesign` is meant to be the entry-point repo of the
JQInanophotonics GitHub org (Srinivasan Group at JQI): "Practical, no-nonsense
guides for the day-to-day skills of doing and publishing research in the
group." New members are pointed here first to learn the tools (Illustrator,
Blender, plotting) used across the group's other repos.

The repo grew organically as Greg's personal notes and has accumulated
hygiene problems that don't fit its role as a shared, public teaching
resource:

- An accidental nested git clone (`3Dprinting/`, containing its own `.git`)
  of the sibling `3DPrintedLabParts` org repo.
- Slide-deck content (`SlideDeckFigures/`, `SlidesWithBeamer/`) that
  duplicates the scope of the sibling `ScientificPresentations` org repo;
  their READMEs are already deleted in the working tree, mid-migration.
- `Fonts/CooldFonts/` bundles ~73 font files, several of which are
  commercial/paid typefaces (Aktiv Grotesk, Atlas Grotesk, Bebas Neue Pro,
  Decima) — a redistribution/licensing risk in a public repo.
- A mid-refactor duplicate Python package in `Plotting/`
  (`pyprettyplot/` tracked + untracked `_pyprettyplot/`), plus stray
  untracked files (`matplotlibrc`, `fonts/`).
- Broken references: `BlenderBoilerPlates/README.md` points to
  `Assets/Assests.blend`, which doesn't exist; a filename typo
  (`RingCaroon.blend` vs actual `RingCartoon.blend`); a stray Blender
  autosave file (`RingCartoon.blend1`); two undocumented `.blend` models.
- README tone is personal/casual ("🏗️ Under Construction🏗️", "email me",
  "borrow my books") rather than framed as a group resource.
- The repo remote is still `gregmoille/ScientificGraphicDesign` (personal),
  not transferred to the `JQInanophotonics` org.

## Goals

- Restructure the top level into a numbered, ordered reading path.
- Remove content that now belongs in sibling org repos.
- Remove the font-licensing risk.
- Fix broken links/typos discovered during the audit.
- Rewrite the root README for a group-wide (not personal) audience.
- Capture deferred work (things intentionally *not* fixed now) in a
  `TODO.md` so it isn't lost.
- Flag (not necessarily execute) the GitHub org-transfer step.

## Non-goals (explicitly out of scope for this pass)

- Resolving the `pyprettyplot`/`_pyprettyplot` duplication or otherwise
  editing the package's in-progress code — tracked in `TODO.md` instead.
- Writing new teaching content for empty stub sections (typography
  deep-dive, Blender basics, etc.) — tracked in `TODO.md` instead.
- Reviewing/trimming the `ScientificColorMap` `.ase` palette set — it's
  properly licensed (CC-BY, public), kept as-is aside from clarifying
  attribution wording.
- Actually performing the GitHub repo-transfer to the org (administrative
  action requiring the user's GitHub auth/consent) — flagged as a manual
  follow-up, not scripted.

## Target structure

```
ScientificGraphicDesign/
├── README.md                      (rewritten, group-audience tone)
├── TODO.md                        (new)
├── .gitignore
├── 01-Plotting/                   (was Plotting/)
├── 02-IllustratorsStyles/         (was IllustratorsStyles/)
├── 03-BlenderBoilerPlates/        (was BlenderBoilerPlates/)
└── 04-Fonts/                      (was Fonts/, font binaries removed)
```

Removed entirely:
- `SlideDeckFigures/`, `SlidesWithBeamer/` — content responsibility moves to
  `ScientificPresentations` org repo. (No migration of content is performed
  as part of this repo's cleanup — the README banks were already deleted;
  we finalize the removal here. Moving/recreating equivalent content in
  `ScientificPresentations` is out of scope for this repo's spec.)
- `3Dprinting/` — accidental nested clone, unrelated to this repo.
- `img/` — empty, unreferenced folder at root.

## Per-folder actions

### `01-Plotting/` (was `Plotting/`)
- Rename folder only; leave `pyprettyplot/`, the untracked
  `_pyprettyplot/`, `matplotlibrc`, and `fonts/` untouched (active WIP).
- Stage deletion of already-removed `__pycache__/*.pyc` files (predates
  `.gitignore`).
- Remove `.DS_Store`.
- Add a `TODO.md` entry for the package duplication/font-reference cleanup.

### `02-IllustratorsStyles/` (was `IllustratorsStyles/`)
- Rename folder only.
- Remove stale "🏗️ Under Construction🏗️" banner (content is complete).
- Tighten the Scientific Colour Maps credit line so attribution to Fabio
  Crameri (CC-BY) is unambiguous rather than a soft "don't forget to
  acknowledge."
- Update the `pyprettyplot` cross-reference path for the new folder number.
- Update font references from Aktiv Grotesk Condensed/Decima to Helvetica
  Neue (see Fonts section).

### `03-BlenderBoilerPlates/` (was `BlenderBoilerPlates/`)
- Rename folder only.
- Fix broken reference to missing `Assets/Assests.blend` — remove the
  now-incorrect instructions, replace with a TODO note rather than
  presenting broken steps as working.
- Fix `RingCaroon.blend` → `RingCartoon.blend` typo.
- Delete `RingCartoon.blend1` (Blender autosave junk, shouldn't be
  committed).
- List `Ring3DDisperiveGlass_Gears.blend` and `Sync3Drender.blend`
  (currently undocumented / referenced only as a dangling `[???]()` link)
  instead of leaving the broken link.
- Leave empty "What is it?" / "How to use it?" sections as an explicit
  TODO rather than silently deleting them.

### `04-Fonts/` (was `Fonts/`)
- Rename folder only.
- Delete all bundled font binaries under `CooldFonts/` (commercial
  typefaces: Aktiv Grotesk, Atlas Grotesk, Bebas Neue Pro, Decima, etc.).
- Rewrite README to recommend Helvetica Neue (available via Adobe Creative
  Cloud, must be installed/activated locally — activation steps deferred
  to TODO.md) as the primary font, replacing prior Aktiv Grotesk/Decima
  defaults referenced across the repo's docs.
- Remove empty "rabbit hole" stub sections (serif vs slab, kerning, glyph
  coverage, etc.); note them in TODO.md instead of leaving blank headers.

## README.md rewrite

- Drop the "🏗️ Under Construction🏗️" banner and the personal
  disclaimer framing ("email me", "borrow my books").
- Reframe the intro: this is the JQInanophotonics group's entry-point guide
  for figure-making tools, matching the org's stated purpose.
- Preserve the existing book list and core design-tips content.
- Update section links to the four numbered folders.
- Remove the "Making Slides" section; replace with a one-line pointer to
  `ScientificPresentations`.
- Add a short "Related repos" section linking sibling org repos
  (ScientificPresentations, ScientificDataManagement, ScientificWriting,
  InstrumentControl, 3DPrintedLabParts).

## TODO.md (new file)

Tracks everything explicitly deferred by this pass:
- `pyprettyplot`/`_pyprettyplot` duplication resolution + font-reference
  update inside the package itself.
- Missing `Assets/Assests.blend`, undocumented Blender models, empty
  Blender README sections.
- Documenting local Helvetica Neue install/activation via Adobe CC.
- Fleshing out typography deep-dive stub sections, if wanted later.
- Transferring the repo from `gregmoille/ScientificGraphicDesign` to the
  `JQInanophotonics` GitHub org (manual/administrative step).

## Git hygiene

- Land the work as a small stack of focused commits rather than one giant
  diff, roughly:
  1. Remove `3Dprinting/`, `SlideDeckFigures/`, `SlidesWithBeamer/`, `img/`,
     stray `.DS_Store`/`__pycache__` cruft.
  2. Font licensing cleanup (delete commercial fonts, rewrite `Fonts`
     README and cross-references).
  3. Numbered restructure (folder renames) + root `README.md` rewrite +
     new `TODO.md`.
  4. Per-folder fixes (Blender broken links/typos, IllustratorsStyles
     banner/credit wording).
- `.gitignore` already covers `.venv`, `__pycache__`, `.DS_Store` —
  no changes needed there.
- Repo transfer to the `JQInanophotonics` org is a GitHub administrative
  action (Settings → Transfer ownership) requiring the user's explicit
  action/consent — not scripted as part of implementation.
