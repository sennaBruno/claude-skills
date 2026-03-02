---
name: obsidian-second-brain
description: Use when setting up an Obsidian vault as a Second Brain from scratch, auditing an existing vault configuration, or bootstrapping PARA structure with Claude Code integration, automated backup, and developer-focused workflows
---

# Obsidian Second Brain — Setup & Audit

## Overview

Complete setup guide for an Obsidian vault as a Second Brain with PARA methodology, Claude Code integration (MCP + hooks + skills), automated Git backup, and developer-focused workflows. Works from zero or audits/completes an existing setup.

## When to Use

- Setting up a new Obsidian vault from scratch
- Auditing an existing vault for missing config/plugins/structure
- Adding Claude Code integration to an Obsidian vault
- Setting up automated backup with Git
- Configuring developer-focused PKM workflows (TILs, project context, investment tracking, hobby tracking)

## Quick Reference

| Layer | Components |
|-------|-----------|
| **Structure** | PARA folders, CLAUDE.md, _contexto.md per project, Dashboard |
| **Plugins** | Dataview, Calendar, Templater, QuickAdd, Git, Tasks, Omnisearch, Linter, Tag Wrangler, Periodic Notes |
| **Templates** | Diario, Nota, Semana, Mensal, Ativo, Pessoa, TIL |
| **Claude Integration** | MCP server, 5 skills (/capture, /today, /emerge, /connect, /review), PostToolUse hook, cron sync |
| **Backup** | Git repo, obsidian-git plugin, cron job, auto-commit hook |
| **Tags** | Hierarchical (#projeto/x, #area/y) + Flat (#til, #decisao, #conexao) |

## Implementation

### Phase 1: Vault Structure (PARA)

Create the folder tree:

```
VaultRoot/
├── 00 - Inbox/                    # Quick capture, unprocessed notes
├── 00 - Dashboard.md              # Homepage with Dataview queries
├── 01 - Projetos/                 # Active projects with _contexto.md
│   └── ProjectName/
│       └── _contexto.md           # Project context file ("Gema")
├── 02 - Areas/                    # Permanent life domains
│   ├── Dev/TILs/                  # Today I Learned notes
│   ├── Investimentos/
│   │   ├── Ativos/                # Individual stock/asset notes
│   │   ├── Dividendos/            # Monthly dividend tracking
│   │   └── Analises/              # Investment analyses
│   ├── Hobbies/
│   │   ├── Anime/
│   │   ├── Manga/
│   │   ├── Games/
│   │   └── Board Games/
│   └── Pessoas/                   # Personal CRM
├── 03 - Recursos/                 # Reference material, links, clippings
├── 04 - Arquivo/                  # Completed/paused projects
├── 05 - Templates/                # Reusable note templates
├── 06 - Diario/                   # Daily notes (NEVER write here via Claude)
│   └── Semanas/                   # Weekly review notes
├── 07 - Claude/                   # Claude-generated analyses (write here)
└── CLAUDE.md                      # Root context for Claude Code
```

### Phase 2: CLAUDE.md (Root Context)

Create `CLAUDE.md` at vault root with:

1. **Identity**: Owner name, role, objective, language
2. **Vault structure**: Folder tree with descriptions
3. **Operation rules**:
   - READ: Full vault access
   - WRITE ALLOWED: Only `07 - Claude/` and `00 - Inbox/`
   - WRITE FORBIDDEN: Never overwrite `06 - Diario/` or personal notes
   - EDIT existing: Only with explicit permission
4. **Project mapping**: Table of project name → code repo path → stack
5. **Tag system**: Hierarchical + flat tags with examples
6. **Skills reference**: Table of available slash commands

### Phase 3: Templates (7 templates)

All templates use `{{date}}` (YYYY-MM-DD) and `{{title}}` Templater variables.

| Template | Frontmatter Tags | Key Sections |
|----------|-----------------|--------------|
| **Diario** | `tipo/reflexao` | Como estou, O que fiz, O que aprendi, Ideias, Amanha |
| **Nota** | `[]` + projeto + status | Contexto, Conteudo, Conexoes, Proximos Passos |
| **Semana** | `tipo/revisao-semanal` | Dataview inbox, Projetos ativos, Tasks pendentes, Highlights, Proxima semana top 3 |
| **Mensal** | `tipo/revisao-mensal` | Projetos, Investimentos, Aprendizado, Hobbies, 3 palavras, Intencao |
| **Ativo** | `area/investimentos/acoes` + ticker/setor/preco_medio | Tese, Dividendos (table), Pontos de Atencao, Revisao |
| **Pessoa** | `pessoa` + nome/birthday/relacao/contato | Como Nos Conhecemos, Interesses, Ultimas Conversas, Presentes, Lembrar |
| **TIL** | `til` + `area/dev` + tech | O Que Aprendi, Contexto, Exemplo (code block), Links |

### Phase 4: Plugins (10 community plugins)

Install plugins to `.obsidian/plugins/` and register in `community-plugins.json`.

**Tier 1 (Essential):**

| Plugin | ID | Key Config |
|--------|----|-----------|
| Dataview | `dataview` | `enableDataviewJs: true`, `enableInlineDataview: true`, `defaultDateFormat: "yyyy-MM-dd"` |
| Templater | `templater-obsidian` | `templates_folder: "05 - Templates"`, `trigger_on_file_creation: true`, folder templates mapped (see below) |
| Calendar | `calendar` | `shouldConfirmBeforeCreate: true` |
| QuickAdd | `quickadd` | Capture "Captura Inbox" configured |
| Tasks | `obsidian-tasks-plugin` | Default config |

**Tier 2 (Power User):**

| Plugin | ID | Key Config |
|--------|----|-----------|
| Periodic Notes | `obsidian-periodic-notes` | Daily/Weekly/Monthly enabled with template + folder mapping |
| Omnisearch | `obsidian-omnisearch` | Default (hotkey `Ctrl+Shift+O`) |
| Linter | `obsidian-linter` | YAML Timestamp on `date` field, trailing spaces, `lintOnSave: true`, ignore Templates folder |
| Tag Wrangler | `tag-wrangler` | Default |
| obsidian-git | `obsidian-git` | `autoSaveInterval: 10`, `autoPullOnBoot: true`, `commitMessage: "vault: {{date}}"` |

**Templater Folder Templates:**

| Folder | Template |
|--------|---------|
| `01 - Projetos` | Template Nota |
| `02 - Areas/Investimentos/Ativos` | Template Ativo |
| `02 - Areas/Pessoas` | Template Pessoa |
| `02 - Areas/Dev/TILs` | Template TIL |
| `02 - Areas` | Template Nota |
| `03 - Recursos` | Template Nota |
| `06 - Diario/Semanas` | Template Semana |
| `06 - Diario` | Template Diario |
| `07 - Claude` | Template Nota |

**Periodic Notes Config:**

| Period | Format | Folder | Template |
|--------|--------|--------|---------|
| Daily | `YYYY-MM-DD` | `06 - Diario` | Template Diario |
| Weekly | `YYYY-[W]ww` | `06 - Diario/Semanas` | Template Semana |
| Monthly | `YYYY-MM` | `06 - Diario` | Template Mensal |

### Phase 5: Dashboard

Create `00 - Dashboard.md` with Dataview queries for:

1. **Projetos Ativos** — TABLE from `01 - Projetos` where `_contexto` exists
2. **Tarefas Pendentes** — Tasks query `not done`, sorted by due, limit 10
3. **Inbox Pendente** — LIST from `00 - Inbox`, sorted by creation
4. **Ultimas Notas** — TABLE last 10 modified notes
5. **Investimentos** — TABLE from `02 - Areas/Investimentos/Ativos`
6. **TILs Recentes** — TABLE from `#til`, last 5
7. **Notas Orfas** — LIST notes with 0 inlinks and 0 outlinks

### Phase 6: Hotkeys

Configure `.obsidian/hotkeys.json`:

| Hotkey | Action |
|--------|--------|
| `Ctrl+Shift+I` | QuickAdd Captura Inbox |
| `Ctrl+Shift+D` | Open daily note |
| `Ctrl+Shift+O` | Omnisearch |
| `Alt+G` | Open graph view |
| `Alt+T` | Insert Templater template |
| `Alt+Left` | Navigate back |
| `Alt+Right` | Navigate forward |

### Phase 7: Git Backup

1. **Init repo** in vault root with `.gitignore`:
   ```
   .obsidian/workspace.json
   .obsidian/workspace
   .obsidian/plugins/obsidian-git/data.json
   .trash/
   .DS_Store
   Thumbs.db
   desktop.ini
   ```

2. **Create private GitHub repo** and push initial commit

3. **Install obsidian-git plugin** — auto-commits every 10 min when Obsidian is open

4. **Cron job** (15 min) for persistent backup even without Obsidian:
   ```bash
   #!/usr/bin/env bash
   VAULT="/path/to/vault"
   cd "$VAULT" || exit 0
   if git diff --quiet && git diff --cached --quiet && [ -z "$(git ls-files --others --exclude-standard)" ]; then
       exit 0
   fi
   git add --all
   git commit -m "vault: sync $(date +'%Y-%m-%d %H:%M')"
   git push origin main 2>/dev/null
   exit 0
   ```
   Register: `*/15 * * * * /path/to/vault-cron-sync.sh >> /tmp/vault-sync.log 2>&1`

5. Ensure cron starts on boot (WSL: `systemd=true` in `/etc/wsl.conf`)

### Phase 8: Claude Code Integration

**MCP Server:**
```bash
claude mcp add obsidian --scope user -- npx @mauricio.wolff/mcp-obsidian@latest "/path/to/vault"
```
- Filesystem-only (works without Obsidian GUI running)
- Path traversal protection, TOCTOU fixes

**PostToolUse Hook** (`vault-autocommit.sh`):
- Triggers on Write/Edit tools
- Auto-commits if file is inside the vault
- Runs async (non-blocking)
- Register in `~/.claude/settings.json` under `hooks.PostToolUse`

**Skills** (install to `~/.claude/skills/`):

| Skill | Trigger | Output Location |
|-------|---------|----------------|
| `/capture` | Quick note to Inbox | `00 - Inbox/` |
| `/today` | Daily planning briefing | `07 - Claude/` |
| `/emerge` | Reveal implicit ideas from recent notes | `07 - Claude/` |
| `/connect` | Cross-domain pattern linking | `07 - Claude/` |
| `/review [project]` | Project status summary | `07 - Claude/` |

**Community Skills:**
```bash
npx skills add kepano/obsidian-skills -g -a claude-code -y
```
Installs: obsidian-markdown, obsidian-bases, obsidian-cli, json-canvas, defuddle

### Phase 9: Audit (Existing Vault)

When auditing an existing vault, check each layer:

```
1. PARA folders exist?           → Create missing ones
2. CLAUDE.md exists and current? → Create or update
3. Templates complete?           → Create missing, verify frontmatter
4. Plugins installed + configured? → Install missing, verify data.json
5. Dashboard exists?             → Create with Dataview queries
6. Hotkeys configured?           → Set up hotkeys.json
7. Git backup active?            → Init repo, .gitignore, cron, push
8. MCP server configured?       → Add via claude mcp add
9. Skills installed?             → Install community + custom skills
10. Hooks registered?            → Add PostToolUse auto-commit hook
```

## Tag System

### Hierarchical (group by domain)
```
#projeto/name          # Active project
#area/dev              # Development knowledge
#area/investimentos    # Investments
#area/investimentos/dividendos
#area/investimentos/acoes
#hobby/anime
#hobby/manga
#hobby/gaming
#hobby/boardgames
```

### Flat (quick filter)
```
#til        #decisao      #ideia       #analise
#reflexao   #conexao      #ativo       #pausado
#concluido  #bug          #referencia  #pessoa
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Writing to `06 - Diario/` from Claude | Output goes to `07 - Claude/` ALWAYS |
| Forgetting `.gitignore` for workspace.json | Obsidian changes this constantly, creates noise |
| Not ignoring obsidian-git data.json | Creates sync loops between plugin and cron |
| Generic templates without frontmatter | Every template needs YAML with tags + date |
| Linter running on Templates folder | Add `05 - Templates` to `foldersToIgnore` |
| No `_contexto.md` in project folders | Claude lacks project context without it |
| Flat-only tags | Hierarchical tags enable powerful Dataview filtering |
