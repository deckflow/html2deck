# Examples

## From the starter template

```bash
# From repo root — 4-slide deck using .slide-container @ 1280×720
npx -y @deckflow/html2deck@latest \
  skill/html2deck/templates/basic-deck.html \
  -o basic-deck.pptx --mode free --json --report
```

Copy and customize before converting:

```bash
cp skill/html2deck/templates/basic-deck.html ./deck.html
# edit titles, cards, KPIs…
npx -y @deckflow/html2deck@latest deck.html -o deck.pptx --mode free --json
```

## Local PPTX from one file

```bash
npx -y @deckflow/html2deck@latest deck.html -o deck.pptx --mode free --json
```

## Multi-file deck

```bash
npx -y @deckflow/html2deck@latest \
  slides/01-title.html \
  slides/02-content.html \
  slides/03-end.html \
  -o deck.pptx --mode free --json
```

## Stdin from a generator

```bash
python gen_deck.py | npx -y @deckflow/html2deck@latest - -o deck.pptx --mode free --json
```

## Match a 1920-wide design

```bash
npx -y @deckflow/html2deck@latest deck.html -o deck.pptx --mode free --width 1920 --json
```

## Target Windows font mapping

```bash
npx -y @deckflow/html2deck@latest deck.html -o deck.pptx --mode free --platform win --json
```

## With conversion report

```bash
npx -y @deckflow/html2deck@latest deck.html -o deck.pptx --mode free --report --json
# → deck.pptx and deck.pptx.report.json
```

## URL input (needs network)

```bash
npx -y @deckflow/html2deck@latest https://example.com/deck.html -o deck.pptx --mode free --json
```

## Cloud reconstruction

```bash
export HTML2DECK_API_KEY=your-key
npx -y @deckflow/html2deck@latest deck.html -o deck.pptx --mode cloud \
  --rebuild-svg --rebuild-chart --embed-fonts --map-motion --json
```

## Cloud PNG frames

```bash
export HTML2DECK_API_KEY=your-key
npx -y @deckflow/html2deck@latest deck.html --format png -o frames --mode cloud --json
```

## From this repository (dev)

```bash
pnpm build
node dist/cli.js path/to/deck.html -o out.pptx --mode free --json
```

## Node API

```js
import { convertHtmlToPptx } from '@deckflow/html2deck';
import { writeFileSync } from 'fs';

const { data, slideCount } = await convertHtmlToPptx({
  inputs: ['a.html', 'b.html'],
  viewportWidth: 1280,
  viewportHeight: 720,
  allowLocalResources: true,
  quiet: true,
});
writeFileSync('deck.pptx', data);
console.log('slides', slideCount);
```
