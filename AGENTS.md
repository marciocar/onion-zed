# 🧅 Sistema Onion — Rules (nativo Zed)

> Arquivo de regras lido nativamente pelo Zed. Substitui o antigo `CLAUDE.md`. Ver [ADR 0001](docs/meta-specs/adr/0001-zed-native-port.md).

## 🎯 Contexto do Projeto

Este é o **Sistema Onion** — um **framework nativo do Zed** (em `.agents/` + `.zed/`) projetado para ser instalado em qualquer projeto (novo, legado ou regulado) e orquestrar o ciclo completo de desenvolvimento.

**Identidade canônica:**

- Framework nativo Zed — **não é produto npm**, não tem CLI standalone, **não depende de Claude Code**
- Plataforma única: **Zed**
- Cobre **três dimensões peer**: produto, engenharia, compliance/governança
- **Workflows faseados retomáveis** — `product/collect→feature` e `engineer/plan→pr-update` são invariantes
- Componentes nativos Zed: **Skills** (`.agents/skills/`), **Specialists** (`.agents/onion/specialists/`, via `spawn_agent`), **Rules** (este arquivo), `context_servers` e `agent.tool_permissions` (em `.zed/settings.json`)

**Inventário:**

- Skills invocáveis (`/onion-<categoria>-<comando>`) cobrindo product, git, engineer, docs, meta, validate, test, development, quick + core skills (`onion`, `onion-warmup`, `onion-patterns`, `onion-validation`, `language-standards`)
- Specialists delegáveis via `spawn_agent` em `.agents/onion/specialists/`
- **Task Manager Abstraction** plugável (Jira, ClickUp, Asana, Linear) em `.agents/onion/utils/task-manager/`

---

## 🤖 Modelo de execução nativo Zed

- **Comandos** são **Skills** invocáveis por `/onion-<categoria>-<comando>` ou `@skill`.
- **Agentes especialistas** são **personas** em `.agents/onion/specialists/<x>.md`. Para delegar, use a tool `spawn_agent` com um prompt do tipo: *"Leia `.agents/onion/specialists/<x>.md` e atue como esse especialista para: <tarefa>"*. O subagente herda as mesmas tools e tem janela de contexto própria.
- **Tools** seguem a nomenclatura nativa do Zed: `read_file`, `write_file`, `edit_file`, `terminal`, `grep`, `find_path`, `list_directory`, `fetch`, `diagnostics`, `spawn_agent`, `search_web`.
- **Permissões** são globais em `.zed/settings.json` (`agent.tool_permissions`), não por skill.

---

## 🔌 Task Manager — Detecção e Roteamento

O Onion é **provider-agnóstico**. Como o Zed **não tem hooks**, a detecção é **instruction-driven**.

### Fluxo obrigatório antes de operar com tasks

1. **Ler** `.env` com a tool `read_file` (ou `terminal`: `grep TASK_MANAGER_PROVIDER .env`)
2. **Identificar** `TASK_MANAGER_PROVIDER` → `jira` | `clickup` | `asana` | `linear` | `none`
3. **Conferir** as variáveis obrigatórias do provider ativo (tabela abaixo)
4. **Delegar** ao specialist correto via `spawn_agent` e usar a formatação adequada

### Mapa Provider → Variáveis → Specialist → Adapter

| Provider | Variáveis obrigatórias | Variáveis opcionais | Specialist | Adapter doc |
|----------|------------------------|---------------------|------------|-------------|
| **`jira`** | `JIRA_HOST`, `JIRA_EMAIL`, `JIRA_API_TOKEN` | `JIRA_PROJECT_KEY`, `JIRA_AUTH_TYPE`, `JIRA_API_VERSION` | `specialists/jira-specialist.md` | `.agents/onion/utils/task-manager/adapters/jira.md` |
| **`clickup`** | `CLICKUP_API_TOKEN` | `CLICKUP_WORKSPACE_ID`, `CLICKUP_DEFAULT_LIST_ID` | `specialists/clickup-specialist.md` | `.agents/onion/utils/task-manager/adapters/clickup.md` |
| **`asana`** | `ASANA_ACCESS_TOKEN` | `ASANA_WORKSPACE_ID`, `ASANA_DEFAULT_PROJECT_ID` | `specialists/task-specialist.md` | `.agents/onion/utils/task-manager/adapters/asana.md` |
| **`linear`** | `LINEAR_API_KEY` | `LINEAR_TEAM_ID` | `specialists/task-specialist.md` | `.agents/onion/utils/task-manager/adapters/linear.md` |
| **`none`** | — | — | `specialists/task-specialist.md` (offline) | — |

> **Integração por provider no Zed:** Jira usa **REST via `fetch`/`terminal`** (não precisa de MCP). ClickUp usa **MCP standalone** declarado em `.zed/settings.json` (`context_servers`). Asana/Linear, hoje hosted em claude.ai, exigem MCP standalone ou adapter REST — se indisponível, operar em modo `none`.

### Regras de delegação

- **Estratégia / gestão / priorização** → `specialists/product-agent.md`
- **Decomposição hierárquica de tasks** (agnóstico) → `specialists/task-specialist.md`
- **Operação técnica do provider ativo** → specialist do provider (jira/clickup)
- **Sem provider** (`none`) → operar offline com `specialists/task-specialist.md`, sem API calls

### Fallback gracioso

Se variáveis obrigatórias faltarem: (1) avisar em pt-BR qual variável falta; (2) sugerir `/onion-meta-setup-integration`; (3) **não inventar** valores nem assumir outro provider.

---

## 📝 Diretrizes de Linguagem

- **Comentários e documentação**: português brasileiro (pt-BR)
- **Código, variáveis, funções, nomes de skills/specialists, branches**: inglês
- **Commits**: português brasileiro
- **Logs e debugging**: inglês

---

## 🛠️ Padrões Técnicos

### Estrutura de Arquivos
- Skills: `.agents/skills/onion-<categoria>-<comando>/SKILL.md` (catálogo **flat**)
- Specialists: `.agents/onion/specialists/<slug>.md`
- Utils/abstrações: `.agents/onion/utils/`
- Templates/prompts: `.agents/onion/templates/`, `.agents/onion/prompts/`
- Config: `.zed/settings.json`
- **Spec as Code**: Meta Specs `docs/meta-specs/`; Business `docs/business-context/`; Technical `docs/technical-context/`; KBs `docs/knowledge-base/`

### Padrões de Código
- Siga convenções da linguagem/framework, priorize legibilidade
- Use type hints quando disponível; documente funções complexas

### Specialists mais usados
- `specialists/onion.md` — orquestração (também exposto como skill `onion`)
- `specialists/product-agent.md` — gestão estratégica de produto
- `specialists/task-specialist.md` — decomposição agnóstica
- `specialists/jira-specialist.md` / `clickup-specialist.md` — operação do provider
- `specialists/code-reviewer.md` / `test-engineer.md` — qualidade
- `specialists/metaspec-gate-keeper.md` — conformidade arquitetural

---

## 🎨 Formatação por Provider

### Jira Cloud (`jira` + REST v3)
- Descrições e comments em **ADF (Atlassian Document Format)** — JSON estruturado
- Workflow-aware: nunca setar `status` direto — usar `POST /issue/{key}/transitions`
- Bulk: `POST /rest/api/3/issue/bulk` (até 50/req); Search: `POST /rest/api/3/search/jql` com `nextPageToken`

### Jira Server/DC (`JIRA_API_VERSION=2`)
- Descrições em wiki markup ou plain text; Search via `GET /rest/api/2/search`

### ClickUp (`clickup`)
- **Descriptions** (`markdown_description`): Markdown nativo
- **Comments**: formatação visual Unicode (`━━━`, `∟`, `▶`, `◆`, `✅`) + timestamp + status obrigatórios

### Asana / Linear
- Asana: HTML notes (subset) ou plain text; Linear: Markdown nativo
- Consulte o adapter correspondente em `.agents/onion/utils/task-manager/adapters/`

---

## 🔗 Princípios de Integração

- **Sincronização contínua**: o provider sempre reflete o estado real do trabalho
- **Tags/labels** apropriadas (`bug`, `feature`, `tech-debt`)
- **Atualização de progresso em tempo real** via comments/transitions
- **Bulk-first** em lote (evita N+1)
- **Field selection** ao buscar issues (`fields=summary,status,assignee`) — reduz payload 70%+

---

## 🧪 Testes e Qualidade
- Inclua testes para funcionalidades críticas
- Valide mudanças arquiteturais delegando a `specialists/metaspec-gate-keeper.md`
- Mantenha cobertura adequada

---

Lembre-se: o Onion é sobre **eficiência**, **qualidade** e **automação inteligente**. Sempre **leia `.env` e detecte o provider ativo** antes de operar com tasks — a mesma skill funciona em Jira, ClickUp, Asana ou Linear quando o roteamento respeita o `.env`.
