# Work Non-Plot Hybrid Tiles

This folder is ready to publish with GitHub Pages.

The tile URL template is:

```text
https://ryancc3.github.io/work-clean-detail-tiles/tiles/{z}/{x}/{y}.png
```

Recommended cache-busting URL:

```text
https://ryancc3.github.io/work-clean-detail-tiles/tiles/{z}/{x}/{y}.png?v=nonplot-z15
```

Use that URL as a custom map source in Gaia.

## What This Contains

- Transparent PNG web tiles in `tiles/{z}/{x}/{y}.png`
- Zooms 10 through 17
- Zooms 10 through 15 use buffered, high-resolution block overview tiles
  with all 74 labels drawn at every overview zoom.
- Overview labels are placed globally and rendered with tile buffers, so labels
  and leader arrows can cross internal tile boundaries without being clipped.
- Overview block shading, halo, and stroke are rendered with tile buffers to
  avoid cut-off polygon edges where sheets overlap or cross tile boundaries.
- Zooms 16 through 17 use detailed field-map tiles with roads,
  contours, streams, block outlines/fills, boundary lines, and useful operational labels.
- Matching non-plot PDFs replace plot-map PDFs by block code; unmatched maps use
  the original `GEOPDFs` source.

## Source Selection

- Effective maps: `74`
- Non-plot replacements used: `40`
- Fallback maps used: `34`
- Unused replacement PDFs are listed in `source_selection_report.json`.

## Bounds

- West: `-115.75681`
- South: `48.98985`
- East: `-114.40327`
- North: `50.55049`

## GitHub Pages Setup

1. Put the contents of this folder at the root of the repository.
2. Push it to GitHub.
3. In the repository, use GitHub Pages from the main branch root or the
   `gh-pages` branch root.
4. Use the published Pages URL with `/tiles/{z}/{x}/{y}.png` at the end.

Do not use Git LFS for these tiles. GitHub Pages needs to serve the actual PNG files.
