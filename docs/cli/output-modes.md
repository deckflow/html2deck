# Output Modes

> Pick the right HTML2Deck CLI output mode for your workflow. Defaults to a human-readable final result; supports machine-readable JSON for scripts, CI, and agent workflows.

HTML2Deck keeps command results and diagnostics separate. Final results go to stdout. Progress, warnings, verbose logs, and errors go to stderr.

Supported inputs are stdin (`-`), one HTML file, multiple ordered HTML files, or a URL. Authentication is optional for local mode and required only for cloud execution or cloud-only enhancement flags.

## Default: Human-Readable Result

By default, HTML2Deck prints the final result on stdout.

```bash
html2deck index.html -o deck.pptx
```

```text
deck.pptx
```

The default success output is the generated artifact path. Progress, if shown, is written to stderr so stdout remains easy to capture.

```bash
html2deck index.html
```

```text
index.pptx
```

Choose another format with `--format`:

```bash
html2deck report.html --format png -o frames
```

```text
frames
```

For PNG output, use a file, stdin, URL, or multiple ordered HTML files:

```bash
html2deck slide-1.html slide-2.html --format png -o frames
```

```text
frames
```

URL input behaves the same way:

```bash
html2deck https://example.com/deck.html -o deck.pptx
```

```text
deck.pptx
```

## `--json`: Machine-Readable Output

Add `--json` when another program needs to parse the result.

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

With `--json`, stdout contains only JSON. Logs and diagnostics stay on stderr.

The exact JSON fields can evolve; treat the examples here as representative of the CLI result envelope, not as a formal schema.

Example stdout:

```json
{
  "ok": true,
  "input": ["index.html"],
  "output": "deck.pptx",
  "format": "pptx",
  "mode": "free"
}
```

Example stderr when progress is enabled:

```text
Rendering index.html
Writing deck.pptx
```

For stdin, the input is reported as `-`:

```bash
cat index.html | html2deck - -o deck.pptx --json
```

```json
{
  "ok": true,
  "input": ["-"],
  "output": "deck.pptx",
  "format": "pptx",
  "mode": "free"
}
```

## Quiet and Verbose Modes

Use `--quiet` to suppress progress and non-essential messages:

```bash
html2deck index.html -o deck.pptx --quiet
```

Example stdout:

```text
deck.pptx
```

Example stderr: no output.


Use `-v` or `--verbose` to write detailed logs to stderr:

```bash
html2deck index.html -o deck.pptx --verbose
```

Example stdout:

```text
deck.pptx
```

Example stderr:

```text
Mode: local
Input: index.html
Format: pptx
Output: deck.pptx
Rendering index.html
Writing deck.pptx
Done
```

`--quiet` and `--verbose` conflict:

```bash
html2deck index.html --quiet --verbose
```

Representative stderr:

```text
Error: --quiet conflicts with --verbose
```

Representative JSON stderr when `--json` is also present:

```json
{
  "ok": false,
  "error": {
    "code": "usage_error",
    "message": "--quiet conflicts with --verbose"
  }
}
```

## Errors

Errors are written to stderr.

```json
{
  "error": {
    "code": "usage_error",
    "message": "--quiet conflicts with --verbose"
  }
}
```

Common error categories:

| Code | Meaning |
| --- | --- |
| `usage_error` | Invalid flags, missing input, bad mode, or conflicting flags. |
| `auth_error` | Cloud rejected the request with 401 (guest rate limit reached, guest access disabled, or an invalid/expired credential). Run `html2deck auth login` or `html2deck config set api-key <key>`. |
| `render_error` | The page could not be loaded or rendered. |
| `conversion_error` | Conversion failed after rendering. |

The error examples are representative. Scripts should rely on exit codes first, then parse JSON errors when `--json` is used.

## Exit Codes

| Code | Meaning |
| --- | --- |
| `0` | Success. |
| `1` | General conversion or runtime error. |
| `2` | Usage error. |
| `3` | Authentication error. |

## Configuring Output Defaults

HTML2Deck defines `--json`, `--quiet`, and `--verbose` as invocation flags. Persistent config covers API key, PPTX size, webhook, and retention hours:

```bash
html2deck config set api-key <key>
html2deck config set size 1920x1080
html2deck config set webhook https://example.com/webhooks/html2deck
html2deck config set retention-hours 3
```

For CI, Docker, and agent environments, `HTML2DECK_API_KEY` takes precedence over stored credentials:

```bash
export HTML2DECK_API_KEY=your-api-key
```

## Stdout vs Stderr

HTML2Deck separates data from diagnostics:

| Stream | Content |
| --- | --- |
| stdout | Final output path or JSON result. |
| stderr | Progress, warnings, verbose logs, and errors. |

This keeps pipelines clean:

```bash
OUTPUT=$(html2deck index.html -o deck.pptx --json | jq -r '.output')
```
