---
name: today
description: Use when the user wants to plan their day, get a daily briefing, review pending tasks across projects, or asks about what to work on today
---

# Today — Planejamento Diario

## Overview

Gera briefing diario cruzando projetos ativos, notas recentes e tarefas pendentes. Ajuda o usuario a decidir o que fazer hoje com base em contexto real.

## Workflow

### 1. Coleta (paralelo com subagents ou ferramentas)

Ler as seguintes fontes:

| Fonte | Path | Permissao |
|-------|------|-----------|
| Inbox nao processado | `00 - Inbox/*.md` (ultimos 7 dias) | LEITURA |
| Diario recente | `06 - Diario/*.md` (ultimos 3 dias) | SOMENTE LEITURA |
| Contextos de projeto | `01 - Projetos/*/\_contexto.md` | LEITURA |
| Analises Claude | `07 - Claude/*.md` (ultimos 7 dias) | LEITURA |

### 2. Analise

- **Tarefas pendentes**: extrair secoes "Proximos Passos" de cada `_contexto.md`
- **Momentum**: projetos com commits recentes > projetos parados
- **Inbox**: notas que precisam ser triadas/processadas
- **Energia**: se diario menciona humor/energia, considerar na priorizacao
- **Conexoes**: padroes entre notas recentes de dominios diferentes

### 3. Output

Criar nota em `/mnt/c/Users/senna/ObsidianVault/07 - Claude/YYYY-MM-DD-today.md`:

```markdown
---
tags:
  - analise
date: YYYY-MM-DD
---

# Today — YYYY-MM-DD

## Panorama
[1-3 frases: estado geral, energia percebida, foco sugerido]

## Projetos Ativos
### [[ZapFlow]]
- Status: [fase atual]
- Pendente: [proximos passos mais urgentes]
- Momentum: [alto/medio/baixo baseado em atividade recente]

### [[Scrap Furnace]]
...

[Repetir para cada projeto ativo]

## Inbox Pendente
- [Notas que precisam triagem]

## Sugestoes para Hoje
1. [Tarefa curta e concreta — quick win primeiro]
2. [Tarefa de foco profundo]
3. [Tarefa opcional se sobrar energia]

## Conexoes Notadas
- [Padroes entre dominios, se houver]
```

## Regras

- NUNCA escrever em `06 - Diario/` — somente ler para contexto
- Output vai para `07 - Claude/` sempre
- Priorizar quick wins primeiro (ADHD-friendly)
- Ser honesto sobre blockers — nao suavizar problemas
- Se nao ha notas recentes em alguma fonte, ignorar sem mencionar
- Projetos em `04 - Arquivo/` NAO entram no briefing
- Usar wikilinks para todos projetos e notas referenciadas
- Idioma: Portugues BR

## Projetos Ativos (Referencia)

| Projeto | Path | Tag |
|---------|------|-----|
| ZapFlow | `/home/senna/whatsapp-saas` | `#projeto/zapflow` |
| Scrap Furnace | `/home/senna/scrap-furnace` | `#projeto/scrap-furnace` |
| AnimeRec | `/home/senna/animerec` | `#projeto/animerec` |
| CryptoArb | `/home/senna/cryptoarb` | `#projeto/cryptoarb` |
| Always Remember | `/home/senna/always-remember` | `#projeto/always-remember` |
