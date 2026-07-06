# Gaia Canfor Summer Layer Rules

These are the rules used for the current GitHub Pages Gaia custom map layer.

## Source Layers

- Web rebuild script: `scripts/build_web_tiles.py`
- Source PDFs: `Canfor Summer maps`
- Web output: `Gaia_MBTiles/github_pages_site`
- Published URL template: `https://ryancc3.github.io/work-clean-detail-tiles/tiles/{z}/{x}/{y}.png`
- Recommended cache-busting URL: `https://ryancc3.github.io/work-clean-detail-tiles/tiles/{z}/{x}/{y}.png?v=canfor-summer-fullpdf`

The current published layer is Canfor Summer only. The previous original
`GEOPDFs` maps are not included in this tile set.

Current source-selection target:

- `22` effective maps
- `0` non-plot replacements
- `22` source PDF fallbacks
- Source priority: plot/source PDFs
- No source or georeferencing failures

## Zoom Behavior

- Web zooms `10-15`: simplified high-resolution overview tiles.
- Web zooms `16-17`: opaque full source GeoPDF detail tiles.

The overview is intentionally a different cartographic style than the detailed
field maps so the block locations stay readable when zoomed out.

## Overview Rules

For web zooms `10-15`:

- Use block-only detection from the 22 Canfor Summer PDFs.
- Generate 1024 px transparent PNG overview tiles.
- Draw all `22` labels at every overview zoom.
- Place labels globally per zoom before tile rendering, so labels are not clipped
  by internal tile edges.
- Render block fill, halo, stroke, labels, and leader arrows with a 512 px tile
  buffer before cropping to avoid cut-off shading or text.
- Use leader arrows only where labels need to sit beside a block.

Expected overview label counts:

- zoom `10`: `22`
- zoom `11`: `22`
- zoom `12`: `22`
- zoom `13`: `22`
- zoom `14`: `22`
- zoom `15`: `22`

## Detail Rules

For web zooms `16-17`, render every Canfor Summer map directly from the selected
source GeoPDF.

- Use plot/source PDFs as the priority source.
- Do not use the cleaned-detail path.
- Do not remove block insets, legends, page borders, notes, or other PDF
  furniture.
- Preserve white/background pixels, so each PDF footprint is opaque and can cover
  the BRMB/base map beneath it.
- Record every detail map as `opaque_full_geopdf` in
  `detail_generation_summary.json`.

## Verification Targets

`manifest.json` should report:

- `22` effective maps
- `0` replacements
- `22` fallbacks
- overview zooms `10-15`
- detail zooms `16-17`
- cache-busting URL ending in `?v=canfor-summer-fullpdf`

`source_selection_report.json` should report:

- one source directory: `Canfor Summer maps`
- no source failures
- no replacement georeferencing failures
- no duplicate source block codes

Tile dimensions should be:

- z`10-15`: `1024x1024`
- z`16-17`: `256x256`
