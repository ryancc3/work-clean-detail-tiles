# Canfor Summer Tiles

This folder is ready to publish with GitHub Pages.

The tile URL template is:

```text
https://ryancc3.github.io/work-clean-detail-tiles/tiles/{z}/{x}/{y}.png
```

Recommended cache-busting URL:

```text
https://ryancc3.github.io/work-clean-detail-tiles/tiles/{z}/{x}/{y}.png?v=canfor-summer-20260705
```

Use that URL as a custom map source in Gaia.

## What This Contains

- Transparent PNG web tiles in `tiles/{z}/{x}/{y}.png`
- Zooms 10 through 17
- Zooms 10 through 15 use buffered, high-resolution block overview tiles
  with all 22 labels drawn at every overview zoom.
- Overview labels are placed globally and rendered with tile buffers, so labels
  and leader arrows can cross internal tile boundaries without being clipped.
- Overview block shading, halo, and stroke are rendered with tile buffers to
  avoid cut-off polygon edges where sheets overlap or cross tile boundaries.
- Zooms 16 through 17 use detailed field-map tiles.
- Plot/source PDFs are preferred for the source selection.
- Isolated maps at detail zooms render as full unedited GeoPDF detail.
- Overlapping maps at detail zooms render through the cleaned detail path to reduce
  stacked legends, inset maps, borders, and large note panels.
- Matching non-plot PDFs are only used when replacement priority is explicitly requested.
- Extra seasonal source folders are included when supplied during the rebuild.

## Source Selection

- Effective maps: `22`
- Source priority: `plot/source PDFs`
- Non-plot replacements used: `0`
- Fallback maps used: `22`
- Unused replacement PDFs are listed in `source_selection_report.json`.
- Source folders are listed in `source_selection_report.json`.

## Bounds

- West: `-122.56880`
- South: `53.50330`
- East: `-121.70790`
- North: `54.43810`

## GitHub Pages Setup

1. Put the contents of this folder at the root of the repository.
2. Push it to GitHub.
3. In the repository, use GitHub Pages from the main branch root or the
   `gh-pages` branch root.
4. Use the published Pages URL with `/tiles/{z}/{x}/{y}.png` at the end.

Do not use Git LFS for these tiles. GitHub Pages needs to serve the actual PNG files.
