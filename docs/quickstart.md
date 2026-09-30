# Quick Start

HTML2Deck converts HTML from local files, standard input, or URLs into PowerPoint (`.pptx`) or PNG output.

## Requirements

- Node.js 18 or newer
- A Chromium-based browser for local conversion. HTML2Deck uses Playwright's browser bundle when available; set `--executable-path` or `HTML2DECK_CHROMIUM_EXECUTABLE_PATH` to use a specific Chromium binary.

## Convert your first deck

Run directly with `npx`:

```bash
npx -y @deckflow/html2deck@latest index.html -o deck.pptx
```

Or install the CLI globally:

```bash
npm install -g @deckflow/html2deck
html2deck --version
html2deck index.html -o deck.pptx
```

Open `deck.pptx` in PowerPoint to review the result.

## Common input forms

```bash
# Read HTML from stdin
generate-html | html2deck - -o deck.pptx

# Combine HTML files into a deck in argument order
html2deck cover.html agenda.html conclusion.html -o deck.pptx

# Convert a hosted page
html2deck https://example.com/deck.html -o deck.pptx

# Render PNG frames instead of a PPTX
html2deck index.html --format png -o frames
```

## Local and cloud execution

Local conversion is the default when no valid `.lic` is found (`auto` → `free`) and does not require authentication:

```bash
html2deck index.html --mode free -o deck.pptx
```

Cloud mode supports rate-limited guest conversion. Sign in to use authenticated cloud access; premium flags require `cloud` or `license`:

```bash
html2deck auth login
html2deck index.html --mode cloud --embed-fonts -o deck.pptx
```

For CI and agent environments, provide `HTML2DECK_API_KEY` instead of storing credentials locally:

```bash
export HTML2DECK_API_KEY=your-api-key
html2deck index.html --mode cloud --embed-fonts -o deck.pptx
```

## Next steps

- Read [concepts.md](./concepts.md) to understand how HTML becomes a deck.
- Browse the [CLI documentation](./cli/index.md) for all commands and flags.
