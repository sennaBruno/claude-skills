# Claude Skills

A collection of reusable skills for [Claude Code](https://claude.ai/code) and compatible AI coding agents.

## Skills

| Skill | Description |
|-------|-------------|
| [estimating-agent-tasks](skills/estimating-agent-tasks/) | ACR (Agent Complexity Rating) framework — replaces story points for AI-agent-assisted development. Estimates time, cost, model selection, and token budget per task. |
| [video-to-context](skills/video-to-context/) | Extract full context from screen recordings (speech transcript via Whisper GPU + key frames via ffmpeg) into a markdown document AI agents can read directly. |
| [quality-gate](skills/quality-gate/) | Comprehensive batch validation across 10 dimensions: code quality, OWASP security, frontend/backend performance, UI/UX, accessibility, design system, API design, data safety, and licensing. |
| [obsidian-second-brain](skills/obsidian-second-brain/) | Complete setup guide for Obsidian vault as Second Brain with PARA methodology, Claude Code integration, automated Git backup, and developer-focused workflows. |
| [evolution-api-management](skills/evolution-api-management/) | Deploy, connect, troubleshoot, and manage Evolution API WhatsApp instances on ECS Fargate. Covers QR code generation, webhook JWT, session lifecycle, and environment isolation. |
| [obsidian-capture](skills/obsidian-capture/) | Quickly capture notes, ideas, thoughts, links, or snippets into your Obsidian Inbox for later processing. |
| [obsidian-connect](skills/obsidian-connect/) | Find cross-domain connections, transversal patterns, and unexpected links between projects, areas, hobbies, and investments in your knowledge base. |
| [obsidian-emerge](skills/obsidian-emerge/) | Discover hidden ideas, implicit patterns, unfinished thoughts, or emergent insights from recent notes and vault activity. |
| [obsidian-review](skills/obsidian-review/) | Review the current state of a specific project, check progress, identify blockers, or get a status summary with next steps. |
| [obsidian-today](skills/obsidian-today/) | Plan your day, get a daily briefing, review pending tasks across projects, or decide what to work on today. |
| [validate-claude-code-setup](skills/validate-claude-code-setup/) | Audit and re-validate Claude Code configuration (model, context window, autocompact threshold, effort level) against current best practices. Research-first approach with decision tables and symptom-based diagnostics. |

## Community / Backup

Skills created by others, archived here for backup. Not original work — use at your own discretion.

| Skill | Description |
|-------|-------------|
| [interface-design](community/interface-design/) | Dashboard, admin panel, app, and interactive product design guidelines. |
| [svg-logo-designer](community/svg-logo-designer/) | Create professional SVG logos from descriptions and design specifications. |
| [product-gap-analysis](community/product-gap-analysis/) | Analyze external research to identify product improvement opportunities. |
| [using-stitch-mcp](community/using-stitch-mcp/) | Design UI screens and generate component layouts using the Stitch MCP server. |

## Installation

### Claude Code (local)

Copy the skill folder to your personal skills directory:

```bash
cp -r skills/estimating-agent-tasks ~/.claude/skills/
```

### Claude Code (project-level)

Copy to your project's `.claude/skills/` directory to share with your team:

```bash
cp -r skills/estimating-agent-tasks .claude/skills/
```

### Plugin marketplace

```
/plugin marketplace add sennaBruno/claude-skills
```

## Usage

Skills are automatically loaded by Claude when relevant. You can also invoke them directly:

- **estimating-agent-tasks**: Mention estimation, story points, sprint planning, task sizing, or token budgeting in your prompt.
- **video-to-context**: Share a screen recording, demo video, or bug reproduction video and ask Claude to analyze it.
- **quality-gate**: Run after completing a feature or before merging — validates across 10 quality dimensions.
- **obsidian-second-brain**: Ask to set up or audit an Obsidian vault with PARA methodology.
- **evolution-api-management**: Mention deploying, connecting, or troubleshooting WhatsApp via Evolution API.
- **obsidian-capture/connect/emerge/review/today**: Use for daily Obsidian workflows — capturing, connecting ideas, reviewing projects, and planning.
- **validate-claude-code-setup**: Say "valida meu setup do Claude Code" or invoke directly to audit model, context window, autocompact threshold, and effort level.

## Contributing

1. Fork this repo
2. Add your skill under `skills/<skill-name>/SKILL.md`
3. Follow the [Anthropic skill format](https://code.claude.com/docs/en/skills)
4. Open a PR

## License

Apache 2.0 — see [LICENSE](LICENSE).
