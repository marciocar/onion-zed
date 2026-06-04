---
name: onion-warmup
description: >
  Prepara o contexto geral do Sistema Onion para uma sessão de trabalho. Use no
  início de uma sessão, ao retornar ao projeto após ausência, ou quando o usuário
  pedir "warm-up", "me dá o contexto", "visão geral do projeto", "por onde começo".
  Carrega README, índice de docs, meta-specs e estado do Task Manager.
disable-model-invocation: true
---

## Objetivo

Estabelecer contexto completo do projeto: visão geral, estrutura de documentação, meta-specs (constituição) e estado atual das integrações.

## Passos (use as tools de leitura)

1. **Estado do projeto** (tool `terminal`/`read_file`):
   - Provider ativo: `grep TASK_MANAGER_PROVIDER .env`
   - Branch atual: `git branch --show-current`
   - Sessões abertas: `list_directory` em `.agents/onion/sessions/`
2. **Identidade e visão**: `read_file` em `README.md` e `AGENTS.md` (rules).
3. **Índice de documentação**: `read_file` em `docs/INDEX.md` (hub central).
4. **Meta-specs (constituição L0)**: `read_file` em `docs/meta-specs/index.md` e no `docs/meta-specs/adr/0001-zed-native-port.md`.
5. **Mapa de recursos**: `list_directory` em `.agents/skills/` (skills disponíveis) e `.agents/onion/specialists/` (specialists delegáveis).

## Contexto a manter na sessão

- **Três dimensões peer**: produto, engenharia, compliance/governança
- **Modelo Zed**: skills (`/onion-<cat>-<cmd>`) + specialists (via `spawn_agent`)
- **Workflows faseados invariantes**: `product/collect→feature`, `engineer/plan→pr-update`
- **Task Manager Abstraction**: detectar provider via `.env` antes de operar com tasks

## Saída esperada

Um resumo conciso com: identidade do projeto, provider ativo, inventário de skills/specialists, meta-specs vigentes e sugestão de próximos passos (skill de categoria: `/onion-engineer-warm-up` ou `/onion-product-warm-up`).

## Quando usar

✅ Primeira vez no projeto · ✅ Retorno após ausência · ✅ Mudança de contexto · ✅ Necessidade de visão geral

## Referências

- Orquestrador: skill `onion` (routing por intenção)
- Rules: `AGENTS.md`
- Índice: `docs/INDEX.md`
