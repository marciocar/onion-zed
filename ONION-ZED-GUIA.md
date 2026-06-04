# 🧅🦓 Sistema Onion no Zed — Guia de Uso

> Guia para carregar no Zed como contexto. Explica como o Sistema Onion funciona **nativamente no Zed** após o port (ver [ADR 0001](docs/meta-specs/adr/0001-zed-native-port.md)). Para regras operacionais o agente lê automaticamente o `AGENTS.md`.

---

## 1. O que é

O **Onion** é um framework nativo do **Zed** que orquestra o ciclo completo de desenvolvimento em três dimensões peer:

- 📋 **Produto** — descoberta, spec, backlog
- ⚙️ **Engenharia** — implementação, PR, entrega
- 🛡️ **Compliance** — governança (ISO 27001/22301, SOC2, PMBOK)

Plataforma única: **Zed**. Sem dependência do Claude Code, sem npm, sem CLI.

---

## 2. Modelo de execução (o essencial)

O Onion roda sobre **dois primitivos do Zed**:

| Primitivo | O que é | Como invocar |
|-----------|---------|--------------|
| **Skill** | Um workflow/comando (ex-comando Onion) | Digite `/onion-<categoria>-<comando>` no Agent Panel, ou `@skill` |
| **Specialist** | Uma persona especialista (ex-agente Onion) | O agente delega via a tool **`spawn_agent`** |

**Como a delegação funciona:** quando uma tarefa pede expertise profunda (Jira, code review, C4, etc.), o agente principal chama `spawn_agent` com um prompt do tipo:

> *"Leia `.agents/onion/specialists/jira-specialist.md` e atue como esse especialista para: criar a issue X."*

O subagente roda com janela de contexto própria e devolve o resultado.

**Diferenças-chave em relação ao Claude Code (não confunda):**
- Não há `@agente` como menção — delegação é sempre `spawn_agent`.
- Não há `allowed-tools` por skill — permissões são **globais** em `.zed/settings.json`.
- Não há injeção dinâmica `!cmd` em skill — o agente lê `.env` via tool.
- Catálogo de skills é **flat** — naming `onion-<cat>-<cmd>` (sem subpastas).

---

## 3. Setup inicial no Zed

1. **Abra o projeto no Zed** e **confie no worktree** (worktree trust) — sem isso, as skills locais (`.agents/skills/`) e os `context_servers` MCP **não ativam**. Ícone de exclamação na barra de título indica worktree não confiável.
2. **Configure o modelo** em `.zed/settings.json` (já há `agent.default_model` = Anthropic; ajuste se quiser) e a **chave de API** via `agent: open settings` (vai pro keychain, não pro arquivo).
3. **Verifique o `.env`** — `TASK_MANAGER_PROVIDER` define o provider de tasks (atual: `jira`).
4. **Pronto.** Abra o Agent Panel (`Ctrl+Alt+J`) e digite `/onion` para orientação ou `/onion-warmup` para carregar contexto.

---

## 4. Estrutura de diretórios

```
.agents/
├── skills/                      # 81 skills (catálogo FLAT, 1 pasta cada)
│   ├── onion/                   # 🧅 orquestrador (cérebro) — invoque /onion
│   ├── onion-warmup/            # warm-up geral
│   ├── onion-patterns/          # convenções (auto-ativa)
│   ├── onion-validation/        # regras de validação (auto-ativa)
│   ├── language-standards/      # idioma pt-BR/EN (auto-ativa)
│   └── onion-<cat>-<cmd>/       # ex-comandos (engineer, product, git, docs, meta, test, validate…)
└── onion/
    ├── specialists/             # 49 personas (alvos de spawn_agent)
    ├── utils/task-manager/      # abstração Jira/ClickUp/Asana/Linear
    ├── templates/ · prompts/    # fragmentos reutilizáveis
    └── docs/                    # docs internas do framework
.zed/settings.json               # modelos, permissões, context_servers (MCP)
AGENTS.md                        # rules (lido nativamente pelo Zed)
docs/                            # documentação do projeto
```

---

## 5. Como começar (por intenção)

Não sabe por onde começar? Invoque **`/onion`** e descreva sua intenção. Ou vá direto:

### Desenvolver uma feature
```
/onion-product-task        → cria/decompõe a task no Task Manager
/onion-engineer-start      → cria sessão de trabalho + analisa as tasks
/onion-engineer-work       → desenvolve, atualiza progresso
/onion-engineer-pre-pr     → valida (lint, testes, review) antes do PR
/onion-engineer-pr         → cria o Pull Request
```

### Discovery de produto
```
/onion-product-collect → /onion-product-refine → /onion-product-spec → /onion-product-task
```

### Hotfix urgente
```
/onion-engineer-hotfix → /onion-engineer-work → /onion-engineer-pr → /onion-git-hotfix-finish
```

### Documentação
```
/onion-docs-build-tech-docs · /onion-docs-build-business-docs · /onion-docs-build-index
```

### Criar componentes do próprio Onion
```
/onion-meta-create-skill        # nova skill
/onion-meta-create-agent        # novo specialist
/onion-meta-create-knowledge-base
/onion-meta-setup-integration   # configurar Jira/ClickUp/etc.
```

---

## 6. Skills por categoria (81 no total)

| Categoria | Prefixo | Exemplos |
|-----------|---------|----------|
| Core | — | `onion`, `onion-warmup`, `onion-patterns`, `onion-validation`, `language-standards` |
| Engenharia | `onion-engineer-` | start, work, pre-pr, pr, pr-update, plan, hotfix, bump, docs, warm-up, validate-phase-sync |
| Produto | `onion-product-` | task, spec, estimate, collect, refine, feature, extract-meeting, branding… (20) |
| Git | `onion-git-` | feature-start/finish/publish, hotfix-start/finish, release-start/finish, sync, fast-commit, init… |
| Docs | `onion-docs-` | build-tech-docs, build-business-docs, build-index, reverse-consolidate, validate-docs… |
| Meta | `onion-meta-` | create-skill, create-agent, create-command, metaspec-validate, setup-integration, all-tools… |
| Test | `onion-test-` | unit, integration, e2e |
| Validate | `onion-validate-` | workflow, collab-three-amigos, qa-points-estimate, test-strategy-create… |
| Quick / Dev | `onion-quick-`, `onion-development-` | analisys, runflow-dev |

> Lista viva de skills: `list_directory .agents/skills/`. Lista de specialists: `list_directory .agents/onion/specialists/`.

---

## 7. Specialists mais usados (delegados via `spawn_agent`)

| Specialist | Papel |
|-----------|-------|
| `onion` | orquestração (também exposto como skill) |
| `jira-specialist` | Jira REST v3/v2, JQL, ADF, transitions |
| `clickup-specialist` | ClickUp (MCP via context_server) |
| `task-specialist` | decomposição agnóstica de tasks |
| `product-agent` | gestão estratégica de produto |
| `code-reviewer` / `branch-code-reviewer` | review de código / de branch |
| `test-engineer` / `test-planner` | testes |
| `gitflow-specialist` | GitFlow |
| `metaspec-gate-keeper` | conformidade arquitetural |
| `zed-specialist` | config/troubleshooting do Zed |
| `react-developer` / `nodejs-specialist` / `postgres-specialist` | stack técnico |

---

## 8. Task Manager (provider-aware)

Antes de operar com tasks, o agente **lê `.env`** (`TASK_MANAGER_PROVIDER`) e roteia:

| Provider | Specialist | Integração no Zed |
|----------|-----------|-------------------|
| `jira` (ativo) | `jira-specialist` | **REST direto** (`terminal`/`fetch`) — não precisa MCP |
| `clickup` | `clickup-specialist` | **MCP standalone** via `context_servers` |
| `asana` / `linear` | `task-specialist` | MCP standalone ou REST (hosted-claude.ai não funciona no Zed) |
| `none` | `task-specialist` | offline, sem persistir |

Formatação: Jira → **ADF (JSON)**; ClickUp → Markdown + comments Unicode; Linear → Markdown.

---

## 9. Configuração (`.zed/settings.json`)

```jsonc
{
  "agent": {
    "default_model": { "provider": "anthropic", "model": "claude-opus-4-8" },
    "subagent_model": { "provider": "anthropic", "model": "claude-sonnet-4-5" },
    "tool_permissions": {
      "default": "confirm",
      "always_allow": ["read_file", "grep", "list_directory", "find_path"],
      "always_confirm": ["terminal", "write_file", "edit_file"],
      "always_deny": ["delete_path"]
    }
  },
  "context_servers": { /* ClickUp aqui, quando ativado */ }
}
```

Ferramentas nativas do agente: `read_file`, `write_file`, `edit_file`, `terminal`, `grep`, `find_path`, `list_directory`, `fetch`, `diagnostics`, `spawn_agent`, `search_web`.

---

## 10. Atalhos úteis do Zed

| Ação | Atalho (Linux/Win) |
|------|--------------------|
| Agent Panel | `Ctrl+Alt+J` |
| Inline Assistant | `Ctrl+Enter` |
| Paleta de comandos | `Ctrl+Shift+P` |
| Buscar arquivo | `Ctrl+P` |
| Alternar threads | `Ctrl+Tab` |

---

## Referências

- **Rules** (lido pelo agente): `AGENTS.md`
- **Constituição**: `docs/meta-specs/` (5 meta-specs L0) + `docs/meta-specs/adr/0001-zed-native-port.md`
- **KB do Zed**: `docs/knowledge-base/tools/zed.md`
- **Índice geral**: `docs/INDEX.md`
- **Convenções**: skill `onion-patterns` · **Validação**: skill `onion-validation`

---

**Dica:** carregue este arquivo e o `AGENTS.md` no início de uma thread no Zed, ou simplesmente invoque `/onion-warmup` para o agente montar o contexto sozinho.
