---
title: Meta-spec — Padrões para Skills do Sistema Onion (nativo Zed)
date: 2026-06-03
version: 2.0.0
level: L0
status: active
gate-keeper: "specialists/metaspec-gate-keeper.md"
---

# Meta-spec — Padrões para Skills do Sistema Onion (nativo Zed)

## Propósito

Define os padrões imutáveis (L0) que **todas as skills** em `.agents/skills/` devem seguir. No port nativo Zed ([ADR 0001](./adr/0001-zed-native-port.md)), os antigos comandos `/cat/x` (`.claude/commands/`) viram **Skills** invocáveis por `/onion-<cat>-<cmd>`. Inclui o conceito **invariante** de workflows faseados retomáveis — agora expressos como **cadeia de skills** — mecanismo que distingue o Onion de coleções de skills avulsas.

Aplica-se ao **Sistema Onion**, não ao projeto-alvo onde o Onion é instalado.

Referências relacionadas:

- [agents.md](./agents.md) — padrões para specialists
- [architecture.md](./architecture.md) — estrutura de diretórios e dependências
- [code-standards.md](./code-standards.md) — padrões de código e idioma
- [integrations.md](./integrations.md) — padrões para integrações
- [adr/0001-zed-native-port.md](./adr/0001-zed-native-port.md) — régua do port

---

## 1. Estrutura obrigatória

Toda skill vive em `.agents/skills/<nome>/SKILL.md`, sendo `<nome>` filha **direta** de `.agents/skills/` (catálogo **flat**). Pastas aninhadas **não são descobertas** pelo Zed.

### 1.1 Frontmatter (Zed aceita apenas 3 campos)

```yaml
---
name: onion-categoria-comando            # obrigatório, = nome da pasta
description: >                           # obrigatório, <1024 chars, com "use quando"
  [Verbo imperativo] [o que faz]. Use quando [contexto explícito].
disable-model-invocation: true           # opcional; true = só invocação manual (/, @)
---
```

- `name` é obrigatório e deve ser **idêntico ao nome da pasta**
- `description` é obrigatório, imperativo, com gatilho explícito ("use quando"), < 1024 chars
- `disable-model-invocation: true` (opcional) restringe a skill a invocação manual via `/` ou `@`

> Campos `model`, `allowed-tools`, `category`, `tags`, `version`, `paths` **não existem no Zed** — são ignorados. Permissões de tools são **globais** em `.zed/settings.json` (`agent.tool_permissions`), não por skill.

### 1.2 Corpo da skill

Após o frontmatter:

```markdown
# <Título descritivo da skill>

## Objetivo
<O que esta skill entrega>

## Quando usar
<Gatilhos, casos de uso típicos>

## Etapas
<Passo a passo executável usando tools nativas Zed: read_file, edit_file, terminal, grep, find_path>

## Saída esperada
<Artefatos, mudanças, output ao usuário>

## Exemplos
<Invocações reais: /onion-cat-cmd>
```

Skills curtas (< 50 linhas) podem omitir seções não aplicáveis, mas **devem manter frontmatter + título + propósito**.

### 1.3 Permissões e tools

- Não declarar permissões por skill — escopo de tools por-artefato foi **perdido** no port (ver ADR 0001). As permissões são consolidadas em `.zed/settings.json` (`agent.tool_permissions`): `default`, `always_allow`, `always_confirm`, `always_deny`.
- Tools são referenciadas no corpo pelo nome nativo Zed em snake_case: `read_file`, `write_file`, `edit_file`, `terminal`, `grep`, `find_path`, `list_directory`, `fetch`, `diagnostics`, `spawn_agent`, `search_web`.
- Detecção de provider via `.env`: usar `read_file` em `.env` ou `terminal` com `grep TASK_MANAGER_PROVIDER .env`.
- Operação técnica do provider ativo é **delegada** a um specialist via `spawn_agent` (ver `agents.md`) — a skill não enumera tools MCP.

> **Não há hooks no Zed.** A automação a nível de evento (antigo `SessionStart`) deixa de existir; a detecção de provider passa a ser **instruction-driven** dentro da skill orquestradora.

---

## 2. Categorias válidas (prefixo de naming)

A categoria vive no **nome** da skill, não em subpasta. Categorias com asterisco representam **as três dimensões peer do ciclo Onion**.

| Categoria | Função | Exemplo de skill |
|---|---|---|
| `product` (*) | Discovery, especificação, decomposição, branding, reuniões | `onion-product-task` |
| `engineer` (*) | Planejamento e implementação faseada de features | `onion-engineer-start` |
| `docs` | Geração e validação de documentação (incl. compliance) | `onion-docs-build-tech-docs` |
| `git` | GitFlow, feature/release/hotfix, code review | `onion-git-feature-start` |
| `meta` | Criação de skills/specialists/KBs, integração | `onion-meta-create-skill` |
| `validate` | Validação de testes, QA, workflows colaborativos | `onion-validate-workflow` |
| `test` | Estratégias de teste (unit, integration, e2e) | `onion-test-unit` |
| `development` | Skills de desenvolvimento específicas | `onion-development-runflow-dev` |
| `quick` | Análises pontuais rápidas | `onion-quick-analisys` |

**Core skills** (sem prefixo de categoria): `onion`, `onion-warmup`, `onion-patterns`, `onion-validation`, `language-standards`.

> Variantes de GitFlow que antes eram subpastas (`git/feature/start`) viram nome flat: `onion-git-feature-start`, `onion-git-hotfix-finish`, etc. Recursos compartilhados antes em `common/` (templates/prompts) migram para `.agents/onion/templates/` e `.agents/onion/prompts/` — **não** são skills.

---

## 3. Workflows faseados — INVARIANTE DO FRAMEWORK

**Princípio**: o Onion implementa workflows faseados como **mecanismo central**. Múltiplas skills cobrindo fases distintas de um mesmo fluxo, com estado retomável persistido em `.agents/onion/sessions/`, são **valor de design**, não duplicação. No modelo Zed, um workflow é uma **cadeia de skills** invocadas em sequência (`/onion-cat-x` → `/onion-cat-y`).

### 3.1 Workflows canônicos

Os **dois workflows abaixo são invariantes** do framework. Devem ser preservados intactos. Qualquer proposta de fusão deve ser rejeitada (delegar a `specialists/metaspec-gate-keeper.md`).

**Workflow de Engenharia** (6 fases — cadeia de skills):

```
/onion-engineer-plan → /onion-engineer-start → /onion-engineer-work → /onion-engineer-pre-pr → /onion-engineer-pr → /onion-engineer-pr-update
```

- `plan` — analisa requisitos e cria plano estruturado
- `start` — cria sessão de desenvolvimento e analisa tasks
- `work` — retoma sessão e identifica próxima fase
- `pre-pr` — valida padrões e qualidade antes do PR
- `pr` — cria Pull Request com GitFlow e sync
- `pr-update` — atualiza PR existente

**Workflow de Produto** (6 fases — cadeia de skills):

```
/onion-product-collect → /onion-product-refine → /onion-product-spec → /onion-product-task → /onion-product-estimate → /onion-product-feature
```

- `collect` — coleta ideias de features ou bugs
- `refine` — refina via perguntas de esclarecimento
- `spec` — cria especificação a partir de requisitos
- `task` — decompõe em tasks/subtasks/action items
- `estimate` — aplica framework de story points
- `feature` — cria task no gerenciador configurado

### 3.2 Regras para workflows faseados

1. Cada fase deve ter **input claro** (estado da sessão ou argumentos), **output claro** (próximo estado da sessão) e ser **invocável isoladamente** quando o estado permite
2. Estado entre fases é persistido em `.agents/onion/sessions/<feature-slug>/`
3. Fases nomeadas explicitamente, sem ambiguidade de ordem; o nome flat preserva a fase (`onion-engineer-pre-pr`)
4. Novos workflows similares devem seguir o mesmo padrão (sessões persistentes, fases nomeadas, cadeia retomável)
5. **Proibido fundir fases** de workflow ativo sem justificativa formal aprovada via PR específico para esta meta-spec

### 3.3 Padrão para identificar skill de workflow faseado

- Tem nome `onion-product-*` ou `onion-engineer-*` (dimensões do ciclo)
- Lê ou escreve estado em `.agents/onion/sessions/`
- Tem nome que sugere fase explícita (verbo temporal: `start`, `work`, `pre-pr`, `pr-update`)
- Documenta a posição no ciclo no corpo da skill

---

## 4. Convenção de naming

- **Nome da pasta = `name`**: `onion-<categoria>-<comando>`, lowercase + hífen, ≤ 64 chars
- **Path**: `.agents/skills/onion-<categoria>-<comando>/SKILL.md` (filha **direta**, sem subpasta)
- **Invocação**: `/onion-<categoria>-<comando>` ou `@<skill>` no chat do Zed
- Ex.: `/onion-engineer-start`, `/onion-product-task`, `/onion-git-feature-start`, `/onion-meta-create-skill`

> **Mudou do legado:** o slash `/cat/cmd` do Claude Code não existe no Zed. A categoria deixa de ser subpasta e passa para o nome (prefixo `onion-cat-`).

### 4.1 Política de colisão de nomes

Como o catálogo é flat e a categoria está no nome, não há colisão real entre categorias (`onion-product-estimate` ≠ `onion-validate-qa-points-estimate`). Regras:

| Situação | Regra |
|---|---|
| Mesmo verbo em categorias distintas | O prefixo de categoria desambígua: `onion-engineer-start` vs `onion-git-feature-start` |
| Variantes GitFlow | Caminho completo no nome: `onion-git-feature-finish`, `onion-git-hotfix-finish`, `onion-git-release-finish` |
| READMEs de categoria | Não são skills; vivem como docs de apoio, não em `.agents/skills/` |

`name` duplicado entre skills é **proibido** — validar com `grep -rh "^name:" .agents/skills/*/SKILL.md` (ver `onion-validation`).

---

## 5. Limites de tamanho

| Limite | Linhas | Tratamento |
|---|---|---|
| Recomendado | até 300 | OK |
| Soft warning | 300 – 500 | Considerar modularização |
| Hard limit | > 500 | Refatoração obrigatória antes de merge |

> O hard limit caiu de 800 (Claude Code) para **500** linhas/skill — skills têm lifecycle persistente em contexto no Zed.

Skills que excederem 500 linhas devem extrair partes para:

- Templates em `.agents/onion/templates/`
- Prompts em `.agents/onion/prompts/`
- Knowledge bases em `docs/knowledge-base/`
- Specialists delegáveis via `spawn_agent`

---

## 6. Modularização

Skills podem reaproveitar:

- **Templates** em `.agents/onion/templates/` (estruturas reutilizáveis)
- **Prompts** em `.agents/onion/prompts/` (instruções compartilhadas)
- **Specialists** em `.agents/onion/specialists/` (delegação via `spawn_agent`)
- **Utils** em `.agents/onion/utils/` (abstrações, ex: Task Manager)

Skill que duplica > 50 linhas de outra skill deve refatorar para template ou prompt compartilhado.

---

## 7. Exemplos de conformidade

### Exemplo conforme (skill de workflow faseado)

Pasta: `.agents/skills/onion-engineer-start/SKILL.md`

- Frontmatter Zed com `name: onion-engineer-start` + `description` com "use quando"
- Filha direta de `.agents/skills/`
- Faz parte da cadeia canônica de engenharia
- Persiste estado em `.agents/onion/sessions/`
- Nome reflete fase explícita

**Veredito**: aprovado.

### Exemplo conforme (skill atômica)

Pasta: `.agents/skills/onion-meta-setup-integration/SKILL.md`

- Frontmatter Zed válido
- Categoria válida (`meta`)
- Função atômica clara (não faz parte de workflow)
- Tamanho < 500 linhas

**Veredito**: aprovado.

### Exemplo não-conforme

Pasta hipotética: `.agents/skills/git/feature/start/SKILL.md`

- Skill **aninhada** — não descoberta pelo Zed (deveria ser `onion-git-feature-start`)
- Frontmatter com `allowed-tools` (campo inexistente no Zed)
- Referência a `@gitflow-specialist` (deveria ser `spawn_agent` + persona)

**Veredito**: rejeitado (3 violações).

---

## 8. Proibições explícitas

- **Proibido** fundir skills de workflow faseado canônico (`onion-engineer-*` ou `onion-product-*`) sem PR específico para esta meta-spec
- **Proibido** skill aninhada (subpasta em `.agents/skills/`)
- **Proibido** `name` em formato diferente de `onion-<cat>-<cmd>` (exceto core skills)
- **Proibido** campos de frontmatter inexistentes no Zed (`allowed-tools`, `model`, `category`, `tags`, `paths`)
- **Proibido** referenciar `/cat/cmd` (use `/onion-cat-cmd`) ou `@agente` como subagente nomeado (use `spawn_agent` + persona)

---

## 9. Versionamento e mudanças

Mudanças nesta spec exigem:

1. PR específico para `docs/meta-specs/commands.md`
2. Atualização do campo `version` no frontmatter
3. Validação das skills existentes (delegar a `specialists/metaspec-gate-keeper.md`)
4. Para mudança em workflows canônicos (Seção 3.1): aprovação registrada em commit com link para issue de discussão

---

## Histórico

| Data | Versão | Mudança |
|------|--------|---------|
| 2026-05-18 | 1.0.0 | Criação (padrões de comandos `.claude/commands/`) |
| 2026-06-03 | 2.0.0 | Port nativo Zed (ADR 0001) — comandos viram skills `.agents/skills/onion-<cat>-<cmd>`, frontmatter Zed, catálogo flat, limite 500 linhas |
