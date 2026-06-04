---
name: onion-meta-create-skill
description: Orquestrador para criar, validar, otimizar e migrar Skills nativas do Zed no catálogo flat .agents/skills/. Use quando precisar gerar uma nova skill, validar/otimizar uma existente ou migrar artefato legacy do Claude Code.
disable-model-invocation: true
---

# 🧩 Skills Zed — Criar / Validar / Otimizar / Migrar

Orquestrador para o specialist de skills. Detecta o modo de operação, coleta
contexto e delega via `spawn_agent`.

## 🎯 Objetivo

Guiar criação e manutenção de **Skills nativas do Zed** — pastas com `SKILL.md`
no catálogo **flat** `.agents/skills/`. Cada `.agents/skills/<nome>/SKILL.md` é
invocável por `/<nome>` e `@<nome>`.

**Lembrete crítico (nativo Zed):**
- Catálogo **flat**: skills são filhas **diretas** de `.agents/skills/`. Pastas
  aninhadas **não são descobertas**. A categoria vai no **nome**
  (`onion-<cat>-<cmd>`), nunca em subpasta.
- Frontmatter Zed aceita **só 3 campos**: `name`, `description`,
  `disable-model-invocation`. Campos `model`, `allowed-tools`, `paths`,
  `category`, `tags`, `version`, `context` **não existem** e são ignorados.
- **Sem injeção dinâmica de contexto**: não há `` !`cmd` ``. Dados live são
  obtidos por **tool calls** (`terminal`, `read_file`, `grep`, etc.) descritos
  como passos na skill.
- Permissões de tools são **globais** em `.zed/settings.json`, não por skill.

## ⚡ Fluxo de Execução

### Passo 1: Detectar Modo

| Argumento | Modo | O que faz |
|-----------|------|-----------|
| `create` (default) | **Criar** | Nova skill em `.agents/skills/<nome>/SKILL.md` |
| `validate` | **Validar** | Checar skill existente (frontmatter Zed, naming flat, linhas) |
| `optimize` | **Otimizar** | Melhorar a `description` (trigger accuracy) |
| `migrate` | **Migrar** | Converter artefato legacy Claude Code em skill Zed |
| `eval` | **Avaliar** | Definir test cases de qualidade da skill |

Defaults inteligentes:
- Sem modo + path para `SKILL.md` existente → perguntar: validate/optimize/eval?
- Sem modo + path para artefato legacy `.claude/` → sugerir `migrate`.
- Sem modo + descrição livre → `create`.

### Passo 2: Coletar Inputs

**Modo `create`** — perguntar se não fornecidos:
1. **Nome** (kebab-case; ex-comandos seguem `onion-<cat>-<cmd>`).
2. **Expertise/domínio**: que erro sistemático ou conhecimento não-óbvio a
   skill encapsula?
3. **Artefatos de referência**: runbooks, schemas, histórico de PRs, API specs?
4. **Scripts necessários?** (Python/Bash) — vão em `scripts/`.
5. **`disable-model-invocation`?** `true` para skills explícitas/destrutivas
   (só `/` ou `@`); `false`/ausente para ativação semântica automática.
6. **Dados live necessários?** → descrever como **passos de tool call**
   (`terminal`/`grep`/`read_file`), nunca injeção dinâmica.

**Modo `validate`** — coletar path e checar: frontmatter (3 campos), naming
flat (filha direta de `.agents/skills/`), < 500 linhas, qualidade da
`description`, ausência de campos/refs Claude Code.

**Modo `optimize`** — coletar path + exemplos de queries que **devem** e que
**não devem** ativar a skill, para iterar a `description`.

**Modo `migrate`** — coletar path do artefato legacy `.claude/` + decisão sobre
`disable-model-invocation` (comando explícito → `true`; ativação semântica →
omitir).

**Modo `eval`** — coletar path + 2-3 tasks reais como ponto de partida.

### Passo 3: Verificações Rápidas (tool `terminal`)

```bash
# Catálogo flat
list_directory .agents/skills/ 2>/dev/null || echo "Pasta .agents/skills/ ainda não existe"

# Duplicação de nome
grep -rl "^name: {{skill_name}}$" .agents/skills/*/SKILL.md 2>/dev/null

# Skills aninhadas por engano (erro — não descobertas)
find .agents/skills -mindepth 2 -name SKILL.md \
  | grep -v '^.agents/skills/[^/]*/SKILL.md$'
```

### Passo 4: Delegar ao Specialist

Delegue via tool `spawn_agent`:

> "Leia `.agents/onion/specialists/agent-skills-specialist.md` e atue como esse
> especialista..."

Contexto estruturado:

```
Modo: {{action}}
Skill: {{skill_name}}
Localização alvo: .agents/skills/{{skill_name}}/SKILL.md (catálogo flat)
KB de referência: docs/knowledge-base/tools/agent-skills.md

Contexto do domínio:
{{contexto coletado}}

Artefatos disponíveis:
{{runbooks, schemas, exemplos}}

Frontmatter Zed (3 campos): name, description, disable-model-invocation
```

O specialist vai:
- **create**: gerar `SKILL.md` + pasta, com frontmatter Zed válido
- **validate**: checar frontmatter, naming flat, linhas, qualidade da description
- **optimize**: iterar a `description` (train/validation com queries de exemplo)
- **migrate**: converter artefato Claude Code em skill Zed, preservando lógica
- **eval**: estruturar test cases de qualidade

### Passo 5: Confirmação Pós-Operação (tool `terminal`)

```bash
find .agents/skills/{{skill_name}} -type f | sort
wc -l .agents/skills/{{skill_name}}/SKILL.md   # < 500
read_file .agents/skills/{{skill_name}}/SKILL.md   # conferir frontmatter (3 campos)
```

## 🗺️ Estrutura Esperada

```
.agents/skills/{{skill_name}}/      # filha DIRETA de .agents/skills/
├── SKILL.md              # < 500 linhas
├── scripts/              # *.py / *.sh auxiliares (opcional)
├── references/           # carregados sob demanda (opcional)
└── examples/             # exemplos de output (opcional)
```

### SKILL.md mínimo (Zed)

```markdown
---
name: {{skill_name}}
description: >
  [O que faz — verbos de ação]. Use quando [contexto explícito], mesmo que o
  usuário não mencione [keyword] diretamente.
disable-model-invocation: true   # opcional
---

## Instruções
[Passo a passo concreto, com tool calls nativas Zed]

## Gotchas
- [Erros sistemáticos do domínio]
```

### Dados live via tool call (substitui injeção dinâmica)

```markdown
## Mudanças atuais
Execute via tool `terminal`: `git diff HEAD` e resuma o output abaixo.

## Instruções
Resuma as mudanças em 2-3 bullets e liste riscos.
```

## 📤 Output Esperado

```
✅ SKILL {{action}}
━━━━━━━━━━━━━━

📁 Arquivo: .agents/skills/{{skill_name}}/SKILL.md
📏 Linhas: N (< 500)
🗂️  Catálogo: flat (filha direta de .agents/skills/)

📋 RESULTADO:
   ∟ description: [preview 80 chars]
   ∟ disable-model-invocation: true/false
   ∟ Scripts: sim/não · References: sim/não

🔍 QUALIDADE:
   ∟ Frontmatter: ✅ 3 campos Zed
   ∟ Description: ✅ imperativa + "use quando"
   ∟ Naming flat: ✅
   ∟ Linhas: ✅ < 500

🚀 PRÓXIMOS PASSOS:
   ∟ Testar: /{{skill_name}} ou pergunta que bate com a description
   ∟ Otimizar trigger: /onion-meta-create-skill optimize {{skill_name}}
   ∟ Avaliar: /onion-meta-create-skill eval {{skill_name}}
━━━━━━━━━━━━━━
```

## 💡 Exemplos de Uso

```bash
/onion-meta-create-skill "processar faturas no formato TISS"
/onion-meta-create-skill validate .agents/skills/onion-engineer-start/
/onion-meta-create-skill optimize onion-product-task
/onion-meta-create-skill migrate .claude/commands/deploy.md
/onion-meta-create-skill eval csv-analyzer
```

## 🔗 Referências

- Specialist principal: `spawn_agent` → `.agents/onion/specialists/agent-skills-specialist.md`
- KB: `docs/knowledge-base/tools/agent-skills.md`
- Relacionados: `/onion-meta-create-command`, `/onion-meta-create-agent`
- Padrões e validação: skills `onion-patterns`, `onion-validation`
- ADR: `docs/meta-specs/adr/0001-zed-native-port.md`

## ⚠️ Notas

- **Catálogo flat**: a categoria vai no nome; subpasta não é descoberta pelo Zed.
- **Live reload**: o Zed detecta mudanças em `.agents/skills/` sem restart
  (exceto a primeira criação da pasta).
- Skill ativa permanece em contexto pelo resto da sessão — cada linha extra é
  custo recorrente; mantenha < 500 linhas.
- Skill sem contexto de domínio real tem valor mínimo — extraia de
  runbooks/schemas/PRs. Se o agente já resolve o task sem a skill, ela não
  agrega valor.
