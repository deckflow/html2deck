# HTML2Deck skills & agent prompts

Agent instructions for HTML → PPTX conversion via `@deckflow/html2deck`.

Lives in the public repo ([deckflow/html2deck](https://github.com/deckflow/html2deck)) so coding agents and downstream projects can install it without the private package tree.

## Layout

```
skill/
├── README.md                 ← you are here
├── html2deck/                ← Cursor Agent Skill (primary)
│   ├── SKILL.md
│   ├── examples.md
│   ├── templates/
│   │   └── basic-deck.html   ← full starter deck (copy & edit)
│   └── references/
│       ├── cli.md
│       ├── html-authoring.md
│       ├── animations.md
│       ├── slide-transitions.md
│       ├── output-contract.md
│       └── troubleshooting.md
└── agent-prompts/            ← drop-in prompt snippets for other agents
    ├── convert-html-to-pptx.md
    └── review-conversion.md
```

## Install for Cursor

**Project skill** (this public repo as the workspace):

```bash
mkdir -p .cursor/skills
ln -sfn ../../skill/html2deck .cursor/skills/html2deck
```

Or copy `skill/html2deck` to `.cursor/skills/html2deck`.

**Personal skill** (all projects):

```bash
git clone https://github.com/deckflow/html2deck.git
mkdir -p ~/.cursor/skills
ln -sfn "$(pwd)/html2deck/skill/html2deck" ~/.cursor/skills/html2deck
```

After install, agents should auto-discover the skill when the user asks for HTML→PPTX / html2deck conversion. You can also say: “用 html2deck skill 把这个 HTML 转成 PPTX”.

**Private package checkout** (nested `public/`):

```bash
mkdir -p .cursor/skills
ln -sfn ../../public/skill/html2deck .cursor/skills/html2deck
```

## Agent defaults

1. Prefer `--mode free --json`
2. Author slides as `.slide-container` at 1280×720 (or declare size / pass matching `--width`)
3. Fix HTML and re-convert when fidelity is wrong
4. Use cloud only for PNG or `--rebuild-*` / `--embed-fonts` / `--map-motion`
