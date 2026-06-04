---
title: Meta-spec — Padrões de Código e Idioma do Sistema Onion (nativo Zed)
date: 2026-06-03
version: 2.0.0
level: L0
status: active
gate-keeper: "specialists/metaspec-gate-keeper.md"
---

# Meta-spec — Padrões de Código e Idioma do Sistema Onion (nativo Zed)

## Propósito

Define padrões de **idioma**, **formatação**, **convenções textuais** e **nomenclatura de artefatos Zed** aplicáveis a todos os componentes do Sistema Onion. No port nativo Zed ([ADR 0001](./adr/0001-zed-native-port.md)), consolida diretrizes que estavam em CLAUDE.md (hoje `AGENTS.md`), READMEs e mensagens informais.

Aplica-se ao **Sistema Onion**, não ao projeto-alvo onde o Onion é instalado.

Referências relacionadas:

- [agents.md](./agents.md), [commands.md](./commands.md)
- [architecture.md](./architecture.md), [integrations.md](./integrations.md)
- [adr/0001-zed-native-port.md](./adr/0001-zed-native-port.md) — régua do port

Skill que automatiza aplicação: `.agents/skills/language-standards/`.

---

## 1. Idioma — separação obrigatória

| Onde | Idioma | Exemplos |
|---|---|---|
| Comentários de código | **pt-BR** | `// calcula valor total com desconto` |
| Documentação Markdown | **pt-BR** | READMEs, KBs, meta-specs, guias, análises, ADRs |
| Mensagens ao usuário | **pt-BR** | Confirmações, prompts, erros voltados ao usuário |
| Respostas do assistente IA | **pt-BR** | Conversas no chat do Zed com o operador |
| Código (variáveis, funções, classes, módulos) | **inglês** | `calculateTotalWithDiscount()`, `class TaskAdapter` |
| Nomes de arquivos | **inglês** | `task-manager-abstraction.md`, `react-developer.md` |
| Nomes de skills e specialists | **inglês** | `onion-engineer-start`, `clickup-specialist` |
| Commits e branches | **inglês** | `feat: add jira adapter retry logic`, `feature/jira-bulk` |
| Logs e debugging | **inglês** | `Error: provider not configured` |
| Frontmatter (campos) | **inglês** | `name:`, `description:` |
| Frontmatter (valores narrativos) | pt-BR aceito | `description: "Gestão estratégica..."` |

### 1.1 Justificativa

- pt-BR em documentação e UX mantém o framework acessível ao mantenedor e leitores nativos
- Inglês em camadas técnicas garante interoperabilidade com ferramentas (git, linters, busca)
- Misturar idiomas dentro da mesma camada é proibido

### 1.2 Exceções aceitas

- Citações diretas de fontes externas podem manter idioma original
- Termos técnicos sem tradução consolidada (ex: "Pull Request", "feature flag", "commit", "skill", "specialist")
- Nomes próprios de tecnologias mantêm grafia oficial (Zed, GitHub, Jira, ClickUp)

---

## 2. Tool names nativas do Zed (snake_case)

Skills e specialists referenciam tools pelo nome **nativo Zed em snake_case** — não pelos nomes do Claude Code (`Bash`, `Read`, `Edit`, `Glob`):

| Tool Zed (snake_case) | Função |
|---|---|
| `read_file` | Ler arquivo |
| `write_file` | Criar/sobrescrever arquivo |
| `edit_file` | Edição cirúrgica |
| `terminal` | Executar comando de shell |
| `grep` | Busca por conteúdo |
| `find_path` | Busca por caminho/glob |
| `list_directory` | Listar diretório |
| `fetch` | HTTP (ex: Jira REST) |
| `diagnostics` | Diagnósticos do projeto |
| `spawn_agent` | Delegar a um specialist |
| `search_web` | Busca na web |

> **Mudou do legado:** `Bash` → `terminal`; `Read` → `read_file`; `Edit` → `edit_file`; `Write` → `write_file`; `Glob` → `find_path`; `Grep` → `grep`. Não usar `allowed-tools` em frontmatter (campo inexistente no Zed).

---

## 3. Nomenclatura de artefatos

### 3.1 Skills

- Pasta + `name`: `onion-<categoria>-<comando>` (lowercase + hífen, ≤ 64 chars)
- Core skills sem prefixo: `onion`, `onion-warmup`, `onion-patterns`, `onion-validation`, `language-standards`
- Filha **direta** de `.agents/skills/` (catálogo flat, sem subpasta)
- Invocação: `/onion-<categoria>-<comando>` ou `@<skill>`

### 3.2 Specialists

- Arquivo `.agents/onion/specialists/<slug>.md`, slug kebab-case (`jira-specialist`, `react-developer`)
- Frontmatter mínimo: `name` + `description`
- Delegação: `spawn_agent` + caminho da persona (não `@<slug>`)

### 3.3 Filenames gerais

- **kebab-case** para tudo em `.agents/` e `docs/`
- Não usar espaços, underscores ou PascalCase
- `.md` para documentos; `.json` para configs (`.zed/settings.json`); `.yml` apenas em workflows CI

### 3.4 Feature slugs (sessions e branches)

- **kebab-case obrigatório** — branch e pasta de sessão (`.agents/onion/sessions/<slug>/`) usam o mesmo slug
- Underscore quebra GitFlow: `user-authentication` ✅; `user_auth` ❌

### 3.5 Branches Git

- GitFlow: `feature/<nome>`, `hotfix/<nome>`, `release/<versao>`
- Chores: `chore/<descricao>`

### 3.6 Commits

- Conventional Commits em inglês: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`
- Mensagem em pt-BR conforme `AGENTS.md`, com tipo em inglês
- Referenciar issue/ID de task quando aplicável

---

## 4. Formatação Markdown

### 4.1 Headers e listas

- `# H1` apenas para título (um por arquivo); `## H2` seções; `### H3` subseções
- Não pular níveis (H2 → H3 → H4)
- Hífen (`-`) para listas não ordenadas; números para sequência relevante; recuo de 2 espaços

### 4.2 Tabelas, código e links

- Tabelas quando comparativo/tabular é mais legível que prosa
- Blocos de código com linguagem declarada (` ```yaml `, ` ```bash `, ` ```mermaid `)
- Links Markdown nativo `[label](path)`, sempre **paths relativos**

### 4.3 Frontmatter de docs

```yaml
---
title: <título humano>
date: <YYYY-MM-DD>
version: <semver>
status: <active | historical | draft>
---
```

---

## 5. Estilo de escrita

- **Documentação técnica**: direto, factual
- **Análises críticas / ADRs**: crítico mas construtivo
- **Mensagens ao usuário**: claro e empático
- **Comentários em código**: explicar **por quê**, não o **quê**

### 5.1 Emojis

- **Permitido** em READMEs, INDEX, skills, guias (uso moderado: 🧅, 📚, 🎯)
- **Proibido** em meta-specs, ADRs e análises críticas
- **Proibido** em código

---

## 6. Padrões de teste

Quando o Onion gerar/validar testes (em projeto-alvo):

- Manter idioma do código-base do projeto-alvo
- Estrutura AAA (Arrange-Act-Assert) ou Given-When-Then quando aplicável
- Cobertura de happy path + edge cases + erro

---

## 7. Configuração e secrets

- **Nunca** commitar credenciais, tokens, API keys
- **Secrets sempre em `.env`** (`.gitignore` sempre); `.env.example` versionado como referência
- **Permissões de tools globais em `.zed/settings.json`** (`agent.tool_permissions`) — nunca por skill/specialist
- **MCPs em `.zed/settings.json`** (`context_servers`) — usar interpolação `${VAR}` resolvida do `.env`, nunca colar tokens
- Documentar variáveis de ambiente em `.env.example` com comentários
- Quando uma skill/specialist referenciar variável, documentar em [integrations.md](./integrations.md)

---

## 8. Exemplos

### Exemplo conforme (frontmatter de skill)

```yaml
---
name: onion-engineer-start
description: >
  Inicia desenvolvimento de feature, cria sessão e analisa tasks.
  Use quando o usuário começar a implementar uma feature planejada.
---
```

- `name` = nome da pasta, naming `onion-cat-cmd`
- `description` com "use quando"; sem campos inválidos

### Exemplo não-conforme

```yaml
---
name: EngineerStart
allowed-tools: Bash(git *) Read Edit
model: sonnet
---
```

Violações:

- `name` em PascalCase (deveria ser `onion-engineer-start`)
- `allowed-tools` e `model` inexistentes no Zed (permissão é global; modelo via `agent.subagent_model`)
- Tools no formato Claude Code (`Bash`, `Read`, `Edit`) em vez de snake_case Zed

---

## 9. Versionamento e mudanças

Mudanças nesta spec exigem:

1. PR específico para `docs/meta-specs/code-standards.md`
2. Atualização do campo `version`
3. Migração de artefatos existentes ou plano de migração explícito

---

## Histórico

| Data | Versão | Mudança |
|------|--------|---------|
| 2026-05-18 | 1.0.0 | Criação (padrões de idioma/código para `.claude/`) |
| 2026-06-03 | 2.0.0 | Port nativo Zed (ADR 0001) — tool names snake_case, naming de skills/specialists Zed, secrets em `.env`, permissões globais em `.zed/settings.json` |
