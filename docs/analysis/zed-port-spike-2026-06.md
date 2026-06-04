# Spike de Validação — Port Nativo do Onion para Zed

> **Data**: 2026-06-03 | **Status**: concluído | **Bloqueante para**: Phase 2 do [plano de port](../../../.claude/plans/) | **Ambiente**: Zed 1.5.3 (Windows + WSL2)

Validação empírica das 5 questões abertas que afetam o desenho do port. Fontes: documentação oficial Zed (`zed.dev/docs/ai/*`) + inspeção do ambiente instalado.

---

## Ambiente observado

| Item | Valor |
|------|-------|
| Versão Zed | 1.5.3 (`5d370d67`) |
| Binário | `/mnt/c/Users/Carva/AppData/Local/Programs/Zed/bin/zed` (Windows, acessível via WSL) |
| Config global | `/mnt/c/Users/Carva/AppData/Roaming/Zed/settings.json` |
| Já configurado | `agent_servers.claude-acp` (`type: registry`) → **usuário já roda Claude Code via ACP no Zed** |
| Keymap | `base_keymap: VSCode` |
| `.agents/` / `.zed/` no projeto | inexistentes (greenfield para o port) |

---

## Resultados das 5 questões

### 1. Skills suportam pastas aninhadas? ❌ NÃO — layout flat obrigatório
> *"Flat layout only. Skills must be direct children of the skills root. Nested folders like `~/.agents/skills/group/my-skill/` are not discovered."*

**Decisão:** naming `onion-<cat>-<x>` (prefixo de categoria no nome do skill) é **obrigatório**, não opcional. Não há subpastas por categoria. Ex.: `.agents/skills/onion-git-feature-start/SKILL.md`.

### 2. Injeção dinâmica de contexto (`` !`cmd` ``)? ❌ NÃO documentada
SKILL.md não tem mecanismo de shell injection nem substituição de variáveis. Pode linkar arquivos de referência (lidos via tool).

**Decisão:** a leitura de `.env` (`TASK_MANAGER_PROVIDER`) — hoje feita por injeção no skill `onion` — migra para **instrução** que manda o agente usar a tool `read_file`/`terminal` como primeiro passo.

### 3. Frontmatter aceito? ✅ Apenas 3 campos
`name` (obrigatório), `description` (obrigatório), `disable-model-invocation` (opcional). Demais campos da spec Agent Skills "planejados para o futuro" → hoje **ignorados**.

**Decisão:** dropar `model`, `allowed-tools`, `category`, `tags`, `parameters`, `version`, `paths` do frontmatter. Informação essencial migra para o corpo ou para `.zed/settings.json` (permissões globais).

### 4. `spawn_agent` para delegação? ✅ SIM (ad-hoc, sem registry)
> *"Spawns a subagent with its own context window to perform a delegated task. Useful for running parallel investigations, completing self-contained tasks, or performing research where only the outcome matters. Each subagent has access to the same tools as the parent agent."*

Não há registry de subagentes nomeados. O subagente recebe uma "delegated task" (prompt) e herda as mesmas tools.

**Decisão:** o padrão de delegação `@especialista` vira `spawn_agent` com prompt do tipo *"Leia `.agents/onion/specialists/<x>.md` e atue como esse especialista para: <tarefa>"*. Padrões Master-Slave (ex. C4→Mermaid) passam estado via prompt/arquivo, não via registry.

### 5. MCP standalone para Asana/Linear/Jira via `context_servers`? ⚠️ PARCIAL
- **Jira (provider ativo):** o adapter usa **REST via fetch direto** → **não precisa de MCP**. ✅ Funciona standalone no Zed.
- **ClickUp:** MCP standalone (`npx`) → vira `context_server`. ✅
- **Asana / Linear:** hoje dependem de MCP **claude.ai-hosted** (Anthropic-managed), que **não** existe no Zed. Precisam de MCP standalone de terceiros **ou** novo adapter REST. ⚠️ Decidir on-demand quando o provider for ativado; fallback gracioso para modo `none`.

---

## Impacto consolidado no plano

| Questão | Confirma desenho? | Ajuste necessário |
|---------|-------------------|-------------------|
| 1. Flat layout | ✅ | Naming `onion-<cat>-<x>` é regra dura |
| 2. Sem injeção dinâmica | ⚠️ | `.env` via tool call (instrução), não `!cmd` |
| 3. Frontmatter mínimo | ✅ | Dropar campos extras; permissões → `.zed/settings.json` |
| 4. spawn_agent | ✅ | Delegação por prompt + arquivo de persona |
| 5. MCP por provider | ⚠️ | Jira REST ✅; ClickUp MCP ✅; Asana/Linear pendente |

**Veredito:** o plano é viável sem mudanças estruturais. Os dois ajustes (`.env` via tool, Asana/Linear pendente) já estão previstos. **Liberado para Phase 0.**
