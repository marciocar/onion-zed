---
name: onion-meta-create-agent
description: Cria um novo SPECIALIST (persona delegável) do Onion em .agents/onion/specialists/<slug>.md, com frontmatter mínimo Zed e tool names snake_case. Use quando precisar de um novo especialista de IA que se integra ao ecossistema nativo Zed.
disable-model-invocation: true
---

# 🤖 Criar Specialist (persona delegável)

No Zed nativo não existe registry de subagentes nomeados (`@agente`). Cada
specialist é um arquivo `.md` em `.agents/onion/specialists/`, lido e ativado
via tool `spawn_agent`. Este gerador cria specialists com frontmatter mínimo
(só `name` + `description`), tool names em snake_case do Zed e < 300 linhas.

## 🎯 Objetivo

Gerar um specialist focado, delegável via `spawn_agent`, que se integra ao
ecossistema Onion nativo Zed.

## ⚡ Fluxo de Execução

### Passo 1: Análise de Contexto

```bash
# Listar specialists existentes
list_directory .agents/onion/specialists/

# Verificar duplicação
grep -rl "^name: {{slug}}$" .agents/onion/specialists/*.md
```

### Passo 2: Definir Slug e Escopo

- **Slug**: kebab-case, ex.: `react-developer`, `jira-specialist`.
- **Escopo**: 3-5 áreas de expertise; uma responsabilidade clara.
- A "categoria" do agente original vira apenas contexto descritivo na
  `description` — **não** é campo de frontmatter no Zed.

### Passo 3: Gerar Estrutura

Base: `.agents/onion/templates/agent-template.md`. Frontmatter **mínimo**:

```yaml
---
name: {{slug}}
description: >
  [Especialização do agente em 1-2 linhas]. Delegue quando [contexto/gatilho].
---

# Você é o [Nome do Specialist]

## 🎯 Filosofia / Propósito
[Identidade e razão de existir]

## 🔧 Áreas de Especialização
1. [Área 1]
2. [Área 2]
3. [Área 3]

## 📋 Processo de Trabalho
[Workflow passo a passo. Use tools nativas Zed: read_file, write_file,
edit_file, terminal, grep, find_path, list_directory, fetch, diagnostics]

## 🔗 Delegação
[Se precisar de outro specialist, use spawn_agent lendo
.agents/onion/specialists/<outro>.md — nunca @agente]

## ⚠️ Regras
- [Regra 1]
- [Regra 2]
```

> **Frontmatter Zed: só `name` e `description`.** Campos `model`, `tools`,
> `color`, `priority`, `category`, `expertise`, `version`, `related_*`
> **não existem no Zed** e são ignorados. Permissões de tools são **globais**
> em `.zed/settings.json` (`agent.tool_permissions`), nunca por specialist.

> **Tool names em snake_case Zed**: `read_file`, `write_file`, `edit_file`,
> `terminal`, `grep`, `find_path`, `list_directory`, `fetch`, `diagnostics`,
> `spawn_agent`, `search_web`. MCPs via `context_servers`.

### Passo 4: Validações Obrigatórias

- [ ] Slug único — não existe em `.agents/onion/specialists/`
- [ ] Slug em kebab-case
- [ ] Frontmatter mínimo (só `name` + `description`)
- [ ] Sem campos Claude Code (`model:`, `tools:`, `category:`, `tags:`)
- [ ] Tool names citados em snake_case Zed
- [ ] Expertise definida (3-5 áreas)
- [ ] Delegação a outros specialists via `spawn_agent` (não `@agente`)
- [ ] < 300 linhas

Verificação automática (tool `terminal`):

```bash
# Duplicação
grep -rl "^name: {{slug}}$" .agents/onion/specialists/ 2>/dev/null \
  && echo "❌ Já existe" || echo "✅ Slug livre"

# Resíduo de Claude Code
grep -nE "model: sonnet|allowed-tools|^tools:|^category:|@[a-z-]+-specialist" \
  .agents/onion/specialists/{{slug}}.md 2>/dev/null
```

### Passo 5: Criar Arquivo

```bash
write_file .agents/onion/specialists/{{slug}}.md
```

### Passo 6: Delegação (opcional, para casos complexos)

Para specialists elaborados, delegue a geração via tool `spawn_agent`:

> "Leia `.agents/onion/specialists/agent-creator-specialist.md` e atue como
> esse especialista para criar o specialist `{{slug}}`."

Contexto a passar:

```
Specialist alvo: {{slug}}
Arquivo: .agents/onion/specialists/{{slug}}.md
Expertise (3-5 áreas): {{áreas}}
Tools necessárias (snake_case Zed): {{lista}}
Template base: .agents/onion/templates/agent-template.md
```

> Para criação rápida com setup mínimo, use a skill
> `/onion-meta-create-agent-express`.

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ SPECIALIST CRIADO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 Arquivo: .agents/onion/specialists/{{slug}}.md

📋 Detalhes:
∟ Nome: {{slug}}
∟ Expertise: [áreas]
∟ Linhas: ~N (< 300)
∟ Frontmatter: name + description

🚀 Para usar: spawn_agent → "Leia .agents/onion/specialists/{{slug}}.md
   e atue como esse especialista para: ..."
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Specialist gerador: `spawn_agent` → `.agents/onion/specialists/agent-creator-specialist.md`
- Versão rápida: `/onion-meta-create-agent-express`
- Template: `.agents/onion/templates/agent-template.md`
- Padrões e validação: skills `onion-patterns`, `onion-validation`
- ADR: `docs/meta-specs/adr/0001-zed-native-port.md`

## ⚠️ Notas

- Sem registry nomeado no Zed: delegação é sempre `spawn_agent` + arquivo de
  persona; o estado Master-Slave passa por prompt/arquivo de sessão.
- Não adicionar MCPs em specialists genéricos.
- Sempre validar duplicação e ausência de campos Claude Code antes de gravar.
