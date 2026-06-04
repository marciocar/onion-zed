# Meta Specs — Sistema Onion (nativo Zed)

---

## 📋 Visão Geral

**Meta Specs** são especificações de nível mais alto que servem como "constituição" do Sistema Onion. Elas definem princípios, padrões e regras imutáveis que todos os componentes nativos do Zed (skills e specialists) devem seguir.

> Desde **2026-06-03** o framework é **nativo e exclusivo do Zed** — ver [ADR 0001 — Port Nativo do Onion para Zed](./adr/0001-zed-native-port.md). As 5 meta-specs foram reescritas (v2.0.0) sobre primitivos Zed: Skills (`.agents/skills/`), Specialists (`.agents/onion/specialists/`, via `spawn_agent`), Rules (`AGENTS.md`), `context_servers` e `agent.tool_permissions` (`.zed/settings.json`).

### Hierarquia de Especificações

```
┌─────────────────────────────────────────────────────────┐
│                    META-SPECS (L0)                      │
│   "Constituição" — Regras Imutáveis (framework Zed)     │
├─────────────────────────────────────────────────────────┤
│                    DOMAIN SPECS (L1)                    │
│          Regras de Negócio e Domínio (projeto-alvo)     │
├─────────────────────────────────────────────────────────┤
│                    FEATURE SPECS (L2)                   │
│          Especificações de Features (projeto-alvo)      │
├─────────────────────────────────────────────────────────┤
│                    TASK SPECS (L3)                      │
│   Sessions e Contextos (.agents/onion/sessions/)        │
└─────────────────────────────────────────────────────────┘
```

---

## 📁 Estrutura

```
docs/meta-specs/
├── index.md              # Este arquivo
├── architecture.md       # Estrutura .agents/ + .zed/ + AGENTS.md; plataforma Zed
├── code-standards.md     # Idioma; tool names snake_case; naming Zed; secrets/.env
├── agents.md             # Specialists (.agents/onion/specialists/) + spawn_agent
├── commands.md           # Skills (.agents/skills/onion-cat-cmd) + workflows faseados
├── integrations.md       # context_servers; Task Manager Abstraction; formatação por provider
└── adr/
    └── 0001-zed-native-port.md   # Decisão e régua do port
```

---

## 🎯 Propósito

### O que são Meta Specs?

Meta Specs definem:

- **Princípios arquiteturais** que o framework Zed deve seguir
- **Padrões de skills e specialists** para consistência
- **Convenções de nomenclatura** (`onion-cat-cmd`, slugs de specialist)
- **Regras de integração** via `context_servers` e adapters
- **Critérios de qualidade** para validação

### Quando usar Meta Specs?

| Situação | Consultar |
|----------|-----------|
| Criar/editar specialist | `agents.md` |
| Criar/editar skill | `commands.md` |
| Decisão arquitetural / estrutura de diretórios | `architecture.md` |
| Idioma, tool names, naming, secrets | `code-standards.md` |
| Integrar sistema externo / Task Manager | `integrations.md` |
| Entender o port para Zed | `adr/0001-zed-native-port.md` |

### Quem mantém Meta Specs?

- **`specialists/metaspec-gate-keeper.md`** (via `spawn_agent`) — valida conformidade; a constituição de validação
- **`/onion-meta-metaspec-validate`** — skill que **aplica** a constituição, executa as leituras e produz veredito com evidência (ponto de entrada confiável)
- **Skill `onion`** — orquestra a aplicação
- **Skills `onion-validation` / `onion-patterns`** — régua operacional de naming/validação Zed
- **Administradores do projeto** — atualizam specs

### Dualidade de contexto — L0 (framework Zed) vs L1+ (projeto-alvo)

O gate-keeper opera em **dois modos**, escolhendo a régua conforme o artefato:

- **Modo Framework (L0)** — no repositório do Onion (nativo Zed), valida artefatos `.agents/**` e `.zed/**` contra as **5 meta-specs L0** (agents/commands/architecture/code-standards/integrations).
- **Modo Projeto-alvo (L1+)** — quando o Onion está instalado num projeto, valida artefatos de **domínio/feature/ADR** contra as metaspecs **daquele projeto**.

Em ambos os modos as metaspecs são **descobertas dinamicamente** (`docs/meta-specs/`, sem nomes cravados), para o mesmo gate-keeper funcionar em qualquer projeto-alvo.

---

## 📜 Meta Specs Disponíveis

> As 5 meta-specs foram criadas em 2026-05-18 (v1.0.0) e **reescritas para o modelo nativo Zed** em 2026-06-03 (v2.0.0), conforme [ADR 0001](./adr/0001-zed-native-port.md).

### 🏗️ [architecture.md](./architecture.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões arquiteturais:
- Estrutura obrigatória de `.agents/` + `.zed/` + `AGENTS.md` + `docs/`
- Separação **skills** (operacional) vs **specialists** (delegação) vs **docs** (informacional)
- Princípio de framework instalável via `.agents/`
- Dependências permitidas entre categorias (com diagrama)
- **Plataforma única: Zed**; sem hooks; permissões globais

### 🔧 [commands.md](./commands.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões para **skills** (`.agents/skills/onion-<cat>-<cmd>/SKILL.md`):
- Catálogo **flat**, frontmatter Zed (`name`/`description`/`disable-model-invocation`)
- Slash UX `/onion-cat-cmd`
- **Workflows faseados como cadeia de skills** (`product/collect→feature`, `engineer/plan→pr-update`) — invariantes
- Limite < 500 linhas/skill

### 🤖 [agents.md](./agents.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões para **specialists** (`.agents/onion/specialists/<slug>.md`):
- Delegação via `spawn_agent` (sem registry nomeado)
- Frontmatter mínimo (`name`/`description`)
- Tool names snake_case Zed
- Limite < 300 linhas

### 📝 [code-standards.md](./code-standards.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões de código e idioma:
- pt-BR (docs/UX) vs inglês (código/commits/logs/nomes)
- Tool names Zed snake_case (`read_file`/`write_file`/`edit_file`/`terminal`/`grep`/`find_path`)
- Naming de skills (`onion-cat-cmd`) e specialists (slug)
- Secrets em `.env`; permissões globais em `.zed/settings.json`

### 🔌 [integrations.md](./integrations.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões para integrações:
- MCPs via `context_servers` (`.zed/settings.json`) em vez de `.mcp.json`
- Jira via REST direto; ClickUp via MCP standalone; Asana/Linear claude.ai-hosted **não funcionam no Zed** (fallback REST/none)
- Task Manager Abstraction em `.agents/onion/utils/task-manager/` como referência canônica
- Formatação por provider (ADF / Markdown / Unicode)

---

## 🔄 Workflow de Validação

```
┌─────────────────┐     ┌──────────────────────┐     ┌─────────────────┐
│    CHANGE       │────▶│  spawn_agent →       │────▶│   APPROVED/     │
│    REQUEST      │     │  metaspec-gate-keeper│     │   REJECTED      │
└─────────────────┘     └──────────────────────┘     └─────────────────┘
```

### Processo

1. **Proposta de mudança**: desenvolvedor propõe alteração em `.agents/` ou `.zed/`
2. **Validação**: `spawn_agent` → `specialists/metaspec-gate-keeper.md` verifica conformidade (ou `/onion-meta-metaspec-validate`)
3. **Decisão**: aprovado se conforme; rejeitado com justificativa e evidência citada

---

## 📚 Referências

- **ADR do port**: [adr/0001-zed-native-port.md](./adr/0001-zed-native-port.md)
- **Knowledge Bases**: `docs/knowledge-base/`
- **Skills**: `.agents/skills/`
- **Specialists**: `.agents/onion/specialists/`
- **Rules**: `AGENTS.md`
- **Config**: `.zed/settings.json`

---

## 📅 Histórico

| Data | Versão | Mudança |
|------|--------|---------|
| 2025-11-24 | 1.0.0 | Criação inicial |
| 2026-05-18 | 1.1.0 | 5 meta-specs L0 (modelo Claude Code, `.claude/`) |
| 2026-06-03 | 2.0.0 | Port nativo Zed (ADR 0001) — todas as meta-specs reescritas sobre primitivos Zed |

---

**Responsável**: Sistema Onion (nativo Zed)
**Última Atualização**: 2026-06-03
