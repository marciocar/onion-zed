---
name: onion-validation
description: >
  Regras de validação para componentes do Sistema Onion (nativo Zed). Use ao
  criar, revisar, auditar ou debuggar skills e specialists. Cobre frontmatter
  Zed, naming flat, checklists de qualidade, limites de linhas, detecção de
  duplicações e scoring. Ative ao validar artefatos em `.agents/`, mesmo sem
  o usuário mencionar "validação".
---

## Validação de Skills (`.agents/skills/<nome>/SKILL.md`)

### Frontmatter obrigatório (Zed aceita só 3 campos)
| Campo | Tipo | Constraint |
|-------|------|------------|
| `name` | string | kebab-case, = nome da pasta, naming `onion-<cat>-<cmd>` para ex-comandos |
| `description` | string | imperativa, com "use quando", <1024 chars |
| `disable-model-invocation` | bool | opcional; `true` = só `/` e `@` |

### Categorias válidas (prefixo de naming de skills)
`engineer`, `product`, `git`, `docs`, `meta`, `validate`, `test`, `development`, `quick`

### Checklist de qualidade
- [ ] `name` único, kebab-case, = nome da pasta
- [ ] Pasta é filha DIRETA de `.agents/skills/` (sem subpasta)
- [ ] `description` com verbo imperativo + "use quando"
- [ ] < 500 linhas
- [ ] Sem campos de frontmatter inválidos (`model`, `category`, `tags`, `allowed-tools`, `paths`)
- [ ] Referências a `@agente` convertidas em `spawn_agent` + persona
- [ ] Referências a `/cat/cmd` convertidas em `/onion-cat-cmd`

## Validação de Specialists (`.agents/onion/specialists/<slug>.md`)

### Frontmatter mínimo
- `name` — kebab-case, único
- `description` — especialização clara + quando delegar

### Checklist
- [ ] Nome único, kebab-case
- [ ] < 300 linhas
- [ ] Sem `tools:`/`model:`/`category:` (permissões são globais; não há registry Zed)
- [ ] Tool names em snake_case Zed (`read_file`, `terminal`, `grep`, etc.)
- [ ] Delegação a outros specialists via `spawn_agent`
- [ ] Seção de identidade/propósito + expertise presentes

## Validações Automatizadas (tool `terminal`)

### Duplicações de nome de skill
```bash
grep -rh "^name:" .agents/skills/*/SKILL.md | awk -F: '{print $2}' | tr -d ' ' | sort | uniq -d
```

### Limites de linhas
```bash
find .agents/skills -name SKILL.md -exec wc -l {} \; | awk '$1 > 500'
find .agents/onion/specialists -name "*.md" -exec wc -l {} \; | awk '$1 > 300'
```

### Skills aninhadas (erro — não descobertas pelo Zed)
```bash
find .agents/skills -mindepth 2 -name SKILL.md | grep -v '^.agents/skills/[^/]*/SKILL.md$'
```

### Specialists fantasmas referenciados
```bash
grep -rho "specialists/[a-z-]\+\.md" .agents docs | sort -u | while read ref; do
  test -f ".agents/onion/$ref" || echo "FANTASMA: $ref"
done
```

### Resíduos de Claude Code
```bash
grep -rl "\.claude/\|allowed-tools\|model: sonnet\|@[a-z-]*-specialist" .agents/ 2>/dev/null
```

## Score de Qualidade (0-100)

| Critério (skill) | Pontos | | Critério (specialist) | Pontos |
|----------|--------|-|----------|--------|
| Frontmatter válido (3 campos) | +25 | | Frontmatter mínimo válido | +25 |
| Naming flat correto | +20 | | Tool names snake_case Zed | +20 |
| description com "use quando" | +20 | | Sem campos Claude Code | +20 |
| < 500 linhas | +15 | | < 300 linhas | +15 |
| Sem resíduo `.claude`/`@agente` | +20 | | Delegação via spawn_agent | +20 |

**Thresholds:** 80-100 ✅ Aprovado · 60-79 ⚠️ Melhorias · <60 ❌ Rejeitado

## Regras para Geradores

### Antes de criar skill
1. Verificar se a pasta já existe em `.agents/skills/`
2. Confirmar naming `onion-<cat>-<cmd>` e que é filha direta
3. `description` com "use quando"; < 500 linhas

### Antes de criar specialist
1. Verificar se já existe em `.agents/onion/specialists/`
2. Frontmatter mínimo; tool names snake_case; < 300 linhas

## Integração com .env

```bash
grep -E "^TASK_MANAGER_PROVIDER=" .env || echo "⚠️ TASK_MANAGER_PROVIDER ausente — rode /onion-meta-setup-integration"
```
Válidos: `clickup` | `jira` | `asana` | `linear` | `none`.

## Fallback para falhas
1. Informar o problema específico; 2. sugerir correção concreta; 3. perguntar se aplica auto-fix; 4. se não, abortar com mensagem clara.

## Referências

- Conformidade arquitetural: `spawn_agent` → `specialists/metaspec-gate-keeper.md`
- Skills relacionadas: `onion-patterns`, `language-standards`
- ADR: `docs/meta-specs/adr/0001-zed-native-port.md`
