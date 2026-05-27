# Gaia Hybrid Layer Rules

These are the rules used for the current Gaia web map layer and the previous
mobile sync layer.

## Source Layers

- Web rebuild script: `scripts/build_web_tiles.py`
- Source PDFs: `GEOPDFs`
- Replacement PDFs: `Non-plot maps`
- Web output: `github_pages_site`
- Previous mobile detailed map source: `work_clean_detail_gaia.mbtiles`
- Previous mobile overview block source: `work_blocks_only_gaia.mbtiles`

For the web rebuild, matching non-plot PDFs replace plot-map PDFs by block code.
Unmatched plot maps and numeric block maps fall back to `GEOPDFs`.

Current web source-selection target:

- `74` effective maps
- `40` non-plot replacements
- `34` fallbacks
- `WER0056` reported as an unused non-plot replacement

## Zoom Behavior

- Web zooms `10-15`: simplified high-resolution overview tiles.
- Web zooms `16-17`: full clean-detail tiles.
- Previous mobile sync zooms `10-12`: simplified overview tiles.
- Previous mobile sync zooms `13-17`: full clean-detail tiles.

The overview is intentionally not a downscaled detailed map. It is a different
cartographic style so the map remains readable when zoomed out.

## Clean Detail Rules

For current web zooms `16-17`, keep:

- roads
- contours
- streams
- block outlines and fills
- boundary lines
- useful operational map labels

Remove:

- slope inset maps
- slope legends
- yellow warning note boxes
- page borders and ticks
- compass marks
- contractor text
- Consult Plant Wizard boxes
- block seed panels

## Overview Geometry Rules

For current web zooms `10-15`:

- Use block-only detection from the effective source PDF set.
- Generate 1024 px PNG overview tiles.
- Use transparent backgrounds.
- Render with a 512 px tile buffer/metatile margin before cropping, so block
  fill, halo, stroke, labels, and arrows can cross internal tile boundaries
  without being clipped.
- Draw block geometry with:
  - white halo
  - black stroke
  - boosted block fill colors
- If a fallback plot map only provides a boundary/outline or a pale fill, close
  the outline mask and use strong amber overview fill.
- Use the following halo/stroke pixel radii:
  - zoom `10`: halo `35`, stroke `19`
  - zoom `11`: halo `29`, stroke `15`
  - zoom `12`: halo `23`, stroke `11`
  - zoom `13`: halo `17`, stroke `8`
  - zoom `14`: halo `13`, stroke `6`
  - zoom `15`: halo `9`, stroke `4`
- Fill opacity target: `220`.

## Label Anchor Rules

Overview labels are not placed at the center of the PDF page. They are anchored
to actual detected block geometry:

1. Build the effective source set, replacing plot maps with matching non-plot
   maps where available.
2. Render each effective GeoPDF through the block-only detector at `300 dpi`.
3. Use the alpha mask of the detected block-only image.
4. Keep the largest detected connected component.
5. Compute an alpha-weighted centroid for that component.
6. Convert that pixel centroid back to Web Mercator/geographic coordinates with
   the inverse GeoPDF homography.
7. Store the resulting anchors in `github_pages_site/overview_label_anchors.json`.

The current anchor set has `74` labels.

## Label Placement Rules

For every current web overview zoom (`10` through `15`), draw all `74` labels.

Use variable font sizes:

- zoom `10`: `42, 38, 34, 30, 26, 22`
- zoom `11`: `48, 44, 40, 36, 32, 28`
- zoom `12`: `54, 50, 46, 42, 38, 34`
- zoom `13`: `60, 56, 52, 48, 44, 40`
- zoom `14`: `66, 62, 58, 54, 50, 46`
- zoom `15`: `72, 68, 64, 60, 56, 52`

Placement rules:

- Sort labels by edge proximity first, then by label length.
- Prefer larger labels close to the block anchor.
- Avoid label-label overlap where possible.
- If a dense cluster cannot fit at the preferred size, use a smaller font.
- If a label needs to sit beside the block, draw a leader arrow from the label
  box back to the block anchor.
- Labels are placed globally per zoom, then rendered into buffered tiles so
  names are not clipped by internal tile boundaries.
- Do not skip labels in the final overview style. The final pass draws all 74
  labels at all overview zooms.

Leader arrow rules:

- Draw arrows under labels.
- Use a white halo line, then a black line.
- Use a filled arrow head at the block anchor.

## Final Verification Targets

`github_pages_site/overview_generation_summary.json` should report:

- zoom `10`: `74` labels
- zoom `11`: `74` labels
- zoom `12`: `74` labels
- zoom `13`: `74` labels
- zoom `14`: `74` labels
- zoom `15`: `74` labels

`github_pages_site/source_selection_report.json` should report:

- `74` effective maps
- `40` replacements
- `WER0056` as unused
- no source or georeferencing failures

The final web custom-source URL is:

```text
https://ryancc3.github.io/work-clean-detail-tiles/tiles/{z}/{x}/{y}.png
```

Recommended cache-busting URL after the non-plot/z15 overview update:

```text
https://ryancc3.github.io/work-clean-detail-tiles/tiles/{z}/{x}/{y}.png?v=nonplot-z15
```

## Mobile Packaging Rules

The existing Gaia mobile sync layer was not rebuilt for the non-plot/z15 web
update. It still uses the previous hybrid MBTiles layer:

- Copy overview XYZ PNG tiles from `github_pages_site/tiles` for zooms `10-12`.
- Convert XYZ row numbers to MBTiles/TMS row numbers.
- Copy detail tiles from `work_clean_detail_gaia.mbtiles` for zooms `13-17`.
- Set MBTiles metadata to `type=overlay`, `format=png`, `scheme=tms`,
  `minzoom=10`, and `maxzoom=17`.

The combined mobile file is:

```text
work_hybrid_overview_detail_gaia.mbtiles
```

For Gaia sync, split the combined layer into files under `100 MB`:

```text
sync_parts_hybrid/work_hybrid_sync_part01_of06.mbtiles
sync_parts_hybrid/work_hybrid_sync_part02_of06.mbtiles
sync_parts_hybrid/work_hybrid_sync_part03_of06.mbtiles
sync_parts_hybrid/work_hybrid_sync_part04_of06.mbtiles
sync_parts_hybrid/work_hybrid_sync_part05_of06.mbtiles
sync_parts_hybrid/work_hybrid_sync_part06_of06.mbtiles
```

All six mobile parts should be imported and enabled together in Gaia.
