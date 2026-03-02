---
name: capture
description: Use when the user wants to quickly capture a note, idea, thought, link, or snippet into their Obsidian Inbox for later processing
---

# Capture — Nota Rapida ao Inbox

## Overview

Captura rapida de pensamento, ideia, link ou snippet no Inbox do Obsidian vault. Prioriza velocidade sobre estrutura.

## Workflow

1. Receba o conteudo do usuario (texto livre, ideia, link, snippet, etc.)
2. Gere titulo curto baseado no conteudo (max 8 palavras)
3. Identifique tags relevantes pelo conteudo
4. Crie arquivo em `/mnt/c/Users/senna/ObsidianVault/00 - Inbox/`

## Formato do Arquivo

**Nome**: `YYYY-MM-DD-titulo-slug.md` (lowercase, hifens, sem acentos)

```markdown
---
tags:
  - [tag principal: ideia, til, referencia, bug, decisao]
  - [tag de dominio se obvio: projeto/zapflow, area/dev, hobby/anime]
date: YYYY-MM-DD
---

# [Titulo]

[Conteudo do usuario, formatado se necessario]

## Conexoes
- [[wikilinks relevantes se obvias]]
```

## Selecao de Tags

| Conteudo | Tag Principal | Tag Dominio |
|----------|---------------|-------------|
| Ideia bruta | `#ideia` | contexto se claro |
| Aprendizado | `#til` | `#area/dev` etc |
| Link/artigo | `#referencia` | pelo tema |
| Bug resolvido | `#bug` | `#projeto/X` |
| Decisao | `#decisao` | `#projeto/X` |

## Regras

- SEMPRE escrever em `00 - Inbox/` — nunca em outro diretorio
- Se conteudo ambiguo, usar tag `#ideia` como default
- Adicionar wikilinks SOMENTE quando conexao for obvia
- Responder com confirmacao: path do arquivo + tags usadas
- Idioma: Portugues BR
- Se usuario passar multiplos itens, criar UM arquivo por item
