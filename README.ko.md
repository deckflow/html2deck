# HTML2Deck

<p align="center">
  <img src="https://raw.githubusercontent.com/deckflow/html2deck/main/assets/preview.png" alt="HTML2Deck — HTML to PPTX" width="800" />
</p>

**언어:** [English](./README.md) · [简体中文](./README.zh-CN.md) · [繁體中文](./README.zh-TW.md) · [Français](./README.fr.md) · [Deutsch](./README.de.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · **한국어**

터미널에서 HTML 파일, 표준 입력 또는 URL을 PPTX 또는 PNG 프레젠테이션으로 변환합니다.

<p align="center">
  <img src="https://raw.githubusercontent.com/deckflow/html2deck/main/assets/screenshots/demo.gif" alt="HTML → PPTX 데모" width="600" />
</p>

## 빠른 시작

설치 없이 실행:

```bash
npx -y @deckflow/html2deck@latest index.html -o deck.pptx
```

## 설치

```bash
npm install -g @deckflow/html2deck
html2deck --version
```

## HTML 변환

단일 HTML 파일 변환(기본 출력: 동일 경로 및 파일명):

```bash
html2deck index.html
# → index.pptx
```

출력 경로 지정:

```bash
html2deck index.html -o deck.pptx
```

표준 입력에서 HTML 읽기:

```bash
cat index.html | html2deck - -o deck.pptx
```

여러 HTML 파일을 순서대로 변환:

```bash
html2deck page1.html page2.html page3.html -o deck.pptx
```

호스팅된 페이지 변환:

```bash
html2deck https://example.com/deck.html -o deck.pptx
```

## 출력 형식

| 형식 | 설명 |
| --- | --- |
| `pptx` | PowerPoint 프레젠테이션(기본값) |
| `png` | PNG 프레임 출력 |

```bash
html2deck index.html --format png -o frames
```

## 실행 모드

| 모드 | 설명 |
| --- | --- |
| `auto` | API 키가 설정되어 있으면 클라우드, 아니면 로컬 |
| `local` | 항상 로컬 실행(API 키 불필요) |
| `cloud` | 항상 클라우드 실행(API 키 필요) |

```bash
html2deck index.html --mode local
html2deck index.html --mode cloud -o deck.pptx
```

### 클라우드 전용 기능

다음 옵션은 클라우드 모드가 필요합니다:

| 옵션 | 설명 |
| --- | --- |
| `--rebuild-svg` | SVG 객체 재구성 |
| `--rebuild-chart` | 차트 재구성 |
| `--embed-fonts` | 글꼴 임베드 |
| `--map-motion` | 애니메이션 매핑 |

```bash
html2deck index.html \
  -o deck.pptx \
  --mode cloud \
  --rebuild-svg \
  --rebuild-chart \
  --embed-fonts \
  --map-motion
```

## 인증 및 설정

인증은 클라우드 실행 및 클라우드 전용 옵션에만 필요합니다. 로컬 변환은 API 키 없이 동작합니다.

```bash
html2deck auth login
html2deck auth status
html2deck config set api-key <key>
```

CI, Docker, 에이전트 환경에서는 환경 변수 설정(저장된 자격 증명보다 우선):

```bash
export HTML2DECK_API_KEY=your-api-key
```

영구 설정:

| 명령 | 설명 | 기본값 |
| --- | --- | --- |
| `html2deck config set api-key <key>` | 클라우드 요청용 API 키 | — |
| `html2deck config set size <size>` | PPTX 크기 | `1920x1080` |
| `html2deck config set webhook <url>` | 기본 콜백 URL | — |
| `html2deck config set retention-hours <n>` | 클라우드 파일 보존 시간(시간) | `3` |

자격 증명은 로컬 `~/.deckflow/credentials`에 저장됩니다.

## CLI 참조

### 변환 옵션

| 옵션 | 설명 | 기본값 |
| --- | --- | --- |
| `-h, --help` | 도움말 표시 | — |
| `--version` | 버전 표시 | — |
| `-o, --output <path>` | 출력 경로 | 입력과 동일한 이름 |
| `-v, --verbose` | stderr에 상세 로그 출력 | `false` |
| `--quiet` | 오류 및 최종 결과만 출력 | `false` |
| `--json` | stdout에 기계 판독 가능 JSON 출력 | `false` |
| `--report` | 변환 보고서 생성 | 끔 |
| `--mode <mode>` | `auto`, `local` 또는 `cloud` | `auto` |
| `--render-wait <seconds>` | 각 페이지 캡처 전 대기 시간(초) | `3` |
| `--format <format>` | `pptx` 또는 `png` | `pptx` |
| `--webhook <url>` | 클라우드 콜백 URL | 설정 |
| `--retention-hours <n>` | 클라우드 파일 보존 시간(시간) | 설정 |

`--quiet`와 `--verbose`는 함께 사용할 수 없습니다.

### JSON 출력

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

## 프로그래밍 API

Node.js 라이브러리로 사용:

```javascript
import { convertHtmlToPptx } from '@deckflow/html2deck';

const result = await convertHtmlToPptx({
  input: 'index.html',
  output: 'deck.pptx',
});
```

## 문서

자세한 CLI 문서는 [`docs/cli/`](./docs/cli/) 디렉터리를 참조하세요.

## 라이선스

독점 소프트웨어. 자세한 내용은 [LICENSE](./LICENSE)를 참조하세요.
