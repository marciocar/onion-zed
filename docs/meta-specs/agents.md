---
title: Meta-spec — Padrões para Specialists do Sistema Onion (nativo Zed)
date: 2026-06-03
version: 2.0.0
level: L0
status: active
gate-keeper: "specialists/metaspec-gate-keeper.md"
---

# Meta-spec — Padrões para Specialists do Sistema Onion (nativo Zed)

## Propósito

Define os padrões imutáveis (L0) que **todos os specialists** em `.agents/onion/specialists/` devem seguir. No port nativo Zed ([ADR 0001](./adr/0001-zed-native-port.md)), os antigos agentes `@x` (`.claude/agents/<cat>/`) viram **personas delegáveis** via a tool `spawn_agent` — **não há registry de subagentes nomeados** no Zed. Esta spec é a constituição normativa de conformidade para PRs que criam ou modificam specialists.

Aplica-se ao **Sistema Onion**, não ao projeto-alvo onde o Onion é instalado.

Referências relacionadas:

- [commands.md](./commands.md) — padrões para skills
- [architecture.md](./architecture.md) — estrutura de diretórios e dependências
- [code-standards.md](./code-standards.md) — padrões de código e idioma
- [integrations.md](./integrations.md) — padrões para integrações
- [adr/0001-zed-native-port.md](./adr/0001-zed-native-port.md) — régua do port

---

## 1. Frontmatter mínimo (Zed)

Todo specialist é um `.md` em `.agents/onion/specialists/<slug>.md` e começa com frontmatter contendo **apenas dois campos**:

```yaml
---
name: <kebab-case-slug>
description: <especialização clara + quando delegar>
---
```

### Campos obrigatórios

| Campo | Tipo | Regra |
|---|---|---|
| `name` | string | kebab-case, único entre todos os specialists, sem prefixo `@` |
| `description` | string | Uma frase descrevendo a especialização e **quando** delegar |

> **Mudou do legado:** `tools:`, `model:` (ex: `model: sonnet`), `color:`, `category:` **não existem** no modelo Zed. Permissões de tools são **globais** em `.zed/settings.json` (`agent.tool_permissions`); o modelo dos subagentes vem de `agent.subagent_model` em `.zed/settings.json`. Não há escopo de tools por specialist.

### Exemplo bem-formado

`.agents/onion/specialists/product-agent.md`:

```yaml
---
name: product-agent
description: Gestão estratégica de produto e coordenação de iniciativas. Delegue para priorização, roadmap e especificação de funcionalidades.
---
```

---

## 2. Delegação via `spawn_agent`

Não há `@agente` como subagente nomeado no Zed. A invocação de um specialist acontece pela tool `spawn_agent`, passando um prompt que aponta para o arquivo da persona:

```
spawn_agent → "Leia .agents/onion/specialists/<slug>.md e atue como esse
especialista para: <tarefa>. Contexto: <estado/arquivos relevantes>."
```

Regras:

- O subagente herda as tools permitidas globalmente e tem **janela de contexto própria**
- **Sem registry nomeado** → o estado Master-Slave é passado **via prompt ou arquivo** (ex: caminho de sessão em `.agents/onion/sessions/`), não por handle de subagente persistente
- Specialist que delega para outro specialist também usa `spawn_agent` + persona

---

## 3. Inventário de specialists (slugs)

Os specialists migram de `.claude/agents/<cat>/<slug>.md` para o catálogo flat `.agents/onion/specialists/<slug>.md`. A antiga categoria deixa de ser pasta; pode permanecer apenas como menção descritiva no corpo. Famílias de slug:

| Família | Função | Exemplos de slug |
|---|---|---|
| Desenvolvimento | Especialistas técnicos verticais | `react-developer`, `nodejs-specialist`, `postgres-specialist` |
| Produto | Discovery, especificação, decomposição | `product-agent`, `task-specialist`, `extract-meeting-specialist` |
| Compliance | Frameworks regulatórios e governança | `iso-27001-specialist`, `soc2-specialist`, `pmbok-specialist` |
| Meta | Orquestração e criação de artefatos Onion | `onion`, `metaspec-gate-keeper`, `agent-creator-specialist` |
| Git | GitFlow, code review pré-PR | `gitflow-specialist`, `branch-code-reviewer` |
| Testes | Estratégia e implementação de testes | `test-agent`, `test-engineer`, `test-planner` |
| Review | Code review pós-implementação | `code-reviewer` |
| Research | Pesquisa multi-fonte | `research-agent` |
| Deployment | Containerização e deploy | `docker-specialist` |
| Task Manager | Operação do provider ativo | `jira-specialist`, `clickup-specialist` |

> A categoria não é mais um diretório validado. O slug é a unidade canônica; deve ser único no catálogo flat.

---

## 4. Convenção de naming

- **Slug** (`name` + nome do arquivo): kebab-case sem prefixo (`product-agent`, não `@product-agent` nem `Product-Agent`)
- **Filename**: `<slug>.md` (idêntico ao `name`)
- **Path**: `.agents/onion/specialists/<slug>.md`
- **Delegação**: `spawn_agent` + caminho da persona (não `@<slug>`)
- Sufixos comuns aceitos: `-specialist`, `-agent`, `-developer`, `-engineer`, `-reviewer`, `-creator`, `-checker`

---

## 5. Limites de tamanho

| Limite | Linhas | Tratamento |
|---|---|---|
| Recomendado | até 220 | OK |
| Soft warning | 220 – 300 | Considerar modularização (extrair KBs, delegar) |
| Hard limit | > 300 | Refatoração obrigatória antes de merge |

> O hard limit caiu de 1.500 (Claude Code) para **300** linhas/specialist — foco e clareza. Personas longas devem extrair conhecimento para `docs/knowledge-base/` ou skills reutilizáveis em `.agents/skills/`.

---

## 6. Padrões de delegação

### Quando criar um specialist novo

Justificativa válida exige **pelo menos um** dos critérios:

- Conhecimento técnico específico não coberto pelos specialists existentes (linguagem, framework, padrão)
- Framework regulatório específico (ISO, SOC2, PMBOK)
- Integração com sistema externo com formatação/protocolo próprio (Jira ADF, ClickUp Unicode)
- Workflow especializado que justifica contexto próprio (review pré-PR de branch, extração de reuniões)

### Quando reusar specialist agnóstico em vez de criar

- Decomposição genérica de tarefas → `specialists/task-specialist.md`
- Análise de produto sem framework específico → `specialists/product-agent.md`
- Pesquisa multi-fonte → `specialists/research-agent.md`

### Regra para o `description`

Deve indicar **quando** delegar (gatilho), não apenas **o que** faz:

```
<Especialização>. Delegue quando <casos de uso>.
```

---

## 7. Tools e integrações no corpo

Como não há campo `tools:`, dependências são descritas **no corpo** do specialist, sob seção "Dependências", usando nomes de tools nativas Zed em snake_case:

`read_file`, `write_file`, `edit_file`, `terminal`, `grep`, `find_path`, `list_directory`, `fetch`, `diagnostics`, `spawn_agent`, `search_web`.

Para integração com Task Manager:

- Specialist do provider documenta o protocolo (Jira REST via `fetch`; ClickUp via MCP `context_server`)
- Variáveis lidas de `.env` via `read_file` / `terminal`
- Detalhes em [integrations.md](./integrations.md)

---

## 8. Estrutura do corpo do arquivo

Após o frontmatter, estrutura mínima recomendada (mantendo < 300 linhas):

```markdown
# <Nome do Specialist>

## Propósito
<O que faz e por que existe>

## Quando delegar
<Gatilhos concretos>

## Quando NÃO delegar
<Limites de escopo, specialists alternativos>

## Workflow
<Passos que executa quando recebe a delegação>

## Dependências
<KBs, tools Zed, outros specialists (via spawn_agent), utils>
```

---

## 9. Exemplos de conformidade

### Exemplo conforme

`.agents/onion/specialists/product-agent.md`

- Frontmatter Zed (apenas `name` + `description`)
- Slug kebab-case único
- `description` orientada a "quando delegar"
- < 300 linhas; tool names em snake_case no corpo

**Veredito**: aprovado.

### Exemplo não-conforme

`.agents/onion/specialists/MyAgent.md`

- Filename PascalCase (deveria ser `my-agent.md`)
- Frontmatter com `tools:` e `model: sonnet` (campos inexistentes no Zed)
- Refere `@code-reviewer` (deveria ser `spawn_agent` + `specialists/code-reviewer.md`)

**Veredito**: rejeitado (3 violações).

---

## 10. Proibições explícitas

- **Proibido** campos de frontmatter fora de `name`/`description` (`tools`, `model`, `category`, `color`)
- **Proibido** referenciar specialist como `@slug` (subagente nomeado não existe no Zed) — usar `spawn_agent` + persona
- **Proibido** `name`/filename fora de kebab-case
- **Proibido** specialist acima de 300 linhas sem plano de refatoração

---

## 11. Versionamento e mudanças

Mudanças nesta spec exigem:

1. PR específico para `docs/meta-specs/agents.md`
2. Atualização do campo `version`
3. Validação de que specialists existentes ainda passam (delegar a `specialists/metaspec-gate-keeper.md`) ou plano de migração explícito
4. Não pode ser feita em PR que toca specialists — separação para evitar mudança normativa "no atacado"

---

## Histórico

| Data | Versão | Mudança |
|------|--------|---------|
| 2026-05-18 | 1.0.0 | Criação (padrões de agentes `.claude/agents/`) |
| 2026-06-03 | 2.0.0 | Port nativo Zed (ADR 0001) — agentes viram specialists `.agents/onion/specialists/`, delegação via `spawn_agent`, frontmatter mínimo (name/description), tool names snake_case, limite 300 linhas |
