# HTML2Deck CLI reference

Package: `@deckflow/html2deck` · Binary: `html2deck`

## Invoke

```bash
npx -y @deckflow/html2deck@latest [inputs...] [flags]
# or globally: html2deck ...
# or from this repo after build: node dist/cli.js ...
```

## Inputs

| Form | Example |
| --- | --- |
| File | `html2deck page.html -o out.pptx` |
| Multiple files (order = slides) | `html2deck a.html b.html c.html -o out.pptx` |
| Stdin | `cat page.html \| html2deck - -o out.pptx` |
| URL | `html2deck https://example.com/deck.html -o out.pptx` |

Stdin **requires** `-o` / `--output`.

## Flags

| Flag | Default | Notes |
| --- | --- | --- |
| `-o, --output <path>` | same basename as input | Required for stdin |
| `--format <pptx\|png>` | `pptx` | PNG needs cloud |
| `--mode <auto\|free\|license\|cloud>` | `auto` | Agents: prefer `free` |
| `--width <pixels>` | auto → else `1280` | Height = width × 720/1280 when set. When omitted, auto-detect from `meta[name=deck-size]`, CSS `--deck-width`/`--deck-height`, or SVG viewBox |
| `--platform <win\|mac\|ios\|android\|linux>` | current OS | Generic font mapping |
| `--render-wait <seconds>` | `3` | Cloud per-page wait |
| `--rebuild-svg` | off | Cloud only |
| `--rebuild-chart` | off | Cloud only |
| `--embed-fonts` | off | Cloud only |
| `--map-motion` | off | Cloud only |
| `--report` | off | Writes `<output>.report.json` |
| `--json` | off | Machine-readable stdout |
| `--quiet` | off | Conflicts with `--verbose` |
| `-v, --verbose` | off | Logs on stderr |
| `--no-animations` | off | Disable element entrance-animation export |
| `--slide-transition <name>` | `random` | Slide-to-slide transition(s). Comma-separated names cycle (`fade,push,wipe`). See [slide-transitions.md](slide-transitions.md) |
| `--no-slide-transitions` | off | Disable slide-to-slide transitions |
| `--webhook <url>` | config | Cloud |
| `--retention-hours <n>` | config (3) | Cloud, 0–99 |

## Modes

| Mode | Behavior |
| --- | --- |
| `free` | Playwright + pptxgenjs Free local; PPTX only; no API key |
| `license` | Full local engine; requires valid `.lic` |
| `cloud` | Upload to HTML2Deck Cloud; guest or API key / space |
| `auto` | Valid `.lic` → `license`, else `free` |

Premium flags with `--mode free` → usage error (guidance to cloud/license).

## Auth (cloud only)

```bash
export HTML2DECK_API_KEY=your-key          # preferred for agents/CI
html2deck config set api-key <key>
html2deck auth login
html2deck auth status
```

Credentials file: `~/.deckflow/credentials`. Env var overrides stored key.

## Config

```bash
html2deck config set api-key <key>
html2deck config set size 1920x1080
html2deck config set webhook https://example.com/hook
html2deck config set retention-hours 3
```

## Local vs cloud capabilities

| Capability | Local | Cloud |
| --- | --- | --- |
| PPTX | ✓ | ✓ |
| PNG frames | ✗ | ✓ |
| Rebuild SVG/chart | ✗ | ✓ |
| Embed fonts / map motion | ✗ | ✓ |
| Multi-file merge | ✓ | ✓ |
| Auto multi-slide detect | ✓ | (server-side) |

## Peer dependencies (local)

Local conversion needs `playwright-core` and `pptxgenjs` available (declared as peerDependencies of `@deckflow/html2deck`).
