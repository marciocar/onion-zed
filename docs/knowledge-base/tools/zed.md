# Zed — Knowledge Base

---

## 📋 Metadata

| Campo | Valor |
|-------|-------|
| **Versão** | 1.0.0 |
| **Data de Criação** | 2026-06-03 |
| **Última Atualização** | 2026-06-03 |
| **Categoria** | tools |
| **Fontes Principais** | [zed.dev/docs](https://zed.dev/docs/), [zed.dev/docs/ai/overview](https://zed.dev/docs/ai/overview), [zed.dev/docs/ai/skills](https://zed.dev/docs/ai/skills), [zed.dev/docs/ai/mcp](https://zed.dev/docs/ai/mcp) |

---

## 📋 Visão Geral

**Zed** é um editor de código open-source construído em **Rust com aceleração GPU**, focado em performance máxima e IA integrada nativamente. Criado pelos autores do Atom e Tree-sitter, foi projetado do zero para collab em tempo real e agentes de IA — sem depender de extensões de terceiros para essas funcionalidades.

**Diferenciadores técnicos:**
- Escrito em Rust; renderização via GPU garante latência de edição < 1 ms
- Colaboração em tempo real embutida no core (sem extensão)
- IA como cidadã de primeira classe: Agent Panel, Inline Assistant, Edit Prediction, MCP, Skills, External Agents
- Suporta 60+ linguagens nativamente; extensível via marketplace

**Linguagens com suporte nativo:** JavaScript, TypeScript, Python, Go, Rust, C/C++, Java, Ruby, PHP, Elixir, Kotlin, Swift, HTML, CSS, SQL, Docker, Terraform, YAML e mais.

---

## 🎯 Casos de Uso

| Caso | Quando usar |
|------|-------------|
| Desenvolvimento assistido por IA | Agentes que leem/escrevem código diretamente no editor |
| Múltiplos agentes em paralelo | Threads independentes por tarefa/projeto simultâneos |
| Agentes externos (Claude Code) | Integrar Claude Code como agente nativo via ACP |
| Colaboração em tempo real | Pair programming com voz integrada, sem extensões |
| Ambientes com LLMs locais | Ollama ou qualquer provedor OpenAI-compatible |
| Automação via MCP | Conectar ferramentas externas ao agente (GitHub, Figma, etc.) |

**Anti-casos — quando NÃO usar:**
- Ecossistemas que dependem fortemente de extensões VS Code sem equivalente no Zed (ainda em maturação)
- Times que usam Jupyter Notebooks como fluxo principal (suporte limitado)
- Ambientes enterprise que exigem auditoria estrita de extensões (catálogo menor que VS Code)

---

## ⚡ Quick Start

### Instalação

```bash
# macOS (Homebrew)
brew install --cask zed

# Linux (script oficial)
curl -f https://zed.dev/install.sh | sh

# Windows: download em https://zed.dev/download
```

### Importar configurações do VS Code

```
Cmd+Shift+P (macOS) / Ctrl+Shift+P (Linux/Windows)
→ zed: import vs code settings
```

### Configurar modelo de IA (settings.json)

```json
{
  "agent": {
    "default_model": {
      "provider": "anthropic",
      "model": "claude-opus-4-8"
    }
  },
  "language_models": {
    "anthropic": {
      "api_key": "sk-ant-..."
    }
  }
}
```

### Atalhos essenciais

| Ação | macOS | Linux/Windows |
|------|-------|---------------|
| Paleta de comandos | `Cmd+Shift+P` | `Ctrl+Shift+P` |
| Buscar arquivo | `Cmd+P` | `Ctrl+P` |
| Abrir Agent Panel | `Cmd+Alt+J` | `Ctrl+Alt+J` |
| Inline Assistant | `Ctrl+Enter` | `Ctrl+Enter` |
| Alternar threads | `Ctrl+Tab` | `Ctrl+Tab` |

---

## 🔧 Configuração e Uso

### Agent Panel e Threads

O **Agent Panel** é a interface principal de conversa com o agente ativo. Cada **Thread** opera de forma isolada com janela de contexto, agente e histórico próprios.

- **Múltiplos threads simultâneos**: abra quantos forem necessários via `Cmd+Alt+J`
- **Por projeto**: threads são agrupados por worktree na barra lateral
- **Git worktrees**: use worktrees separadas para evitar conflitos quando dois agentes editam os mesmos arquivos

### Ferramentas disponíveis para o agente

| Categoria | Ferramentas |
|-----------|-------------|
| **Leitura/busca** | `read_file`, `grep`, `find_path`, `list_directory`, `diagnostics`, `fetch` |
| **Edição** | `edit_file`, `write_file`, `create_directory`, `copy_path`, `move_path`, `delete_path` |
| **Execução** | `terminal` |
| **Paralelismo** | `spawn_agent` (cria subagente com contexto próprio) |
| **Web** | `search_web` (exclusivo Zed Pro) |

### Permissões de ferramentas

```json
{
  "agent": {
    "tool_permissions": {
      "default": "confirm",
      "always_allow": ["read_file", "list_directory", "grep"],
      "always_deny": ["delete_path"],
      "always_confirm": ["terminal", "write_file"]
    }
  }
}
```

Prioridade: `built-in > always_deny > always_confirm > always_allow > default`

### MCP (Model Context Protocol)

Adiciona ferramentas externas ao agente. Configuração em `settings.json`:

```json
{
  "context_servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "<token>" }
    },
    "servidor-remoto": {
      "url": "https://meu-servidor.com/mcp",
      "headers": { "Authorization": "Bearer <token>" }
    }
  }
}
```

Ferramentas MCP usam o formato `mcp:<servidor>:<ferramenta>` nas permissões (ex: `mcp:github:create_issue`).

### Provedores de LLM suportados

| Provedor | Notas |
|----------|-------|
| **Anthropic** | Padrão (`claude-sonnet-4-5`); suporta extended thinking |
| **OpenAI** | Suporte a modelos de raciocínio com `max_completion_tokens` |
| **Google AI** | Modelos experimentais; controle de thinking tokens |
| **Ollama** | Local; descoberta automática de modelos |
| **Amazon Bedrock** | 3 métodos de autenticação; cross-region inference |
| **OpenRouter** | Gateway multi-provedor |
| **GitHub Copilot** | Via subscription, sem chave API extra |
| **DeepSeek, Mistral, xAI** | Configurados via chave API |
| **OpenAI-compatible** | Qualquer endpoint compatível |

---

## 💡 Best Practices

**1. Skills em vez de Rules para instruções reutilizáveis** ([fonte](https://zed.dev/docs/ai/rules))
> A partir do Zed v1.4.0, Skills substituíram as Rules on-demand. Use `.rules` apenas para instruções globais automáticas; use Skills para contextos especializados e reutilizáveis.

**2. Modelos específicos por tarefa** ([fonte](https://zed.dev/docs/ai/agent-settings))
> Configure `subagent_model` diferente do `default_model` — subagentes em tarefas paralelas podem usar modelo mais rápido/barato sem impactar o agente principal.

**3. Worktrees separadas para agentes paralelos** ([fonte](https://zed.dev/docs/ai/parallel-agents))
> Quando dois threads editam o mesmo projeto, use Git worktrees para evitar conflitos de arquivo. Cada thread opera sua própria cópia.

**4. Field selection em ferramentas** ([fonte](https://zed.dev/docs/ai/tools))
> O `spawn_agent` isola contexto; use-o para delegar subtarefas longas sem poluir a janela de contexto do agente principal.

**5. Chaves API via settings, nunca hardcoded** ([fonte](https://zed.dev/docs/ai/llm-providers))
> Zed armazena chaves no keychain do SO — configure via `agent: open settings`, não diretamente em `settings.json` versionado.

---

## ⚠️ Limitações e Gotchas

| Limitação | Detalhe |
|-----------|---------|
| **Extensões VS Code** | Não compatível diretamente; extensões precisam ser reescritas para a API do Zed |
| **`search_web`** | Exclusivo para assinantes Zed Pro com provedor Zed |
| **Skills locais e worktree trust** | Skills em `<worktree>/.agents/skills/` só ativam em worktrees marcadas como confiáveis |
| **Jupyter Notebooks** | Suporte limitado comparado ao VS Code com extensão oficial |
| **Windows** | Suporte mais recente; algumas funcionalidades podem estar atrasadas em relação a macOS/Linux |
| **Agente não edita SKILL.md** | Por segurança, o agente não pode modificar arquivos de skill sem autorização explícita do usuário |
| **MCP: Resources não suportado** | Apenas `Tools` e `Prompts` são suportados — não `Resources` |

---

## 🔗 Integração com o Sistema Onion

O Zed é a **plataforma única** do Sistema Onion. Desde o port nativo (2026-06-03, [ADR 0001](../../meta-specs/adr/0001-zed-native-port.md)), o framework roda inteiramente sobre primitivos nativos do Zed — sem dependência do Claude Code.

### Mapeamento de conceitos Onion → Zed (estrutura real)

| Componente Onion | Primitivo Zed | Localização |
|-------------|-------------------|-------------|
| **Skills** (81, ex-comandos + core) | Skills (slash `/` e `@`) | `.agents/skills/onion-<cat>-<cmd>/SKILL.md` |
| **Specialists** (49, ex-agentes) | `spawn_agent` (subagente delegado) | `.agents/onion/specialists/<slug>.md` |
| **Rules** | `AGENTS.md` (lido nativamente) | `AGENTS.md` (raiz) |
| **Config + permissões + modelos** | `.zed/settings.json` | `agent.tool_permissions`, `agent.default_model`, `context_servers` |
| **Task Manager Abstraction** | utils agnósticos | `.agents/onion/utils/task-manager/` |
| **MCP (ClickUp)** | `context_servers` | `.zed/settings.json` |
| **Múltiplos workflows** | Threads paralelos | Threads Sidebar |

### Regras do port (essenciais)

- **Catálogo flat**: skills são filhas diretas de `.agents/skills/` (sem subpastas) — naming `onion-<cat>-<cmd>`.
- **Frontmatter Zed**: só `name`, `description`, `disable-model-invocation`.
- **Delegação**: specialists via `spawn_agent` ("Leia `.agents/onion/specialists/<x>.md` e atue como esse especialista…").
- **Permissões globais**: `.zed/settings.json` (não há `allowed-tools` por skill).
- **Sem injeção dinâmica**: ler `.env` via tool, não `!cmd`.
- **Worktree trust**: skills locais e `context_servers` exigem worktree confiável.

### Skills relacionadas

- `/onion-meta-create-skill` — criar nova skill Zed
- `/onion-meta-setup-integration` — configurar integrações (Jira REST, ClickUp MCP)
- `/onion-engineer-warm-up` — contexto técnico do projeto
- `spawn_agent` → `specialists/zed-specialist.md` — troubleshooting do ambiente Zed

---

## 🔗 Referências

- [Documentação oficial do Zed](https://zed.dev/docs/)
- [Visão geral de IA no Zed](https://zed.dev/docs/ai/overview)
- [Skills — formato e criação](https://zed.dev/docs/ai/skills)
- [MCP no Zed](https://zed.dev/docs/ai/mcp)
- [Ferramentas do agente](https://zed.dev/docs/ai/tools)
- [External Agents / ACP](https://zed.dev/docs/ai/external-agents)
- [Parallel Agents](https://zed.dev/docs/ai/parallel-agents)
- [Inline Assistant](https://zed.dev/docs/ai/inline-assistant)
- [Rules](https://zed.dev/docs/ai/rules)
- [Agent Settings](https://zed.dev/docs/ai/agent-settings)
- [LLM Providers](https://zed.dev/docs/ai/llm-providers)
- [Worktree Trust](https://zed.dev/docs/worktree-trust)
- [Migração do VS Code](https://zed.dev/docs/migrate/vs-code)
- KBs relacionadas: [agent-skills](./agent-skills.md) · [claude-code-commands-best-practices-2025](./claude-code-commands-best-practices-2025.md)

---

**Última atualização**: 2026-06-03
**Fonte principal**: https://zed.dev/docs/
