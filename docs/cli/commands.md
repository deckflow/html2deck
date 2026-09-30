# Commands

> Reference HTML2Deck CLI commands, flags, examples, and expected behavior for conversion, authentication, persistent config, output formats, and execution modes.

The primary command follows the pattern `html2deck <input> [flags]`. Config commands follow `html2deck config set <key> <value>`.

Run `html2deck --help` for usage details and `html2deck --version` for version information.

## Convert

Convert HTML input into a presentation artifact.

| Command | Description |
| --- | --- |
| `html2deck <input> [flags]` | Convert stdin, one HTML file, multiple ordered HTML files, or a URL. |

### Supported input forms

| Input | Example | Description |
| --- | --- | --- |
| stdin | `cat index.html \| html2deck - -o deck.pptx` | Read HTML from stdin. |
| HTML file | `html2deck index.html` | Convert one local HTML file. |
| HTML files | `html2deck page1.html page2.html page3.html` | Convert files in argument order. |
| URL | `html2deck https://example.com/deck.html -o deck.pptx` | Load and convert a hosted page. |

### Flags for `html2deck <input>`

| Flag | Description | Default |
| --- | --- | --- |
| `-h, --help` | Show help. | Off |
| `--version` | Show version information. | Off |
| `-o, --output <path>` | Output path. Example: `deck.pptx`. | Same base name and path as the input when possible. |
| `-v, --verbose` | Write detailed logs to stderr. | `false` |
| `--quiet` | Only output errors and the final result. Conflicts with `--verbose`. | `false` |
| `--json` | Make stdout machine-readable JSON only. | `false` |
| `--report` | Generate a conversion report next to the output. | Off |
| `--mode <mode>` | Choose `auto`, `free`, `license`, or `cloud` execution. | `auto` |
| `--render-wait <seconds>` | Wait time per page before capture. | `3` |
| `--embed-fonts` | Embed fonts. Cloud opt-in; license local defaults on (remote index cached under `~/.deckflow/html2deck/fonts`). | Cloud: Off; License: On |
| `--fonts-index-url <url>` | Remote `fonts-index.json` for Licensed embed. | `FONTS_INDEX_URL` or default CDN |
| `--format <format>` | Choose `pptx` or `png`. | `pptx` |
| `--webhook <url>` | Callback URL for cloud conversion events. | Config |
| `--retention-hours <n>` | Cloud file retention time in hours. | Config |

## Execution Modes

`--mode` controls Free / License / Cloud product behavior.

| Mode | Description |
| --- | --- |
| `auto` | Default. Valid local `.lic` → `license`; otherwise `free`. Does not auto-switch to cloud for API keys. |
| `free` | Force Free local. Soft tips suggest `--mode cloud` for advanced features. |
| `license` | Require a valid commercial `.lic` and run the full local engine. |
| `cloud` | Run in cloud mode. Guest requests are rate-limited; sign in when prompted or for authenticated access. |

Premium flags (`--embed-fonts`, `--rebuild-svg`, `--rebuild-chart`) require `cloud` or `license`. Free rejects them with guidance.

## Authentication

Local conversion works without an API key. Cloud mode can start as a rate-limited guest; sign in or configure an API key when prompted or when you need authenticated access.

| Command | Description |
| --- | --- |
| `html2deck auth login` | Start an interactive browser login flow. |
| `html2deck auth status` | Verify the currently configured credentials. |

You can also store an API key directly:

```bash
html2deck config set api-key <key>
```

Stored credentials are written locally at `~/.deckflow/credentials`.

For CI, Docker, or agent environments, set `HTML2DECK_API_KEY`. The environment variable takes precedence over stored credentials.

```bash
export HTML2DECK_API_KEY=your-api-key
```

## Config

Persistent settings are managed with `html2deck config set`.

| Command | Description | Default |
| --- | --- | --- |
| `html2deck config set api-key <key>` | Store the API key used for cloud requests. | None |
| `html2deck config set size <size>` | Set PPTX dimensions. | `1920x1080` |
| `html2deck config set webhook <url>` | Set the default callback URL. | None |
| `html2deck config set retention-hours <n>` | Set cloud file retention time in hours. | `3` |

## Output Formats

| Format | Example | Description |
| --- | --- | --- |
| `pptx` | `html2deck index.html --format pptx -o deck.pptx` | PowerPoint deck output. Default. |
| `png` | `html2deck index.html --format png -o frames` | PNG frame output. |

## Install

Run HTML2Deck without installing:

```bash
npx -y @deckflow/html2deck@latest index.html -o deck.pptx
```

Install globally:

```bash
npm install -g @deckflow/html2deck
```

## Examples

```bash
html2deck index.html
html2deck index.html -o deck.pptx
cat index.html | html2deck - -o deck.pptx
html2deck page1.html page2.html page3.html -o deck.pptx
html2deck https://example.com/deck.html -o deck.pptx --render-wait 9
html2deck index.html --mode cloud --embed-fonts -o deck.pptx
html2deck index.html -o deck.pptx --json
html2deck index.html -o deck.pptx --report
```
