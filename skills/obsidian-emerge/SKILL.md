---
name: emerge
description: Use when the user wants to discover hidden ideas, implicit patterns, unfinished thoughts, or emergent insights from their recent notes and vault activity
---

# Emerge — Revelacao de Ideias Implicitas

## Overview

Analisa notas recentes do vault para revelar ideias implicitas, pensamentos incompletos e padroes emergentes que o usuario pode nao ter percebido conscientemente.

## Workflow

### 1. Varredura (paralelo)

| Fonte | Periodo | O que buscar |
|-------|---------|--------------|
| `00 - Inbox/` | 14 dias | Ideias brutas, temas recorrentes |
| `06 - Diario/` | 14 dias | Emocoes, energia, preocupacoes (SOMENTE LEITURA) |
| `07 - Claude/` | 14 dias | Analises anteriores, conexoes ja feitas |
| `01 - Projetos/*/\_contexto.md` | atual | Estado dos projetos, decisoes recentes |
| `02 - Areas/` | 30 dias | Notas modificadas recentemente |

### 2. Analise Profunda

Buscar estes padroes:

- **Temas recorrentes**: palavras, conceitos ou preocupacoes que aparecem em 3+ notas
- **Pensamentos incompletos**: ideias mencionadas de passagem mas nunca desenvolvidas
- **Contradicoes**: notas que dizem coisas opostas sobre mesmo tema
- **Decisoes em loop**: mesma decisao sendo reconsiderada repetidamente
- **Energia implicita**: o que aparece mais frequentemente = o que realmente importa (vs. o que o usuario diz que importa)
- **Perguntas nao feitas**: questoes que as notas levantam mas nunca respondem

### 3. Output

Criar nota em `/mnt/c/Users/senna/ObsidianVault/07 - Claude/YYYY-MM-DD-emerge.md`:

```markdown
---
tags:
  - analise
  - conexao
date: YYYY-MM-DD
---

# Emerge — YYYY-MM-DD

## Ideias Implicitas
[Ideias que estao "entre linhas" nas notas recentes]
- Em [[nota X]], voce mencionou Y de passagem — isso pode significar...

## Pensamentos Incompletos
[Coisas iniciadas mas nao desenvolvidas]
- [[nota Z]] levanta a questao de... mas para ali

## Padroes de Energia
[Onde atencao esta sendo investida vs onde diz querer investir]

## Contradicoes Interessantes
[Tensoes produtivas entre ideias]

## Perguntas Nao Feitas
[Questoes que as notas levantam implicitamente]

## Sugestoes
[Acoes concretas: desenvolver ideia X, resolver tensao Y, explorar pergunta Z]
```

## Regras

- NUNCA escrever fora de `07 - Claude/`
- Tom: **curioso, nao prescritivo**. "Percebi que..." — nunca "Voce deveria..."
- Nao julgar prioridades do usuario — apenas revelar padroes
- Citar fontes com wikilinks: "Em [[nota X]], voce mencionou..."
- Respeitar privacidade: do diario extrair apenas padroes, nunca citar conteudo literal sensivel
- Minimo 3 insights por execucao, maximo 10
- Se nao encontrar padroes significativos, dizer honestamente
- Idioma: Portugues BR
