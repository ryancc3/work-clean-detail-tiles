# Work Clean Detail Tiles

This folder is ready to publish with GitHub Pages.

The tile URL template is:

```text
https://ryancc3.github.io/work-clean-detail-tiles/tiles/{z}/{x}/{y}.png
```

Use that URL as a custom map source in Gaia.

## What This Contains

- Transparent PNG web tiles in `tiles/{z}/{x}/{y}.png`
- Zooms 10 through 17
- Zooms 10 through 12 use simplified, labeled, high-DPI block overview tiles
  with labels anchored to detected block geometry and kept inside tile
  boundaries to avoid clipped names
- Zooms 13 through 17 use the detailed field-map tiles with roads, contours,
  streams, block outlines/fills, boundary lines, and useful operational map labels
- Removed slope insets, legends, yellow warning boxes, page borders, compass, contractor text, Consult Plant Wizard boxes, and block seed panels

## Bounds

- West: `-115.75681`
- South: `48.98985`
- East: `-114.40327`
- North: `50.55049`

## GitHub Pages Setup

1. Create a new GitHub repository.
2. Put the contents of this folder at the root of that repository.
3. Push it to GitHub.
4. In the repository, go to `Settings > Pages`.
5. Set the source to deploy from the main branch, root folder.
6. Use the published Pages URL with `/tiles/{z}/{x}/{y}.png` at the end.

Do not use Git LFS for these tiles. GitHub Pages needs to serve the actual PNG files.
