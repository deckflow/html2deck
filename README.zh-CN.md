# HTML2Deck

<p align="center">
  <img src="https://raw.githubusercontent.com/deckflow/html2deck/main/assets/preview.png" alt="HTML2Deck — HTML to PPTX" width="800" />
</p>

**语言：** [English](./README.md) · **简体中文** · [繁體中文](./README.zh-TW.md) · [Français](./README.fr.md) · [Deutsch](./README.de.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [한국어](./README.ko.md)

在终端中将 HTML 文件、标准输入或 URL 转换为 PPTX 或 PNG 演示文稿。

## 运行模式

| 模式 | 判定 | 高级功能（字体嵌入 / 原生图表 / SVG 重建） |
| --- | --- | --- |
| **auto**（默认） | 本地有效 `.lic` → 等同 `license`；否则 `free` | 跟随解析结果 |
| **free** | `--mode free` | 本地降级（仅 FA 字体、SVG 当图、图表栅格）。仅此模式会提示改用 `--mode cloud`。 |
| **license** | `--mode license`（需有效 `.lic`） | 本地全能力（原 html2pptx 引擎）。 |
| **cloud** | `--mode cloud` | `--embed-fonts` / `--rebuild-svg` / `--rebuild-chart` 走云端。 |

```bash
html2deck deck.html -o deck.pptx --mode free
html2deck deck.html -o deck.pptx --mode cloud --embed-fonts --rebuild-svg --rebuild-chart
html2deck deck.html -o deck.pptx --mode license -f
```

## 快速开始

无需安装，直接运行：

```bash
npx -y @deckflow/html2deck@latest index.html -o deck.pptx
```

## 安装

```bash
npm install -g @deckflow/html2deck
html2deck --version
```

## 转换 HTML

转换单个 HTML 文件（默认输出到同路径、同名文件）：

```bash
html2deck index.html
# → index.pptx
```

指定输出路径：

```bash
html2deck index.html -o deck.pptx
```

从标准输入读取 HTML：

```bash
cat index.html | html2deck - -o deck.pptx
```

按顺序转换多个 HTML 文件：

```bash
html2deck page1.html page2.html page3.html -o deck.pptx
```

转换托管页面：

```bash
html2deck https://example.com/deck.html -o deck.pptx
```

## 输出格式

| 格式 | 说明 |
| --- | --- |
| `pptx` | PowerPoint 演示文稿（默认） |
| `png` | PNG 帧输出 |

```bash
html2deck index.html --format png -o frames
```

## 执行模式

| 模式 | 说明 |
| --- | --- |
| `auto` | 已配置 API Key 时使用云端，否则本地执行 |
| `local` | 始终本地执行（无需 API Key） |
| `cloud` | 始终云端执行（需要 API Key） |

```bash
html2deck index.html --mode free
html2deck index.html --mode cloud -o deck.pptx
```

### 仅云端可用功能

以下选项需要云端模式：

| 选项 | 说明 |
| --- | --- |
| `--rebuild-svg` | 重建 SVG 对象 |
| `--rebuild-chart` | 重建图表对象 |
| `--embed-fonts` | 嵌入字体 |
| `--map-motion` | 映射动画 |

```bash
html2deck index.html \
  -o deck.pptx \
  --mode cloud \
  --rebuild-svg \
  --rebuild-chart \
  --embed-fonts \
  --map-motion
```

## 认证与配置

仅云端执行和云端专属功能需要认证。本地转换无需 API Key。

```bash
html2deck auth login
html2deck auth status
html2deck config set api-key <key>
```

在 CI、Docker 或 Agent 环境中，可使用环境变量（优先级高于本地存储的凭据）：

```bash
export HTML2DECK_API_KEY=your-api-key
```

持久化配置：

| 命令 | 说明 | 默认值 |
| --- | --- | --- |
| `html2deck config set api-key <key>` | 云端请求使用的 API Key | — |
| `html2deck config set size <size>` | PPTX 尺寸 | `1920x1080` |
| `html2deck config set webhook <url>` | 默认回调地址 | — |
| `html2deck config set retention-hours <n>` | 云端文件保留时长（小时） | `3` |

凭据保存在本地 `~/.deckflow/credentials`。

## CLI 参考

### 转换选项

| 选项 | 说明 | 默认值 |
| --- | --- | --- |
| `-h, --help` | 显示帮助 | — |
| `--version` | 显示版本 | — |
| `-o, --output <path>` | 输出路径 | 与输入同名同路径 |
| `-v, --verbose` | 详细日志输出到 stderr | `false` |
| `--quiet` | 仅输出错误和最终结果 | `false` |
| `--json` | stdout 输出机器可读 JSON | `false` |
| `--report` | 生成转换报告 | 关闭 |
| `--mode <mode>` | `auto`、`free`、`license` 或 `cloud` | `auto` |
| `--render-wait <seconds>` | 每页捕获前等待秒数 | `3` |
| `--format <format>` | `pptx` 或 `png` | `pptx` |
| `--webhook <url>` | 云端回调地址 | 配置项 |
| `--retention-hours <n>` | 云端文件保留时长（小时） | 配置项 |

`--quiet` 与 `--verbose` 不能同时使用。

### JSON 输出

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

## 编程 API

在 Node.js 中作为库使用：

```javascript
import { convertHtmlToPptx } from '@deckflow/html2deck';

const result = await convertHtmlToPptx({
  input: 'index.html',
  output: 'deck.pptx',
});
```

## 文档

详细 CLI 文档见 [`docs/cli/`](./docs/cli/) 目录。

### Agent Skill（Cursor / 编程代理）

安装 [`skill/html2deck/`](./skill/html2deck/)，让代理直接按约定把 HTML 转成 PPTX，无需反复查找 CLI 与幻灯片写作规则。说明见 [skill/README.md](./skill/README.md)。

## 许可证

专有软件。详见 [LICENSE](./LICENSE)。
