---
name: review
description: Use when the user wants to review the current state of a specific project, check progress, identify blockers, or get a status summary with next steps
---

# Review — Status de Projeto

## Overview

Resume o estado atual de um projeto especifico, cruzando documentacao do vault com estado real do codigo-fonte. Gera snapshot honesto de progresso, blockers e proximos passos.

## Workflow

### 1. Identificar Projeto

Resolver argumento do usuario para projeto:

| Input (case-insensitive) | Projeto | Repo Path |
|--------------------------|---------|-----------|
| zapflow, zap, alinhou, barber | ZapFlow | `/home/senna/whatsapp-saas` |
| scrap, furnace, jogo, game | Scrap Furnace | `/home/senna/scrap-furnace` |
| anime, animerec, rec | AnimeRec | `/home/senna/animerec` |
| crypto, arb, cryptoarb | CryptoArb | `/home/senna/cryptoarb` |
| remember, adhd, lembrete | Always Remember | `/home/senna/always-remember` |
| market, bot, marketplace | MarketBot (arquivo) | `/home/senna/marketplace-bot` |

Se input nao reconhecido, listar projetos disponiveis e perguntar.

### 2. Coletar Dados (paralelo)

| Fonte | Comando/Path | Info |
|-------|-------------|------|
| Contexto vault | `01 - Projetos/[X]/\_contexto.md` | Arquitetura, status documentado |
| Git log | `git log --oneline -20` no repo | Atividade recente |
| Git status | `git status` no repo | Changes pendentes |
| Branches | `git branch` no repo | Trabalho em paralelo |
| Design docs | `docs/plans/` no repo | Plano original, fases |
| Notas vault | `07 - Claude/*[projeto]*` | Analises anteriores |
| Tests | Runner do projeto (pytest/vitest) | Estado dos testes |

### 3. Output

Criar nota em `/mnt/c/Users/senna/ObsidianVault/07 - Claude/YYYY-MM-DD-review-[projeto-slug].md`:

```markdown
---
tags:
  - analise
  - projeto/[tag]
date: YYYY-MM-DD
---

# Review — [Projeto] — YYYY-MM-DD

## Status Geral
[1-2 frases: fase atual, saude do projeto, momentum]

## Ultimo Progresso
[Baseado em git log — ultimos commits relevantes]

## O Que Esta Funcionando
[Features/sistemas completos e funcionais]

## Blockers / Riscos
[O que impede progresso — ser honesto, nao suavizar]

## Divida Tecnica
[Se identificada no codigo ou docs]

## Proximos Passos
1. [Mais urgente]
2. [Segundo mais urgente]
3. [Desejavel]

## Metricas
| Metrica | Valor |
|---------|-------|
| Commits (30d) | X |
| Tests | X passing |
| Fase atual | X de Y |
| Ultimo commit | YYYY-MM-DD |
```

## Regras

- NUNCA escrever fora de `07 - Claude/`
- Ser **honesto** sobre blockers — transparencia > otimismo
- Se projeto esta em `04 - Arquivo/`, mencionar que esta arquivado e o motivo
- Incluir metricas concretas sempre que possivel
- Se testes falhando, reportar quais e por que
- Usar wikilinks para referenciar notas e projetos
- Nao rodar testes automaticamente sem confirmacao do usuario (podem ser lentos)
- Idioma: Portugues BR
