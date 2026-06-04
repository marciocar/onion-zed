---
name: onion-patterns
description: >
  Padrões de nomenclatura, estrutura e convenções do Sistema Onion (nativo Zed).
  Use ao criar ou editar skills, specialists, sessions ou qualquer artefato em
  `.agents/`. Cobre estrutura de diretórios, naming de skills (flat catalog),
  kebab-case slugs, frontmatter Zed, limites de linhas, formatação de comments
  ClickUp e fluxos principais. Ative mesmo sem o usuário mencionar "padrão".
---

## Estrutura de Diretórios (nativo Zed)

### `.agents/skills/` — catálogo FLAT (sem subpastas)
```
.agents/skills/
├── onion/                      # orquestrador (cérebro)
├── onion-warmup/
├── onion-patterns/
├── onion-validation/
├── language-standards/
└── onion-<categoria>-<comando>/  # ex-comandos, 1 pasta por skill
```
> **Regra dura:** skills são filhas diretas de `.agents/skills/`. Pastas aninhadas **não são descobertas** pelo Zed. A categoria vai no **nome** (`onion-git-feature-start`), não em subpasta.

### `.agents/onion/specialists/` — personas delegáveis
Cada specialist é um `.md` (ex-agente). Delegado via tool `spawn_agent`. Sem registry nomeado.

### `.agents/onion/` — recursos compartilhados
- `utils/` — abstrações (task-manager, etc.)
- `templates/`, `prompts/` — fragmentos reutilizáveis
- `sessions/<feature-slug>/` — contexto de feature (`context.md`, `architecture.md`, `plan.md`, `notes.md`)

## Nomenclatura

### Feature slugs — kebab-case obrigatório
```
✅ user-authentication   ✅ payment-integration
❌ UserAuth   ❌ payment_integration   ❌ feature123
```

### Skills (ex-comandos)
- Pasta + `name`: `onion-<categoria>-<comando>` (lowercase+hífen, ≤64 chars)
- Categorias: `engineer`, `product`, `git`, `docs`, `meta`, `validate`, `test`, `development`, `quick`
- Invocação: `/onion-<categoria>-<comando>` ou `@<skill>`
- Ex: `/onion-engineer-start`, `/onion-product-task`, `/onion-git-feature-start`, `/onion-meta-create-skill`

### Specialists (ex-agentes)
- Arquivo: `.agents/onion/specialists/<slug>.md` em kebab-case
- Delegação: `spawn_agent` → *"Leia `.agents/onion/specialists/<slug>.md` e atue como esse especialista para: …"*
- Ex: `jira-specialist.md`, `react-developer.md`, `onion.md`

## Frontmatter (Zed aceita apenas 3 campos)

### Skill (`.agents/skills/<nome>/SKILL.md`)
```yaml
---
name: onion-categoria-comando            # obrigatório, = nome da pasta
description: >                           # obrigatório, <1024 chars, com "use quando"
  [Verbo imperativo] [o que faz]. Use quando [contexto explícito].
disable-model-invocation: true           # opcional; true = só invocação manual (/, @)
---
```
> Campos `model`, `allowed-tools`, `category`, `tags`, `version`, `paths` **não existem no Zed** — são ignorados. Permissões de tools são globais em `.zed/settings.json`.

### Specialist (`.agents/onion/specialists/<slug>.md`)
```yaml
---
name: nome-specialist
description: Descrição da especialização e quando delegar
---
```

## Limites e Métricas

| Métrica | Limite | Razão |
|---------|--------|-------|
| Skill (SKILL.md) | < 500 linhas | Lifecycle persistente em contexto |
| Specialist | < 300 linhas | Foco e clareza |
| Description em skill | < 1024 chars | Trigger budget do Zed |

## Formatação por Provider (ClickUp)

Quando `TASK_MANAGER_PROVIDER=clickup`, comments seguem padrão visual Unicode:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ FASE N CONCLUÍDA — Nome da Fase
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 YYYY-MM-DD | Status: DONE
∟ Item: valor
🚀 Próxima: Fase N+1
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
- **Subtask**: comentário detalhado; **Task principal**: resumido. Sempre timestamp + status.
- Para Jira: **ADF** (JSON estruturado), não Markdown nem Unicode.

## Fluxos Principais

```
Feature: /onion-product-task → /onion-engineer-start <slug> → /onion-engineer-work → /onion-engineer-pre-pr → /onion-engineer-pr
Hotfix:  /onion-engineer-hotfix → /onion-engineer-work → /onion-engineer-pr → /onion-git-hotfix-finish
Criação: /onion-meta-create-agent | /onion-meta-create-skill | /onion-meta-create-knowledge-base
```

## Gotchas

- **Subpasta em `.agents/skills/`**: não é descoberta — use naming flat `onion-cat-cmd`.
- **Feature slug com underscore quebra GitFlow**: branch e pasta de sessão usam o mesmo slug — kebab-case obrigatório.
- **Frontmatter com campos extras**: ignorados pelo Zed; não confie em `allowed-tools` por skill (permissão é global).
- **Delegação**: sempre `spawn_agent` + arquivo de persona — não existe `@agente` como subagente nomeado no Zed.

## Referências

- ADR: `docs/meta-specs/adr/0001-zed-native-port.md`
- KB: `docs/knowledge-base/concepts/task-manager-abstraction.md`
- Templates: `.agents/onion/templates/`
- Skills relacionadas: `language-standards`, `onion-validation`
- Validação de conformidade: `spawn_agent` → `specialists/metaspec-gate-keeper.md`
