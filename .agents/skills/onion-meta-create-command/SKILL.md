---
name: onion-meta-create-command
description: Cria uma nova SKILL invocável do Onion (ex-comando) no catálogo flat .agents/skills/onion-<cat>-<cmd>/. Use quando precisar gerar um novo comando/skill que segue os padrões nativos Zed do Sistema Onion.
disable-model-invocation: true
---

# 📝 Criar Skill Invocável (ex-comando)

No Zed nativo, comandos e skills foram **mesclados**: uma skill em
`.agents/skills/<nome>/SKILL.md` é invocável por `/<nome>` e `@<nome>`. Este
gerador cria skills no naming `onion-<categoria>-<comando>` (catálogo **flat**,
sem subpastas), com frontmatter Zed e delegação a specialists via `spawn_agent`.

## 🎯 Objetivo

Gerar uma skill que se integra ao ecossistema Onion nativo Zed, com UX de slash
`/onion-<cat>-<cmd>`, frontmatter mínimo válido e referências corretas.

## ⚡ Fluxo de Execução

### Passo 1: Análise de Contexto

```bash
# Listar skills existentes
list_directory .agents/skills/

# Verificar duplicação de nome
grep -rl "^name: onion-{{category}}-{{command}}$" .agents/skills/*/SKILL.md
```

### Passo 2: Determinar Categoria

SE `{{category}}` fornecido → usar diretamente.
SENÃO → inferir do propósito:

| Propósito | Categoria |
|-----------|-----------|
| Desenvolvimento, código, build | `engineer` |
| Tasks, specs, features, backlog | `product` |
| Git, branches, PRs, GitFlow | `git` |
| Documentação | `docs` |
| Criação de skills/specialists, tooling | `meta` |
| Validação, QA, estratégia de teste | `validate` |
| Testes (unit/integration/e2e) | `test` |
| Integrações/SDKs específicos | `development` |
| Análises rápidas | `quick` |

> Categorias válidas: `engineer`, `product`, `git`, `docs`, `meta`,
> `validate`, `test`, `development`, `quick`.

### Passo 3: Gerar Estrutura

Base: `.agents/onion/templates/command-template.md`. O nome da pasta e o campo
`name` são **idênticos** e seguem `onion-<categoria>-<comando>`:

```yaml
---
name: onion-{{category}}-{{command}}
description: >
  [Verbo imperativo] [o que a skill faz]. Use quando [contexto explícito],
  mesmo que o usuário não mencione [keyword] diretamente.
disable-model-invocation: true   # opcional: true = só invocação manual (/ ou @)
---

# [Título da Skill]

[Descrição breve do propósito]

## 🎯 Objetivo
[O que esta skill faz]

## ⚡ Fluxo de Execução

### Passo 1: [Nome]
[Instruções — use tools nativas Zed: read_file, write_file, edit_file,
terminal, grep, find_path, list_directory]

### Passo 2: [Nome]
[Instruções]

## 📤 Output Esperado
[Formato de saída]

## 🔗 Referências
[Templates em .agents/onion/templates/, specialists relacionados]
```

> **Frontmatter Zed: só `name`, `description`, `disable-model-invocation`.**
> Campos como `model`, `allowed-tools`, `category`, `tags`, `version`,
> `parameters`, `related_*` **não existem no Zed** e são ignorados. Permissões
> de tools são globais em `.zed/settings.json` (`agent.tool_permissions`).

### Passo 4: Delegar ao Specialist

Para a geração de fato, delegue via tool `spawn_agent`:

> "Leia `.agents/onion/specialists/command-creator-specialist.md` e atue como
> esse especialista para criar a skill `onion-{{category}}-{{command}}`."

Contexto a passar ao specialist:

```
Skill alvo: onion-{{category}}-{{command}}
Pasta: .agents/skills/onion-{{category}}-{{command}}/SKILL.md
Categoria: {{category}}
Propósito: {{descrição livre}}
Specialists a referenciar (via spawn_agent): {{lista, se houver}}
Template base: .agents/onion/templates/command-template.md
```

### Passo 5: Validações Obrigatórias

Executar antes de gravar o arquivo:

- [ ] Nome único — não existe em `.agents/skills/`
- [ ] Nome em kebab-case `onion-<cat>-<cmd>` (≤64 chars)
- [ ] Categoria válida (lista do Passo 2)
- [ ] Pasta é filha **direta** de `.agents/skills/` (sem subpasta aninhada)
- [ ] Frontmatter só com os 3 campos Zed; `name` = nome da pasta
- [ ] `description` com verbo imperativo + "use quando"
- [ ] < 400 linhas
- [ ] Refs a outros agentes usam `spawn_agent` + persona (não `@agente`)
- [ ] Refs a outras skills usam `/onion-<cat>-<cmd>` (não `/cat/cmd`)
- [ ] Seções obrigatórias: Objetivo, Fluxo de Execução, Output

Verificação automática (tool `terminal`):

```bash
# Duplicação
grep -rl "^name: onion-{{category}}-{{command}}$" .agents/skills/ 2>/dev/null \
  && echo "❌ Já existe" || echo "✅ Nome livre"

# Skill aninhada por engano
find .agents/skills -mindepth 2 -name SKILL.md \
  | grep -v '^.agents/skills/[^/]*/SKILL.md$'
```

### Passo 6: Criar Arquivo

```bash
write_file .agents/skills/onion-{{category}}-{{command}}/SKILL.md
```

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ SKILL CRIADA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 Arquivo: .agents/skills/onion-{{category}}-{{command}}/SKILL.md

📋 Detalhes:
∟ Nome: onion-{{category}}-{{command}}
∟ Categoria: {{category}}
∟ Linhas: ~N (< 400)
∟ Frontmatter: name + description (+ disable-model-invocation)

🚀 Para usar: /onion-{{category}}-{{command}}  (ou @onion-{{category}}-{{command}})
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Specialist gerador: `spawn_agent` → `.agents/onion/specialists/command-creator-specialist.md`
- Template: `.agents/onion/templates/command-template.md`
- Padrões e validação: skills `onion-patterns`, `onion-validation`
- ADR: `docs/meta-specs/adr/0001-zed-native-port.md`

## ⚠️ Notas

- Catálogo **flat**: a categoria vai no **nome** (`onion-git-feature-start`),
  nunca em subpasta — pastas aninhadas não são descobertas pelo Zed.
- Máximo 400 linhas por skill; reaproveite prompts de `.agents/onion/prompts/`.
- Sempre validar duplicação e naming antes de gravar.
