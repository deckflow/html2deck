# Features

> Configure the HTML2Deck CLI with shared flags for output format, execution mode, render timing, reports, logging, and cloud conversion context.

HTML2Deck converts HTML from files, stdin, or hosted URLs into PPTX or PNG artifacts. Local conversion works without authentication; cloud execution and cloud-only enhancement flags require an API key.

Run HTML2Deck without installing:

```bash
npx -y @deckflow/html2deck@latest index.html -o deck.pptx
```

Install globally when you want the `html2deck` command available on your PATH:

```bash
npm install -g @deckflow/html2deck
```

## Common Flags

These flags are supported by the primary conversion command.

| Flag | Description | Default |
| --- | --- | --- |
| `-o, --output <path>` | Output path. | Same name and path as input when possible. |
| `--format <format>` | Output format: `pptx` or `png`. | `pptx` |
| `--mode <mode>` | Execution mode: `auto`, `free`, `license`, or `cloud`. | `auto` |
| `--render-wait <seconds>` | Wait before capturing each page. | `3` |
| `--report` | Generate a conversion report next to the output. | Off |
| `--json` | Print machine-readable JSON on stdout only. | `false` |
| `--quiet` | Only output errors and final result. | `false` |
| `-v, --verbose` | Write detailed logs to stderr. | `false` |
| `--webhook <url>` | Callback URL for cloud conversion events. | Config |
| `--retention-hours <n>` | Cloud file retention time in hours. | Config |

`--quiet` and `--verbose` conflict.

***

## Input Sources

HTML2Deck accepts stdin, one local HTML file, multiple ordered local HTML files, or a URL through the same command:

```bash
html2deck <input> [flags]
```

```bash
cat index.html | html2deck - -o deck.pptx
html2deck index.html
html2deck page1.html page2.html page3.html -o deck.pptx
html2deck https://example.com/deck.html -o deck.pptx
```

For stdin, pass `--output` because there is no source filename to derive the output path from. For multiple input files, HTML2Deck preserves the order passed on the command line.

***

## Execution Modes

HTML2Deck `--mode` values:

| Mode | Behavior |
| --- | --- |
| `auto` | Default. Valid local `.lic` → `license`; otherwise `free`. Does **not** auto-switch to cloud when an API key exists. |
| `free` | Force Free local (degraded advanced features). Soft tips suggest `--mode cloud`. |
| `license` | Require a valid commercial `.lic` and run the full local engine. |
| `cloud` | Force cloud conversion. Guest requests are rate-limited. |

```bash
html2deck index.html --mode auto
html2deck index.html --mode free
html2deck index.html --mode license
html2deck index.html --mode cloud
html2deck index.html --mode license --license ~/.deckflow/html2deck.lic
```

Set cloud credentials with `html2deck config set api-key <key>` or the `HTML2DECK_API_KEY` environment variable.

***

## Premium Features (Cloud or Licensed)

In **Free local** mode, these flags throw a guidance error pointing to Cloud or License.
In **Licensed local** or **Cloud**, they unlock the corresponding enhancements:

| Flag | Description | Default |
| --- | --- | --- |
| `--embed-fonts` | Embed fonts (Cloud param `needEmbedFonts`; Licensed local defaults on; also `-f/--fonts`). | Cloud: Off; License: On |
| `--rebuild-svg` | Rebuild SVG as editable shapes (Cloud `needRebuildSvg`; Licensed local decomposes by default). | Off |
| `--rebuild-chart` | Rebuild JS charts as native PPT charts (Cloud `needRebuildChart`; Licensed enables native charts). | Off |
| `-f, --fonts` | Licensed local full font embed (remote index + EOT cache under `~/.deckflow/html2deck/fonts`; optional local `FONTS_PATH` / `FONTS_INDEX_PATH` override). | Off |
| `--fonts-index-url <url>` | Remote `fonts-index.json` URL for Licensed embed. | `FONTS_INDEX_URL` or `https://slave-01.slide.im/_files/fonts/fonts-index.json` |
| `--license <path>` | Path to signed commercial `.lic`. | Env / `~/.deckflow/html2deck.lic` |

```bash
html2deck index.html -o deck.pptx --mode cloud --embed-fonts --rebuild-svg --rebuild-chart
html2deck index.html -o deck.pptx --mode license -f
```

***

## Render Timing

`--render-wait <seconds>` controls how long HTML2Deck waits before capturing each page.

```bash
html2deck index.html --render-wait 9 -o deck.pptx
```

Use it when the page needs time to load fonts, charts, client-side data, or animation state.

***

## Reports

`--report` writes a conversion report next to the output path:

```bash
html2deck index.html -o deck.pptx --report
```

Reports are intended for conversion review and automation diagnostics. Use `--json` when scripts need the final conversion result on stdout without progress text.

***

## Stdin Support

Use `-` to read HTML from stdin:

```bash
cat index.html | html2deck - -o deck.pptx
```

This is useful in generators, CI pipelines, and agent workflows where HTML is produced by another process.

***

## Error Handling

Errors are written to stderr. With `--json`, stdout remains machine-readable and should not contain progress text or diagnostics. Use `--quiet` to reduce nonessential output, or `--verbose` to write detailed logs to stderr when troubleshooting. `--quiet` and `--verbose` cannot be used together.

```json
{
  "error": {
    "code": "usage_error",
    "message": "--quiet conflicts with --verbose"
  }
}
```

***

## Configuration

Persistent settings are managed with `html2deck config set`:

```bash
html2deck config set api-key <key>
html2deck config set size 1920x1080
html2deck config set webhook https://example.com/webhooks/html2deck
html2deck config set retention-hours 3
```

| Key | Description | Default |
| --- | --- | --- |
| `api-key` | API key for cloud requests. | None |
| `size` | PPTX dimensions. | `1920x1080` |
| `webhook` | Default callback URL. | None |
| `retention-hours` | Cloud file retention time in hours. | `3` |

For cloud credentials, `HTML2DECK_API_KEY` can be set in the environment instead of storing a key locally.
