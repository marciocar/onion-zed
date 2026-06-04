---
name: onion-meta-all-tools
description: Documenta as ferramentas NATIVAS do Zed (read_file, write_file, edit_file, terminal, grep, find_path, list_directory, fetch, diagnostics, spawn_agent, search_web) e as tools MCP expostas via context_servers, organizadas por categoria em .agents/onion/docs/tools/. Use quando precisar catalogar ou referenciar as ferramentas do ambiente.
disable-model-invocation: true
---

# 🧰 Listagem de Ferramentas Nativas do Zed

## 🎯 Objetivo

Documentar todas as ferramentas disponíveis no ambiente **nativo Zed**,
organizadas por categoria, em `.agents/onion/docs/tools/`.

## 🛠️ Ferramentas Nativas do Zed

O Zed expõe um conjunto fixo de tools nativas (nomes em snake_case). As
permissões são **globais** em `.zed/settings.json` (`agent.tool_permissions`),
nunca por skill/specialist.

### Operações de arquivo
- `read_file` — lê o conteúdo de um arquivo.
- `write_file` — cria ou sobrescreve um arquivo.
- `edit_file` — edição pontual (substituição de trecho) em arquivo existente.

### Busca e navegação
- `grep` — busca por conteúdo (regex) no projeto.
- `find_path` — localiza arquivos por padrão de caminho/nome.
- `list_directory` — lista o conteúdo de um diretório.

### Execução e delegação
- `terminal` — executa comandos shell (substitui as permissões `Bash(...)` do
  Claude Code; aqui o escopo é global em `agent.tool_permissions`).
- `spawn_agent` — delega uma tarefa a um specialist (substitui o `@agente` do
  Claude Code): "Leia `.agents/onion/specialists/<slug>.md` e atue como esse
  especialista para: …".

### Web
- `fetch` — busca o conteúdo de uma URL.
- `search_web` — pesquisa na web.

### Diagnóstico
- `diagnostics` — lê diagnósticos do projeto (erros/avisos de linguagem/LSP).

### Ferramentas MCP (via `context_servers`)
- Servidores MCP são configurados em `.zed/settings.json` na chave
  `context_servers` (não `.mcp.json`/`mcpServers`).
- Exemplos no Onion: **ClickUp** (MCP standalone) e **Jira** (REST direto, sem
  MCP). MCPs claude.ai-hosted (Asana/Linear/Atlassian) **não** funcionam no Zed.
- As tools MCP aparecem com prefixo próprio do servidor e ficam disponíveis ao
  agente quando o `context_server` está ativo.

## 📋 Instruções

### 1. Estrutura de Arquivos

Gere arquivos por categoria em `.agents/onion/docs/tools/`:

- `native-tools.md` — ferramentas nativas do Zed (lista acima, por categoria).
- `mcp.md` — tools MCP disponíveis via `context_servers` (ClickUp, etc.).
- `skills.md` — skills do catálogo `.agents/skills/` (invocáveis por `/` e `@`).
- `specialists.md` — personas delegáveis em `.agents/onion/specialists/`
  (acionadas via `spawn_agent`).
- `rules.md` — regras/config do workspace (`AGENTS.md`, `.zed/settings.json`).
- `[categoria].md` — outras categorias relevantes.

### 2. Formato de Cada Item

```typescript
// Assinatura
function read_file(path: string): string
// Propósito: lê o conteúdo de um arquivo do projeto.
```

### 3. Estrutura de Cada Arquivo
- **Índice** no início (links internos).
- **Hierarquia** quando aplicável (subcategorias, grupos).
- **Lista de marcadores** por ferramenta.
- **Exemplos práticos** quando relevante.

### 4. README Principal

Crie `.agents/onion/docs/tools/README.md` com:
- Visão geral da documentação de ferramentas.
- Índice de todos os arquivos de categoria.
- Guia rápido de uso (nativas vs. MCP).

## ⚙️ Execução

1. Se `.agents/onion/docs/tools/README.md` existir → perguntar:
   **Substituir** ou **Atualizar**?
2. Levantar as ferramentas disponíveis no contexto:
   - Nativas: lista fixa acima.
   - MCP: ler `context_servers` em `.zed/settings.json` (tool `read_file`).
   - Skills: `list_directory .agents/skills/`.
   - Specialists: `list_directory .agents/onion/specialists/`.
3. Organizar por categoria.
4. Gerar os arquivos markdown em `.agents/onion/docs/tools/`.
5. Confirmar a criação/atualização.

## 🔗 Referências

- ADR (mapeamento de tools): `docs/meta-specs/adr/0001-zed-native-port.md`
- Padrões: skills `onion-patterns`, `onion-validation`
- Config de MCP/permissões: `.zed/settings.json`
  (`context_servers`, `agent.tool_permissions`)
