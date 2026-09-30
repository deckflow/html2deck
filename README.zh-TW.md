# HTML2Deck

<p align="center">
  <img src="https://raw.githubusercontent.com/deckflow/html2deck/main/assets/preview.png" alt="HTML2Deck — HTML to PPTX" width="800" />
</p>

**語言：** [English](./README.md) · [简体中文](./README.zh-CN.md) · **繁體中文** · [Français](./README.fr.md) · [Deutsch](./README.de.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [한국어](./README.ko.md)

在終端機中將 HTML 檔案、標準輸入或 URL 轉換為 PPTX 或 PNG 簡報。

<p align="center">
  <img src="https://raw.githubusercontent.com/deckflow/html2deck/main/assets/screenshots/demo.gif" alt="HTML → PPTX 示範" width="600" />
</p>

## 快速開始

無需安裝，直接執行：

```bash
npx -y @deckflow/html2deck@latest index.html -o deck.pptx
```

## 安裝

```bash
npm install -g @deckflow/html2deck
html2deck --version
```

## 轉換 HTML

轉換單一 HTML 檔案（預設輸出至同路徑、同名檔案）：

```bash
html2deck index.html
# → index.pptx
```

指定輸出路徑：

```bash
html2deck index.html -o deck.pptx
```

從標準輸入讀取 HTML：

```bash
cat index.html | html2deck - -o deck.pptx
```

依序轉換多個 HTML 檔案：

```bash
html2deck page1.html page2.html page3.html -o deck.pptx
```

轉換託管頁面：

```bash
html2deck https://example.com/deck.html -o deck.pptx
```

## 輸出格式

| 格式 | 說明 |
| --- | --- |
| `pptx` | PowerPoint 簡報（預設） |
| `png` | PNG 影格輸出 |

```bash
html2deck index.html --format png -o frames
```

## 執行模式

| 模式 | 說明 |
| --- | --- |
| `auto` | 已設定 API Key 時使用雲端，否則本地執行 |
| `local` | 始終本地執行（無需 API Key） |
| `cloud` | 始終雲端執行（需要 API Key） |

```bash
html2deck index.html --mode local
html2deck index.html --mode cloud -o deck.pptx
```

### 僅雲端可用功能

以下選項需要雲端模式：

| 選項 | 說明 |
| --- | --- |
| `--rebuild-svg` | 重建 SVG 物件 |
| `--rebuild-chart` | 重建圖表物件 |
| `--embed-fonts` | 嵌入字型 |
| `--map-motion` | 對應動畫 |

```bash
html2deck index.html \
  -o deck.pptx \
  --mode cloud \
  --rebuild-svg \
  --rebuild-chart \
  --embed-fonts \
  --map-motion
```

## 驗證與設定

僅雲端執行和雲端專屬功能需要驗證。本地轉換無需 API Key。

```bash
html2deck auth login
html2deck auth status
html2deck config set api-key <key>
```

在 CI、Docker 或 Agent 環境中，可使用環境變數（優先於本地儲存的憑證）：

```bash
export HTML2DECK_API_KEY=your-api-key
```

持久化設定：

| 命令 | 說明 | 預設值 |
| --- | --- | --- |
| `html2deck config set api-key <key>` | 雲端請求使用的 API Key | — |
| `html2deck config set size <size>` | PPTX 尺寸 | `1920x1080` |
| `html2deck config set webhook <url>` | 預設回呼 URL | — |
| `html2deck config set retention-hours <n>` | 雲端檔案保留時間（小時） | `3` |

憑證儲存在本地 `~/.deckflow/credentials`。

## CLI 參考

### 轉換選項

| 選項 | 說明 | 預設值 |
| --- | --- | --- |
| `-h, --help` | 顯示說明 | — |
| `--version` | 顯示版本 | — |
| `-o, --output <path>` | 輸出路徑 | 與輸入同名同路徑 |
| `-v, --verbose` | 詳細日誌輸出至 stderr | `false` |
| `--quiet` | 僅輸出錯誤和最終結果 | `false` |
| `--json` | stdout 輸出機器可讀 JSON | `false` |
| `--report` | 產生轉換報告 | 關閉 |
| `--mode <mode>` | `auto`、`local` 或 `cloud` | `auto` |
| `--render-wait <seconds>` | 每頁擷取前等待秒數 | `3` |
| `--format <format>` | `pptx` 或 `png` | `pptx` |
| `--webhook <url>` | 雲端回呼 URL | 設定項 |
| `--retention-hours <n>` | 雲端檔案保留時間（小時） | 設定項 |

`--quiet` 與 `--verbose` 不能同時使用。

### JSON 輸出

```bash
html2deck index.html -o deck.pptx --json
```

```json
{
  "ok": true,
  "input": ["index.html"],
  "output": "deck.pptx",
  "format": "pptx",
  "mode": "local"
}
```

## 程式 API

在 Node.js 中作為函式庫使用：

```javascript
import { convertHtmlToPptx } from '@deckflow/html2deck';

const result = await convertHtmlToPptx({
  input: 'index.html',
  output: 'deck.pptx',
});
```

## 文件

詳細 CLI 文件見 [`docs/cli/`](./docs/cli/) 目錄。

## 授權

專有軟體。詳見 [LICENSE](./LICENSE)。
