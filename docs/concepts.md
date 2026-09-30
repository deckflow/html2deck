# Core Concepts

## Inputs and slides

HTML2Deck accepts a local HTML file, HTML from stdin, one or more HTML files, or a URL. Multiple input files are converted in the order supplied. A single HTML document can also produce multiple slides when HTML2Deck detects slide containers, or when you provide a slide selector through the programmatic API.

## Rendering before conversion

HTML2Deck loads HTML in Chromium and inspects the rendered DOM rather than parsing source markup alone. This means computed layout, CSS, web fonts, and browser-rendered content inform the output. Give pages that load data or fonts asynchronously enough time to settle before conversion.

## Editable PPTX content

The local engine translates inspected content into PowerPoint objects where possible: text, shapes, tables, images, SVGs, canvas content, and math each have dedicated handling. Fidelity depends on the source HTML and on what PowerPoint can represent. Content that cannot be reconstructed exactly may be simplified or rasterized; review the generated deck, especially after using complex CSS or embedded content.

## Local versus cloud mode

`--mode auto` (default) uses `license` when a valid local `.lic` is found and `free` otherwise — it does **not** switch to cloud based on an API key. `--mode free` forces Free local. `--mode license` requires a valid commercial license and unlocks the full local engine. `--mode cloud` runs remotely (rate-limited guest until you sign in) and is intended for cloud enhancements such as font embedding.

## Output formats

- `pptx` is the default editable PowerPoint output.
- `png` writes rendered image frames.

Use `--report` to write a conversion report next to the output, and `--json` when scripts need a machine-readable result on stdout.

## Fonts

The output can only render fonts installed on the machine that opens the presentation unless they are embedded. Licensed local mode embeds matched fonts from a remote font library (cached under `~/.deckflow/html2deck/fonts`). Use cloud mode with `--embed-fonts` when cloud-side embedding is required. Font availability and license terms remain your responsibility.

## Animations and transitions

HTML2Deck can inspect supported CSS and Anime.js entrance animations and write compatible PowerPoint animations. Slide transitions can be selected, cycled, randomized, or disabled. These features translate supported effects; they are not a guarantee that every browser animation has an equivalent in PowerPoint.

## Automation

The CLI is designed for generators, CI, and agent workflows. Feed generated HTML through stdin, use `--json` to parse results, and keep diagnostics on stderr separate from data on stdout.
