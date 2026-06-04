---
title: Meta-spec — Padrões de Integração do Sistema Onion (nativo Zed)
date: 2026-06-03
version: 2.0.0
level: L0
status: active
gate-keeper: "specialists/metaspec-gate-keeper.md"
---

# Meta-spec — Padrões de Integração do Sistema Onion (nativo Zed)

## Propósito

Define os padrões obrigatórios para integrações com sistemas externos (Task Managers, MCPs, APIs). No port nativo Zed ([ADR 0001](./adr/0001-zed-native-port.md)), MCPs deixam de ser declarados em `.mcp.json` e passam para `context_servers` em `.zed/settings.json`. Usa **Task Manager Abstraction** como referência canônica de design de adapter — toda nova integração segue o mesmo padrão.

Aplica-se ao **Sistema Onion**, não ao projeto-alvo onde o Onion é instalado.

Referências relacionadas:

- [agents.md](./agents.md), [commands.md](./commands.md)
- [architecture.md](./architecture.md), [code-standards.md](./code-standards.md)
- [adr/0001-zed-native-port.md](./adr/0001-zed-native-port.md) — régua do port

Referência técnica: [.agents/onion/utils/task-manager/](../../.agents/onion/utils/task-manager/).

---

## 1. Task Manager Abstraction como referência canônica

A Task Manager Abstraction é o padrão **SDAAL** (Specification-Driven AI Abstraction Layer) de referência. A lógica migra de `.claude/utils/task-manager/` para `.agents/onion/utils/task-manager/` **sem reescrita**. Estrutura:

```
.agents/onion/utils/task-manager/
├── factory.md           # Instancia o adapter via TASK_MANAGER_PROVIDER
├── interface.md         # Contrato ITaskManager
├── types.md             # Tipos e DTOs
├── detector.md          # Detecção de provider (instruction-driven, sem hooks)
└── adapters/
    ├── jira.md          # Adapter Jira (REST v3, ADF)
    ├── clickup.md       # Adapter ClickUp (MCP standalone)
    ├── asana.md         # Adapter Asana (REST / fallback)
    └── linear.md        # Adapter Linear (REST/GraphQL / fallback)
```

Toda nova integração deve replicar essa estrutura.

---

## 2. Estrutura obrigatória de adapter

Para cada integração com sistema externo, em `.agents/onion/utils/<dominio>/`:

```
factory.md           # Roteamento por variável de ambiente
interface.md         # Contrato comum (operações independentes de provider)
types.md             # Tipos compartilhados
detector.md          # Detecção (opcional)
adapters/<provider>.md  # Um arquivo por provider suportado
```

### 2.1 Conteúdo de cada arquivo

**factory.md** — lê variável do `.env`, valida obrigatórias, retorna adapter ou erro descritivo, lida com `none`/ausência (fallback gracioso).

**interface.md** — lista operações que **todo provider deve suportar**, inputs/outputs neutros, sem vazar detalhes de provider.

**types.md** — DTOs comuns, enums, tipos compartilhados.

**detector.md** (opcional) — detecção quando a variável não é explícita; heurísticas. No Zed **não há hooks**, logo a detecção é **instruction-driven**: a skill orquestradora lê `.env` via `read_file`/`terminal` no início do fluxo.

**adapters/<provider>.md** — implementação concreta: variáveis necessárias, formatação requerida (ADF/Markdown/HTML/Unicode), tratamento de erros e cotas do provider.

---

## 3. Gestão de `.env` e variáveis de ambiente

### 3.1 Convenção de nomes

- Prefixo do domínio em UPPER_SNAKE_CASE: `TASK_MANAGER_PROVIDER`, `JIRA_API_TOKEN`, `CLICKUP_WORKSPACE_ID`
- Sufixo descritivo (`_TOKEN`, `_HOST`, `_ID`, `_URL`)
- Booleanos como string: `"true"` / `"false"`

### 3.2 Obrigatórias vs opcionais

Cada adapter documenta suas variáveis. Exemplo (Jira):

| Variável | Tipo | Obrigatoriedade | Default | Descrição |
|---|---|---|---|---|
| `JIRA_HOST` | URL | Obrigatória | — | URL base da instância Jira |
| `JIRA_EMAIL` | email | Obrigatória | — | Email do usuário Jira |
| `JIRA_API_TOKEN` | secret | Obrigatória | — | Token gerado em Atlassian |
| `JIRA_PROJECT_KEY` | string | Opcional | — | Filtro default |
| `JIRA_AUTH_TYPE` | enum | Opcional | `basic` | `basic` ou `bearer` |
| `JIRA_API_VERSION` | enum | Opcional | `3` | `2` (Server/DC) ou `3` (Cloud) |

### 3.3 Fallback gracioso

Quando o usuário invoca uma skill que requer integração mas a variável obrigatória está ausente:

1. **Não inventar** valor nem assumir provider alternativo
2. Reportar em pt-BR qual variável falta
3. Sugerir: `/onion-meta-setup-integration`
4. Continuar offline quando possível (`specialists/task-specialist.md` decompõe localmente sem persistir)

```
Não foi possível conectar ao Jira: variável JIRA_API_TOKEN está vazia.
Para configurar, execute: /onion-meta-setup-integration
Para operar offline, defina TASK_MANAGER_PROVIDER=none no .env.
```

### 3.4 `.env.example` versionado

- `.env.example` no root contém **todas** as variáveis com placeholder e comentários
- `.env` no `.gitignore` sempre
- **Secrets só no `.env`** — nunca no `.zed/settings.json` (use `${VAR}`)

---

## 4. MCPs via `context_servers` (`.zed/settings.json`)

> **Mudou do legado:** MCP stdio deixa de viver em `.mcp.json` (`mcpServers`) e passa para `.zed/settings.json` (`context_servers`). `enableAllProjectMcpServers`/`enabledMcpjsonServers` do Claude Code não existem no Zed.

### 4.1 Declaração

```json
{
  "context_servers": {
    "ClickUp": {
      "command": "npx",
      "args": ["-y", "<clickup-mcp-server-package>"],
      "env": {
        "CLICKUP_API_TOKEN": "${CLICKUP_API_TOKEN}",
        "CLICKUP_WORKSPACE_ID": "${CLICKUP_WORKSPACE_ID}"
      }
    }
  }
}
```

- **NUNCA** colar tokens — usar interpolação `${VAR}` resolvida do `.env`/ambiente
- `context_servers` exige **worktree confiável** (worktree trust) no Zed
- `/onion-meta-setup-integration` guia a configuração de `.env` + `context_servers`

### 4.2 Suporte de MCP por provider no Zed

| Provider | Integração no Zed | Observação |
|---|---|---|
| **Jira** | **REST direto** via `fetch`/`terminal` | Não precisa de MCP — provider atual |
| **ClickUp** | **MCP standalone** em `context_servers` | Funciona como server stdio próprio |
| **Asana** | **Não funciona** (MCP claude.ai-hosted) | Fallback: adapter REST ou `none` |
| **Linear** | **Não funciona** (MCP claude.ai-hosted) | Fallback: adapter REST/GraphQL ou `none` |

> Os conectores **claude.ai-hosted** (Asana, Linear, Atlassian) **não estão disponíveis no Zed**. Para Jira a solução é REST direto; ClickUp usa MCP standalone; Asana/Linear operam por adapter REST ou caem em modo `none` até existir MCP standalone equivalente.

---

## 5. Formatação por provider

Cada provider tem formato preferido. **Adapter é responsável por traduzir** dados internos para o formato do provider.

| Provider | Descrições de task | Comments | Estrutura |
|---|---|---|---|
| Jira Cloud (v3) | ADF (Atlassian Document Format) — JSON estruturado | ADF | Bulk via `/issue/bulk` |
| Jira Server/DC (v2) | Wiki markup ou plain text | Wiki markup | Search via `/search` (paginated) |
| ClickUp | Markdown nativo em `markdown_description` | Unicode visual em `commentText` (`━━━`, `∟`, `▶`, `◆`, `✅`) | REST + MCP standalone |
| Asana | HTML notes (subset) ou plain text | HTML | REST |
| Linear | Markdown nativo | Markdown | REST/GraphQL |

### 5.1 Roteamento de delegação por provider

| Provider | Specialist (via `spawn_agent`) |
|---|---|
| `jira` | `specialists/jira-specialist.md` (JQL, ADF, transitions, bulk; REST via `fetch`) |
| `clickup` | `specialists/clickup-specialist.md` (MCP, Unicode comments, custom fields) |
| `asana` / `linear` / `none` | `specialists/task-specialist.md` (agnóstico/offline) |

Templates de formatação por provider vivem em `.agents/onion/utils/<dominio>/adapters/<provider>.md`.

---

## 6. Bulk operations e performance

### 6.1 Bulk-first

Operação em lote (> 5 itens) usa endpoint bulk do provider:

- Jira: `POST /rest/api/3/issue/bulk` (até 50/req)
- ClickUp: endpoints bulk quando disponíveis
- Evitar loops N+1

### 6.2 Field selection

- Jira: `fields=summary,status,assignee` (reduz payload 70%+)
- Linear: query GraphQL com seleção explícita

### 6.3 Paginação

- Jira Cloud v3: `POST /rest/api/3/search/jql` com `nextPageToken` (o antigo `/search` foi removido em maio/2025)
- ClickUp: `page`
- Não iterar todas as páginas sem necessidade

---

## 7. Tratamento de erros

| Erro | Resposta esperada do adapter |
|---|---|
| Variável de ambiente ausente | Fallback gracioso (Seção 3.3) |
| Token inválido / expirado | Mensagem clara em pt-BR + sugestão de regeneração |
| Rate limit | Retry com backoff exponencial, máximo 3 tentativas |
| Recurso não encontrado | Reportar ID + provider + sugestão de verificação |
| Erro de validação do provider | Mensagem original do provider + tradução pt-BR |
| Erro de rede transitório | Retry com backoff |
| Erro inesperado | Logar e reportar, não silenciar |

Adapter nunca deve "engolir" erro; skills chamadoras propagam ao usuário com contexto.

---

## 8. Adicionar novo adapter — checklist

1. Criar `.agents/onion/utils/<dominio>/adapters/<provider>.md` (Seção 2.1)
2. Atualizar `factory.md` para reconhecer o novo provider
3. Atualizar `detector.md` se houver detecção
4. Documentar variáveis em `.env.example`
5. Atualizar `AGENTS.md` (tabela "Provider → Variáveis → Specialist → Adapter")
6. Criar specialist em `.agents/onion/specialists/<provider>-specialist.md` (opcional, recomendado)
7. Se exigir MCP standalone, adicionar `context_servers` em `.zed/settings.json`
8. Atualizar esta meta-spec (Seções 4.2 e 5)
9. Validar (delegar a `specialists/metaspec-gate-keeper.md`)

---

## 9. Proibições explícitas

- **Proibido** integração que requer credencial fora de `.env`
- **Proibido** token literal em `.zed/settings.json` (usar `${VAR}`)
- **Proibido** chamar API externa diretamente em skill sem passar pelo adapter
- **Proibido** adapter que vaza tipos específicos do provider para o nível de interface
- **Proibido** assumir provider sem ler `.env` primeiro
- **Proibido** declarar MCP em `.mcp.json` (usar `context_servers` em `.zed/settings.json`)

---

## 10. Versionamento e mudanças

Mudanças nesta spec exigem:

1. PR específico para `docs/meta-specs/integrations.md`
2. Atualização do campo `version`
3. Migração de adapters existentes quando aplicável
4. Validação (delegar a `specialists/metaspec-gate-keeper.md`)

---

## Histórico

| Data | Versão | Mudança |
|------|--------|---------|
| 2026-05-18 | 1.0.0 | Criação (integrações via `.mcp.json`, MCPs claude.ai-hosted) |
| 2026-06-03 | 2.0.0 | Port nativo Zed (ADR 0001) — MCP via `context_servers` em `.zed/settings.json`, Jira REST direto, ClickUp MCP standalone, Asana/Linear fallback REST/none, abstração em `.agents/onion/utils/task-manager/` |
