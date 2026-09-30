# Prompt: Review a HTML2Deck conversion

Use after an HTML→PPTX run to judge quality and decide fixes.

## Checklist

1. Open or inspect `<output>.report.json` if `--report` was used:
   - `slideCount` matches expected slides
   - `elements.byType` has text/shapes (not only images) when editable content was expected
   - `fonts.families` includes intended faces
2. Open the PPTX in PowerPoint/Keynote or unpack and spot-check:
   - Slide order
   - Truncated text / overflow
   - Missing images
   - Wrong background or blank slides
3. Map issues to HTML fixes (preferred) using the troubleshooting table in the `html2deck` skill:
   - Wrong slide count → slide host selectors / size gate / active class
   - Blank slide → content outside host or hidden state
   - Fonts → `@font-face` / `--platform` / cloud `--embed-fonts`
   - Charts → cloud `--rebuild-chart` if editable charts are required
4. Re-run conversion after HTML changes; compare new report `slideCount` and element totals.

## Pass criteria

- Expected number of slides
- No blank slides
- Body text selectable/editable in PPTX (unless intentionally rasterized)
- Output path returned by `--json` exists and opens
