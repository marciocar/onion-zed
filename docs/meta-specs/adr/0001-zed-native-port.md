# ADR 0001 — Port Nativo do Onion para Zed (Zed-exclusivo)

| Campo | Valor |
|-------|-------|
| **Status** | Aceito |
| **Data** | 2026-06-03 |
| **Decisores** | Marcio Carvalho |
| **Supera** | "Plataforma única: Claude Code" (identidade canônica de 2026-05-18, [onion-review-2026-05](../../analysis/onion-review-2026-05.md)) |
| **Spike de suporte** | [zed-port-spike-2026-06](../../analysis/zed-port-spike-2026-06.md) |

---

## Contexto

O Sistema Onion foi construído 100% acoplado ao **Claude Code**: 77 comandos slash (`.claude/commands/`), 49 agentes nomeados invocáveis por `@` (`.claude/agents/`), 4 skills (`.claude/skills/`), hooks (`SessionStart`), permissões `Bash(...)` e MCP via `.mcp.json`. Esses são primitivos exclusivos do Claude Code.

A direção estratégica mudou: o ambiente de trabalho primário passa a ser o **Zed** (1.5.3, com `claude-acp` já configurado), e a decisão é tornar o framework **nativo e exclusivo do Zed**, abandonando a dependência do Claude Code standalone.

## Decisão

1. **Port nativo total** — todos os artefatos do Onion são reescritos sobre primitivos nativos do Zed: Skills (`.agents/skills/`), Rules (`AGENTS.md`), `spawn_agent`, External Agents/ACP, `context_servers` e `agent.tool_permissions`.
2. **Zed-exclusivo** — abandona-se a compatibilidade com Claude Code standalone (CLI/terminal sem Zed). Não haverá dupla manutenção.
3. **Nova identidade de plataforma:** "Plataforma única: **Zed**".

## Régua do port (mapeamento canônico)

| Onion (Claude Code) | Destino nativo Zed |
|---|---|
| Comando `/cat/x` (`.claude/commands/<cat>/<x>.md`) | Skill `onion-<cat>-<x>` (`.agents/skills/onion-<cat>-<x>/SKILL.md`), slash `/onion-<cat>-<x>` |
| Agente `@x` (`.claude/agents/<cat>/<x>.md`) | Persona `.agents/onion/specialists/<x>.md` + delegação via `spawn_agent` |
| Skill `.claude/skills/<x>/` | `.agents/skills/<x>/SKILL.md` |
| `CLAUDE.md` | `AGENTS.md` (rules nativas Zed) |
| `.claude/settings.json` (permissions/hooks) | `.zed/settings.json` (`agent.tool_permissions`) |
| `.mcp.json` (`mcpServers`) | `.zed/settings.json` (`context_servers`) |
| `.claude/utils/task-manager/` | `.agents/onion/utils/task-manager/` (lógica intacta) |
| `common/templates`, `common/prompts` | `.agents/onion/templates/`, `.agents/onion/prompts/` |

### Convenção de naming (regra dura)

- **Skills**: `onion-<categoria>-<comando>`, lowercase + hífen, ≤64 chars. O catálogo Zed é **flat** (sem subpastas) → o prefixo de categoria está no nome. Ex.: `/git/feature/start` → `onion-git-feature-start`.
- **Specialists**: arquivo `.agents/onion/specialists/<slug>.md` (slug kebab-case do agente original).
- **Core skills** (sem prefixo de categoria): `onion`, `onion-warmup`, `onion-patterns`, `onion-validation`, `language-standards`.

## Consequências

### Positivas
- Independência total do Claude Code; alinhamento com o ambiente real de trabalho.
- Skills do Zed são invocáveis por `/` e `@` → preserva a UX de slash command dos 77 comandos.
- Abstração de Task Manager (SDAAL) migra sem reescrita de lógica.

### Negativas / trade-offs aceitos
- **Escopo de tools por-artefato é perdido** (`allowed-tools`/`tools:` por comando/agente) → vira `agent.tool_permissions` **global**.
- **Hooks não existem no Zed** → detecção de `TASK_MANAGER_PROVIDER` passa a ser **instruction-driven** (orquestrador lê `.env` via tool), menos garantida que o `SessionStart`.
- **Injeção dinâmica de contexto** (`` !`cmd` ``) em skills não existe → migra para tool calls.
- **MCPs claude.ai-hosted** (Asana/Linear/Atlassian) não funcionam no Zed → Jira usa REST direto ✅; ClickUp MCP standalone ✅; Asana/Linear pendentes (MCP standalone ou adapter REST).
- **Catálogo de ~81 skills** pode pesar na UX → mitigado por prefixo `onion-<cat>-` e `disable-model-invocation`.
- Sem registry de subagentes nomeados → delegação Master-Slave passa estado via prompt/arquivo.

## Alternativas descartadas

- **Híbrido (ACP + nativo):** Claude Code dentro do Zed preservaria 100% da funcionalidade com baixo custo, mas mantém a dependência do Claude Code — rejeitado por contrariar o objetivo de independência.
- **Só fundação Zed:** apenas adicionar Zed por cima do Onion atual — rejeitado por não "transformar" o framework.
