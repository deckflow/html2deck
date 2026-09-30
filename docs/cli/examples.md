# Examples

> Browse HTML2Deck CLI workflows for local HTML files, stdin, ordered HTML files, hosted URLs, reports, script-friendly output, and cloud reconstruction.

## Run Without Installing

Convert a local HTML file with the npm package:

```bash
npx -y @deckflow/html2deck@latest index.html -o deck.pptx
```

Install the CLI globally when you use it often:

```bash
npm install -g @deckflow/html2deck
html2deck --version
```

***

## Convert a Single HTML File

Convert a local HTML file to PPTX:

```bash
html2deck index.html
```

The default output uses the input file name:

```text
index.pptx
```

Write to a specific output path:

```bash
html2deck index.html -o deck.pptx
```

***

## Convert HTML from Stdin

Pipe generated HTML into HTML2Deck:

```bash
cat index.html | html2deck - -o deck.pptx
```

Use `-` as the stdin input placeholder. Provide `--output` or `-o` when converting stdin.

***

## Convert Multiple Ordered Files

Pass files in the order they should appear:

```bash
html2deck title.html agenda.html content.html summary.html -o deck.pptx
```

***

## Convert a URL

Convert a hosted HTML deck:

```bash
html2deck https://example.com/deck.html -o deck.pptx
```

Wait longer for client-side rendering:

```bash
html2deck https://example.com/deck.html -o deck.pptx --render-wait 9
```

***

## Generate PNG Output

Generate PNG frames from one HTML file:

```bash
html2deck index.html --format png -o frames
```

Generate PNG frames from multiple explicit HTML files:

```bash
html2deck title.html chart.html appendix.html --format png -o frames
```

Generate the default PPTX format explicitly:

```bash
html2deck index.html --format pptx -o deck.pptx
```

***

## Use Free, License, Auto, or Cloud Mode

Force Free local conversion:

```bash
html2deck index.html -o deck.pptx --mode free
```

Let HTML2Deck choose from a local license (or Free):

```bash
html2deck index.html -o deck.pptx --mode auto
```

Force license mode (requires a valid `.lic`):

```bash
html2deck index.html -o deck.pptx --mode license
```

Force cloud conversion:

```bash
html2deck index.html -o deck.pptx --mode cloud
```

`auto` uses `license` when a valid local `.lic` exists and `free` otherwise. It does not switch to cloud based on an API key.

***

## Use Cloud Reconstruction Features

Rebuild richer presentation objects in cloud mode:

```bash
html2deck index.html \
  -o deck.pptx \
  --mode cloud \
  --embed-fonts \
```

These flags require `--mode cloud` or `--mode license`.

***

## Generate a Conversion Report

Create a report next to the output:

```bash
html2deck index.html -o deck.pptx --report
```

***

## Get Machine-Readable Output

Return JSON for scripts and CI:

```bash
html2deck index.html -o deck.pptx --json
```

```json
{
  "ok": true,
  "input": ["index.html"],
  "output": "deck.pptx",
  "format": "pptx",
  "mode": "free"
}
```

Combine JSON with stdin:

```bash
cat index.html | html2deck - -o deck.pptx --json
```

***

## Adjust Log Output

Print only the final result:

```bash
html2deck index.html -o deck.pptx --quiet
```

Print detailed conversion logs:

```bash
html2deck index.html -o deck.pptx --verbose
```

***

## Configure Cloud Access

Start browser login:

```bash
html2deck auth login
```

Set an API key directly:

```bash
html2deck config set api-key <key>
```

For CI, Docker, and agent environments, set the environment variable:

```bash
export HTML2DECK_API_KEY=your-api-key
html2deck index.html -o deck.pptx --mode cloud
```

Verify your credentials:

```bash
html2deck auth status
```

***

## Configure PPTX Size

```bash
html2deck config set size 1920x1080
```

The default PPTX size is `1920x1080`.
