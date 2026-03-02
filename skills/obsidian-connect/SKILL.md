---
name: connect
description: Use when the user wants to find cross-domain connections, transversal patterns, unexpected links between projects, areas, hobbies, and investments in their knowledge base
---

# Connect — Conexoes Transversais

## Overview

Busca conexoes entre dominios distantes do vault: projetos de software, investimentos, hobbies (anime, gaming, board games), areas de conhecimento. O valor do segundo cerebro esta nas conexoes inesperadas entre mundos aparentemente separados.

## Workflow

### 1. Mapear Dominios (paralelo)

| Dominio | Path | Tipo de conteudo |
|---------|------|-----------------|
| Projetos | `01 - Projetos/*/\_contexto.md` | Arquitetura, patterns, decisoes |
| Dev | `02 - Areas/Dev/` | TILs, patterns, ferramentas |
| Investimentos | `02 - Areas/Investimentos/` | Analises, estrategias, metricas |
| Hobbies | `02 - Areas/Hobbies/` | Anime, manga, gaming, board games |
| Analises | `07 - Claude/` | Conexoes anteriores |
| Inbox | `00 - Inbox/` | Ideias recentes |

### 2. Cruzar Dominios

Tipos de conexao a buscar:

- **Pattern transfer**: pattern de arquitetura de um projeto que resolve problema em outro
- **Mecanica → Feature**: mecanica de jogo (idle, board game) que inspira feature de SaaS
- **Metrica → Metrica**: indicador de investimento que mapeia para metrica de produto
- **Narrativa → UX**: conceito de anime/manga que informa design de experiencia
- **Problema → Solucao**: problema resolvido num dominio que existe nao-resolvido em outro
- **Ferramenta → Ferramenta**: tecnica transferivel entre contextos

### 3. Output

Criar nota em `/mnt/c/Users/senna/ObsidianVault/07 - Claude/YYYY-MM-DD-connect.md`:

```markdown
---
tags:
  - conexao
  - analise
date: YYYY-MM-DD
---

# Connect — YYYY-MM-DD

## Conexoes Encontradas

### [Titulo descritivo da conexao]
- **De**: [[dominio A]] → **Para**: [[dominio B]]
- **Insight**: [explicacao clara]
- **Acao possivel**: [como aplicar concretamente]

### [Proxima conexao]
...

## Meta-Padroes
[Temas que aparecem em 3+ dominios — padroes de padroes]
```

## Exemplos de Conexoes Validas

| De | Para | Conexao |
|----|------|---------|
| EventBus (Scrap Furnace) | Desacoplamento (ZapFlow) | Signal pattern resolve acoplamento de modulos |
| Curvas exponenciais (idle games) | Pricing tiers (SaaS) | Mesma psicologia de progressao |
| Pipeline ML (AnimeRec) | Recomendacao board games | Algoritmo adaptavel para outro dominio |
| Analise dividendos (BBAS3) | Metricas retention (SaaS) | Yield vs churn como indicadores de saude |
| Isekai (anime) | Onboarding (app) | Transportar usuario para mundo novo progressivamente |

## Regras

- NUNCA escrever fora de `07 - Claude/`
- Conexoes devem ser **actionable** — nao apenas "X lembra Y"
- Minimo 3 conexoes por execucao
- Tag obrigatoria: `#conexao`
- Usar wikilinks extensivamente para criar grafo de conhecimento
- Priorizar conexoes **surpreendentes** sobre obvias
- Se conexao ja foi feita em analise anterior, mencionar e expandir
- Idioma: Portugues BR
