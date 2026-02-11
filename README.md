# Claude Skills

A collection of reusable skills for [Claude Code](https://claude.ai/code) and compatible AI coding agents.

## Skills

| Skill | Description |
|-------|-------------|
| [estimating-agent-tasks](skills/estimating-agent-tasks/) | ACR (Agent Complexity Rating) framework — replaces story points for AI-agent-assisted development. Estimates time, cost, model selection, and token budget per task. |
| [video-to-context](skills/video-to-context/) | Extract full context from screen recordings (speech transcript via Whisper GPU + key frames via ffmpeg) into a markdown document AI agents can read directly. |

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

## Contributing

1. Fork this repo
2. Add your skill under `skills/<skill-name>/SKILL.md`
3. Follow the [Anthropic skill format](https://code.claude.com/docs/en/skills)
4. Open a PR

## License

Apache 2.0 — see [LICENSE](LICENSE).
