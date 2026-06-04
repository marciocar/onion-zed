---
name: onion
description: >
  Orquestrador mestre do Sistema Onion. Use quando o usuário precisar de orientação
  sobre por onde começar, qual skill ou specialist usar, como executar um fluxo de
  trabalho (feature, hotfix, PR, documentação, produto, testes), como configurar
  integrações (Jira, ClickUp, Asana, Linear), ou qualquer navegação dentro do framework.
  Ative também quando o usuário perguntar "o que faço agora?", "próximos passos",
  "como funciona o sistema?", "qual specialist para X?", "como crio Y?", mesmo sem
  mencionar "onion" explicitamente.
---

## Primeiro passo — Levantar estado do projeto

O Zed não injeta contexto dinâmico em skills. Antes de orientar, **colete o estado** usando as tools:

1. **Provider ativo**: `read_file` em `.env` (ou `terminal`: `grep TASK_MANAGER_PROVIDER .env`).
2. **Sessões abertas**: `list_directory` em `.agents/onion/sessions/`.
3. **Branch atual**: `terminal`: `git branch --show-current`.

---

## Modelo de execução (nativo Zed)

- **Skills** invocáveis por `/onion-<categoria>-<comando>` (ex.: `/onion-engineer-start`) ou `@skill`.
- **Specialists** (`.agents/onion/specialists/<x>.md`) delegados via tool `spawn_agent`: *"Leia `.agents/onion/specialists/<x>.md` e atue como esse especialista para: <tarefa>"*.

---

## Routing por Intenção

### Desenvolvimento de Feature
| Intenção | Skill / Specialist |
|----------|-----------------|
| Criar task / planejar feature | `/onion-product-task` |
| Iniciar desenvolvimento | `/onion-engineer-start` |
| Continuar trabalho em feature | `/onion-engineer-work` |
| Preparar PR (lint, testes, review) | `/onion-engineer-pre-pr` |
| Criar Pull Request | `/onion-engineer-pr` |
| Atualizar PR existente | `/onion-engineer-pr-update` |
| Hotfix urgente em produção | `/onion-engineer-hotfix` |
| Bump de versão (semver) | `/onion-engineer-bump` |

**Sequência completa de feature:**
```
/onion-product-task → /onion-engineer-start → /onion-engineer-work → /onion-engineer-pre-pr → /onion-engineer-pr
```

---

### Produto e Discovery
| Intenção | Skill / Specialist |
|----------|-----------------|
| Coletar requisitos / ideias | `/onion-product-collect` |
| Refinar requisitos | `/onion-product-refine` |
| Especificação de produto | `/onion-product-spec` |
| Estimar story points | `/onion-product-estimate` |
| Warm-up de produto | `/onion-product-warm-up` |
| Transcrever áudio / reunião | `/onion-product-whisper` |
| Extrair ata de reunião | `/onion-product-extract-meeting` |
| Consolidar múltiplas reuniões | `/onion-product-consolidate-meetings` |
| Converter documento em tasks | `/onion-product-convert-to-tasks` |
| Analisar dor do cliente | `/onion-product-analyze-pain-price` |
| Branding e posicionamento | `/onion-product-branding` |

**Sequência de discovery:**
```
/onion-product-collect → /onion-product-refine → /onion-product-spec → /onion-product-task
```

---

### Documentação
| Intenção | Skill / Specialist |
|----------|-----------------|
| Documentação técnica | `/onion-docs-build-tech-docs` |
| Documentação de negócio | `/onion-docs-build-business-docs` |
| Atualizar índice de docs | `/onion-docs-build-index` |
| Engenharia reversa de projeto | `/onion-docs-reverse-consolidate` |
| Validar documentação | `/onion-docs-validate-docs` |
| Diagrama de arquitetura C4 | `spawn_agent` → `specialists/c4-architecture-specialist.md` |
| Diagrama Mermaid | `spawn_agent` → `specialists/mermaid-specialist.md` |

---

### Criar Componentes do Onion
| Intenção | Skill |
|----------|---------|
| Novo specialist | `/onion-meta-create-agent` |
| Novo skill | `/onion-meta-create-skill` |
| Novo comando/skill | `/onion-meta-create-command` |
| Nova knowledge base | `/onion-meta-create-knowledge-base` |
| Configurar integração | `/onion-meta-setup-integration` |
| Análise de problema complexo | `/onion-meta-analyze-complex-problem` |

---

### Qualidade e Revisão
| Intenção | Skill / Specialist |
|----------|-----------------|
| Code review | `spawn_agent` → `specialists/code-reviewer.md` |
| Review de branch completa | `spawn_agent` → `specialists/branch-code-reviewer.md` |
| Testes unitários | `/onion-test-unit` |
| Testes de integração | `/onion-test-integration` |
| Testes E2E | `/onion-test-e2e` |
| Planejamento de testes | `spawn_agent` → `specialists/test-planner.md` |
| Validar conformidade arquitetural | `spawn_agent` → `specialists/metaspec-gate-keeper.md` |
| Validar workflow do Onion | `/onion-validate-workflow` |

---

### Git e Versionamento
| Intenção | Skill |
|----------|---------|
| Iniciar feature branch | `/onion-git-feature-start` |
| Finalizar feature | `/onion-git-feature-finish` |
| Publicar branch remota | `/onion-git-feature-publish` |
| Sincronizar com GitFlow | `/onion-git-sync` |
| Iniciar release | `/onion-git-release-start` |
| Finalizar release | `/onion-git-release-finish` |
| Commit rápido | `/onion-git-fast-commit` |

---

### Task Managers (Provider-Aware)
| Provider | Specialist | Quando usar |
|----------|--------|-------------|
| ClickUp | `specialists/clickup-specialist.md` | Tasks, listas, custom fields, MCP |
| Jira | `specialists/jira-specialist.md` | JQL, ADF, transitions, sprints (REST direto) |
| Qualquer / agnóstico | `specialists/task-specialist.md` | Decomposição, Asana, Linear |
| Estratégia / gestão | `specialists/product-agent.md` | Priorização, roadmap |

---

### Contexto e Navegação
| Intenção | Skill |
|----------|---------|
| Warm-up geral | `/onion-warmup` |
| Warm-up de engenharia | `/onion-engineer-warm-up` |
| Warm-up de produto | `/onion-product-warm-up` |
| Listar ferramentas | `/onion-meta-all-tools` |

---

## Gotchas Críticos

**Task Manager Provider obrigatório** — Antes de operar com tasks: ler `TASK_MANAGER_PROVIDER` no `.env` via tool. Válidos: `clickup`, `jira`, `asana`, `linear`, `none`. Se ausente/inválido: avisar e sugerir `/onion-meta-setup-integration`. Nunca inventar nem assumir.

**Feature slug: sempre kebab-case** — Correto: `user-authentication`. Errado: `user_authentication`, `UserAuth`. Usado na branch Git e na pasta de sessão `.agents/onion/sessions/<feature-slug>/`.

**Sessões de contexto** — Contexto de feature em `.agents/onion/sessions/<feature-slug>/` (`context.md`, `plan.md`, `architecture.md`). `/onion-engineer-start` cria; `/onion-engineer-work` consome. Verificar se a sessão existe antes de recomendar `work`.

**Formatação ClickUp vs Jira** — ClickUp: descriptions Markdown, comments Unicode visual. Jira Cloud v3: ADF (JSON). Jira Server/DC v2: wiki markup.

**Naming de skill** — `onion-<categoria>-<comando>`, catálogo flat (sem subpastas). Delegação a specialist é sempre via `spawn_agent`.

**Search Jira 2025** — `GET /rest/api/3/search` removido (mai/2025). Usar `POST /rest/api/3/search/jql` com `nextPageToken`.

---

## Referências

- Persona orquestradora completa: `.agents/onion/specialists/onion.md`
- Task Manager Abstraction: `.agents/onion/utils/task-manager/` + `docs/knowledge-base/concepts/task-manager-abstraction.md`
- Rules do projeto: `AGENTS.md`
- Índice geral: `docs/INDEX.md`
