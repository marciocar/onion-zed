---
title: "Referência de Ferramentas Nativas do Zed"
description: "Ferramentas nativas do Zed usadas pelo Sistema Onion + MCP via context_servers"
last_updated: "2026-06-03"
category: "onion"
tags: [tools, reference, onion, zed, ai-agents]
---

# 🛠️ Referência de Ferramentas (nativo Zed)

Este documento lista as ferramentas disponíveis para skills e specialists do Sistema Onion **no Zed**, organizadas por categoria. Ver [ADR 0001](../meta-specs/adr/0001-zed-native-port.md).

> **Ferramentas nativas do Zed** (nomes canônicos): `read_file`, `write_file`, `edit_file`, `terminal`, `grep`, `find_path`, `list_directory`, `fetch`, `diagnostics`, `spawn_agent`, `search_web`. As assinaturas TypeScript abaixo são ilustrativas. **Permissões** dessas tools são **globais** em `.zed/settings.json` (`agent.tool_permissions`) — não por skill.
>
> **MCP no Zed:** servidores MCP (ClickUp, Context7, Firecrawl, etc.) são declarados como **`context_servers`** em `.zed/settings.json` e exigem **worktree trust**. As seções de MCP mais abaixo continuam válidas — apenas a forma de declaração mudou (de `.mcp.json` para `context_servers`).

## 📋 Índice por Categoria

- [📁 Busca e Exploração de Código](#-busca-e-exploração-de-código)
- [📝 Manipulação de Arquivos](#-manipulação-de-arquivos)
- [⚡ Terminal e Execução](#-terminal-e-execução)
- [🤖 Delegação a Specialists](#-delegação-a-specialists)
- [📓 Jupyter Notebooks](#-jupyter-notebooks)
- [🔍 Diagnostics (Linting)](#-diagnostics-linting)
- [🌐 Busca na Web e Fetch](#-busca-na-web-e-fetch)
- [🧠 Memórias](#-memórias)
- [✅ Gestão de Tarefas](#-gestão-de-tarefas)
- [🔌 MCP via context_servers](#-mcp-via-context_servers)
- [📋 ClickUp MCP](#-clickup-mcp-gestão-de-projetos)
- [📚 Context7 MCP](#-context7-mcp-documentação)
- [🧭 Sequential Thinking MCP](#-sequential-thinking-mcp-análise-complexa)
- [💻 Code Understanding MCP](#-code-understanding-mcp-análise-de-repositórios)
- [🕷️ Firecrawl MCP](#️-firecrawl-mcp-web-scraping)
- [🖥️ Chrome DevTools MCP](#️-chrome-devtools-mcp-automação-browser)
- [⚙️ NX Extension MCP](#️-nx-extension-mcp-framework-nx)
- [🌿 Skills Git Gitflow](#-skills-git-gitflow)

---

## 📁 Busca e Exploração de Código

### `grep`
```typescript
function grep(
  pattern: string,
  path?: string,
  output_mode?: "content" | "files_with_matches" | "count",
  type?: string,
  glob?: string,
  multiline?: boolean,
  head_limit?: number,
  A?: number, // context after
  B?: number, // context before  
  C?: number, // context both
  i?: boolean // case insensitive
): Promise<GrepResults>
```
**Propósito**: Busca poderosa baseada em ripgrep com regex completo (ferramenta nativa `grep`)

**Quando usar**:
-  Busca exata de símbolos/strings
-  Padrões regex complexos
-  Múltiplos arquivos rapidamente
-  Explorar codebases desconhecidas (busca por padrão em vez de busca semântica)

### `find_path`
```typescript
function find_path(
  glob_pattern: string,
  target_directory?: string
): Promise<FileList>
```
**Propósito**: Busca arquivos por padrões glob (ferramenta nativa do Zed; substitui o antigo `glob_file_search`)

**Exemplo**:
```bash
find_path("**/*.test.ts") // Todos arquivos de teste TypeScript
```

---

## 📝 Manipulação de Arquivos

### `read_file`
```typescript
function read_file(
  target_file: string,
  offset?: number,
  limit?: number
): Promise<FileContent>
```
**Propósito**: Leitura de arquivos do sistema local com numeração de linha

### `write_file`
```typescript
function write_file(
  file_path: string,
  contents: string
): Promise<WriteResult>
```
**Propósito**: Escrita/sobrescrever arquivos no sistema local (ferramenta nativa `write_file`)

### `edit_file`
```typescript
interface EditOperation {
  old_string: string;
  new_string: string;
  replace_all?: boolean;
}

function edit_file(
  file_path: string,
  edits: EditOperation[]
): Promise<EditResult>
```
**Propósito**: Edição de arquivos no Zed (ferramenta nativa `edit_file`). Substitui os antigos `search_replace` e `MultiEdit` — faz substituições exatas, uma ou várias por chamada.

### `list_directory`
```typescript
function list_directory(
  target_directory: string,
  ignore_globs?: string[]
): Promise<DirectoryListing>
```
**Propósito**: Listagem de diretórios com filtros opcionais (ferramenta nativa `list_directory`)

> Não há ferramenta nativa dedicada de exclusão de arquivo no Zed — use `terminal` (`rm`) quando necessário e permitido por `agent.tool_permissions`.

---

## ⚡ Terminal e Execução

### `terminal`
```typescript
function terminal(
  command: string,
  is_background?: boolean,
  explanation?: string
): Promise<CommandResult>
```
**Propósito**: Execução de comandos no terminal (ferramenta nativa `terminal`)

**Características**:
- Suporte a comandos background quando aplicável
- Flags não-interativas automáticas
- Pipe para `cat` em comandos com pager

---

## 🤖 Delegação a Specialists

### `spawn_agent`
```typescript
function spawn_agent(
  prompt: string
): Promise<AgentResult>
```
**Propósito**: Dispara um subagente no Zed para atuar como um specialist do Onion. O subagente herda as mesmas tools e tem janela de contexto própria.

**Padrão Onion**:
```text
spawn_agent("Leia .agents/onion/specialists/<slug>.md e atue como esse especialista para: <tarefa>")
```

**Quando usar**:
-  Delegar trabalho especializado (ex.: `react-developer`, `code-reviewer`, `jira-specialist`)
-  Isolar contexto de uma subtarefa longa
-  Orquestração Master-Slave (a skill principal coordena, os specialists executam)

---

## 📓 Jupyter Notebooks

### `edit_notebook`
```typescript
function edit_notebook(
  target_notebook: string,
  cell_idx: number,
  is_new_cell: boolean,
  cell_language: "python" | "markdown" | "javascript" | "typescript" | "r" | "sql" | "shell" | "raw" | "other",
  old_string: string,
  new_string: string
): Promise<EditResult>
```
**Propósito**: Edição especializada de células de notebooks Jupyter

---

## 🔍 Diagnostics (Linting)

### `diagnostics`
```typescript
function diagnostics(
  paths?: string[]
): Promise<DiagnosticsResults>
```
**Propósito**: Leitura de erros/avisos (linter, type-check) do workspace atual (ferramenta nativa `diagnostics`; substitui o antigo `read_lints`)

---

## 🌐 Busca na Web e Fetch

### `search_web`
```typescript
function search_web(
  search_term: string,
  explanation?: string
): Promise<SearchResults>
```
**Propósito**: Busca informações em tempo real na web (ferramenta nativa `search_web`)

**Quando usar**:
-  Informações atualizadas não disponíveis nos dados de treinamento
-  Verificação de fatos atuais
-  Pesquisa de tecnologias/eventos recentes

### `fetch`
```typescript
function fetch(
  url: string
): Promise<FetchResult>
```
**Propósito**: Busca o conteúdo de uma URL específica (ferramenta nativa `fetch`). Útil para ler documentação, APIs REST (ex.: chamadas diretas ao Jira REST) e páginas conhecidas.

---

## 🧠 Memórias

> **Nota (Zed):** o Onion **não** depende de uma tool nativa de memória. A persistência de contexto entre sessões é feita por **arquivos**: as **sessions** em `.agents/onion/sessions/<feature-slug>/` (`context.md`, `plan.md`, `decisions.md`, `progress.md`) e os documentos em `docs/` (business/technical/meta-specs). Para "lembrar" algo, grave com `write_file`/`edit_file` no arquivo de sessão apropriado.

---

## ✅ Gestão de Tarefas

### `todo_write`
```typescript
interface TodoItem {
  id: string;
  content: string;
  status: "pending" | "in_progress" | "completed" | "cancelled";
}

function todo_write(
  todos: TodoItem[],
  merge: boolean
): Promise<TodoResult>
```
**Propósito**: Criação e gerenciamento de listas de tarefas estruturadas

**Características**:
- Estados: `pending`, `in_progress`, `completed`, `cancelled`
- Suporte a merge ou substituição completa
- Ideal para planejamento e tracking de progresso

---

## 🔌 MCP via context_servers

> No Zed, servidores MCP são declarados em `.zed/settings.json` na chave `context_servers` e exigem **worktree trust**. As ferramentas de cada MCP ficam disponíveis para skills/specialists conforme `agent.tool_permissions`. As seções de MCP a seguir (ClickUp, Context7, Firecrawl, etc.) descrevem servidores que podem ser declarados dessa forma.

### `list_mcp_resources`
```typescript
function list_mcp_resources(
  server?: string
): Promise<ResourceList>
```
**Propósito**: Listagem de recursos disponíveis dos servidores MCP

### `fetch_mcp_resource`
```typescript
function fetch_mcp_resource(
  server: string,
  uri: string,
  downloadPath?: string
): Promise<ResourceContent>
```
**Propósito**: Busca de recursos específicos de servidores MCP

---

## 📋 ClickUp MCP (Gestão de Projetos)

### Gestão de Workspace
```typescript
function mcp_clickup_get_workspace_hierarchy(
  random_string: string
): Promise<WorkspaceHierarchy>
```

### Gestão de Tasks (CRUD)
```typescript
// Criar task única
function mcp_clickup_create_task(
  name: string,
  listId?: string,
  listName?: string,
  description?: string,
  markdown_description?: string,
  dueDate?: string,
  startDate?: string,
  priority?: 1 | 2 | 3 | 4,
  status?: string,
  assignees?: (number | string)[],
  tags?: string[],
  custom_fields?: CustomField[],
  parent?: string,
  check_required_custom_fields?: boolean
): Promise<TaskCreationResult>

// Obter detalhes de task
function mcp_clickup_get_task(
  taskId?: string,
  taskName?: string,
  customTaskId?: string,
  listName?: string,
  subtasks?: boolean
): Promise<TaskDetails>

// Atualizar task
function mcp_clickup_update_task(
  taskId?: string,
  taskName?: string,
  listName?: string,
  name?: string,
  description?: string,
  markdown_description?: string,
  dueDate?: string,
  startDate?: string,
  priority?: "1" | "2" | "3" | "4" | null,
  status?: string,
  assignees?: (number | string)[],
  custom_fields?: CustomField[],
  time_estimate?: string
): Promise<UpdateResult>
```

### Gestão de Tasks (Operações)
```typescript
// Mover task
function mcp_clickup_move_task(
  taskId?: string,
  taskName?: string,
  sourceListName?: string,
  listId?: string,
  listName?: string
): Promise<MoveResult>

// Duplicar task
function mcp_clickup_duplicate_task(
  taskId?: string,
  taskName?: string,
  sourceListName?: string,
  listId?: string,
  listName?: string
): Promise<DuplicateResult>

// Deletar task
function mcp_clickup_delete_task(
  taskId?: string,
  taskName?: string,
  listName?: string
): Promise<DeleteResult>
```

### Comentários em Tasks
```typescript
// Obter comentários
function mcp_clickup_get_task_comments(
  taskId?: string,
  taskName?: string,
  listName?: string,
  start?: number,
  startId?: string
): Promise<CommentsResult>

// Criar comentário
function mcp_clickup_create_task_comment(
  commentText: string,
  taskId?: string,
  taskName?: string,
  listName?: string,
  notifyAll?: boolean,
  assignee?: number
): Promise<CommentResult>
```

### Anexos em Tasks
```typescript
function mcp_clickup_attach_task_file(
  taskId?: string,
  taskName?: string,
  listName?: string,
  file_data?: string, // base64
  file_name?: string,
  file_url?: string, // URL ou path local
  auth_header?: string,
  chunk_index?: number,
  chunk_total?: number,
  chunk_session?: string,
  chunk_is_last?: boolean
): Promise<AttachmentResult>
```

### Operações Bulk (Alta Performance)
```typescript
// Criar múltiplas tasks
function mcp_clickup_create_bulk_tasks(
  tasks: BulkTaskItem[],
  listId?: string,
  listName?: string,
  options?: BulkOptions
): Promise<BulkResult>

// Atualizar múltiplas tasks
function mcp_clickup_update_bulk_tasks(
  tasks: BulkUpdateItem[],
  options?: BulkOptions
): Promise<BulkUpdateResult>

// Mover múltiplas tasks
function mcp_clickup_move_bulk_tasks(
  tasks: BulkMoveItem[],
  targetListId?: string,
  targetListName?: string,
  options?: BulkOptions
): Promise<BulkMoveResult>

// Deletar múltiplas tasks
function mcp_clickup_delete_bulk_tasks(
  tasks: BulkDeleteItem[],
  options?: BulkOptions
): Promise<BulkDeleteResult>
```

### Busca Avançada de Tasks
```typescript
function mcp_clickup_get_workspace_tasks(
  tags?: string[],
  list_ids?: string[],
  folder_ids?: string[],
  space_ids?: string[],
  statuses?: string[],
  assignees?: string[],
  custom_fields?: object,
  date_created_gt?: number,
  date_created_lt?: number,
  date_updated_gt?: number,
  date_updated_lt?: number,
  due_date_gt?: number,
  due_date_lt?: number,
  archived?: boolean,
  include_closed?: boolean,
  include_archived_lists?: boolean,
  include_closed_lists?: boolean,
  include_subtasks?: boolean,
  subtasks?: boolean,
  include_compact_time_entries?: boolean,
  page?: number,
  order_by?: "id" | "created" | "updated" | "due_date",
  reverse?: boolean,
  detail_level?: "summary" | "detailed"
): Promise<WorkspaceTasksResult>
```

### Time Tracking
```typescript
// Obter entradas de tempo
function mcp_clickup_get_task_time_entries(
  taskId?: string,
  taskName?: string,
  listName?: string,
  startDate?: string,
  endDate?: string
): Promise<TimeEntriesResult>

// Iniciar tracking
function mcp_clickup_start_time_tracking(
  taskId?: string,
  taskName?: string,
  listName?: string,
  description?: string,
  billable?: boolean,
  tags?: string[]
): Promise<TrackingResult>

// Parar tracking
function mcp_clickup_stop_time_tracking(
  description?: string,
  tags?: string[]
): Promise<TrackingResult>

// Adicionar entrada manual
function mcp_clickup_add_time_entry(
  start: string,
  duration: string,
  taskId?: string,
  taskName?: string,
  listName?: string,
  description?: string,
  billable?: boolean,
  tags?: string[]
): Promise<TimeEntryResult>

// Deletar entrada
function mcp_clickup_delete_time_entry(
  timeEntryId: string
): Promise<DeleteResult>

// Obter tracking atual
function mcp_clickup_get_current_time_entry(
  random_string: string
): Promise<CurrentTrackingResult>
```

### Gestão de Listas
```typescript
// Criar lista em space
function mcp_clickup_create_list(
  name: string,
  spaceId?: string,
  spaceName?: string,
  content?: string,
  dueDate?: string,
  priority?: 1 | 2 | 3 | 4,
  assignee?: number,
  status?: string
): Promise<ListCreationResult>

// Criar lista em pasta
function mcp_clickup_create_list_in_folder(
  name: string,
  folderId?: string,
  folderName?: string,
  spaceId?: string,
  spaceName?: string,
  content?: string,
  status?: string
): Promise<ListCreationResult>

// Obter detalhes da lista
function mcp_clickup_get_list(
  listId?: string,
  listName?: string
): Promise<ListDetails>

// Atualizar lista
function mcp_clickup_update_list(
  listId?: string,
  listName?: string,
  name?: string,
  content?: string,
  status?: string
): Promise<ListUpdateResult>

// Deletar lista
function mcp_clickup_delete_list(
  listId?: string,
  listName?: string
): Promise<DeleteResult>
```

### Gestão de Pastas
```typescript
// Criar pasta
function mcp_clickup_create_folder(
  name: string,
  spaceId?: string,
  spaceName?: string,
  override_statuses?: boolean
): Promise<FolderCreationResult>

// Obter pasta
function mcp_clickup_get_folder(
  folderId?: string,
  folderName?: string,
  spaceId?: string,
  spaceName?: string
): Promise<FolderDetails>

// Atualizar pasta
function mcp_clickup_update_folder(
  folderId?: string,
  folderName?: string,
  spaceId?: string,
  spaceName?: string,
  name?: string,
  override_statuses?: boolean
): Promise<FolderUpdateResult>

// Deletar pasta
function mcp_clickup_delete_folder(
  folderId?: string,
  folderName?: string,
  spaceId?: string,
  spaceName?: string
): Promise<DeleteResult>
```

### Gestão de Tags
```typescript
// Obter tags do space
function mcp_clickup_get_space_tags(
  spaceId?: string,
  spaceName?: string
): Promise<TagsList>

// Adicionar tag à task
function mcp_clickup_add_tag_to_task(
  tagName: string,
  taskId?: string,
  taskName?: string,
  customTaskId?: string,
  listName?: string
): Promise<TagResult>

// Remover tag da task
function mcp_clickup_remove_tag_from_task(
  tagName: string,
  taskId?: string,
  taskName?: string,
  customTaskId?: string,
  listName?: string
): Promise<TagResult>
```

### Gestão de Membros
```typescript
// Listar membros do workspace
function mcp_clickup_get_workspace_members(
  random_string: string
): Promise<MembersList>

// Encontrar membro por nome
function mcp_clickup_find_member_by_name(
  nameOrEmail: string
): Promise<MemberResult>

// Resolver assignees para IDs
function mcp_clickup_resolve_assignees(
  assignees: string[]
): Promise<AssigneeResolution>
```

---

## 📚 Context7 MCP (Documentação)

### `mcp_context7_resolve_library_id`
```typescript
function mcp_context7_resolve_library_id(
  libraryName: string
): Promise<LibraryResolution>
```
**Propósito**: Resolução de nomes de bibliotecas para IDs compatíveis com Context7

**Uso obrigatório**: Deve ser chamado antes de `get_library_docs` para obter ID válido

### `mcp_context7_get_library_docs`
```typescript
function mcp_context7_get_library_docs(
  context7CompatibleLibraryID: string, // ex: "/mongodb/docs", "/vercel/next.js"
  tokens?: number, // máximo de tokens (default: 10000)
  topic?: string // foco específico, ex: "hooks", "routing"
): Promise<LibraryDocs>
```
**Propósito**: Obtenção de documentação atualizada de bibliotecas

---

## 🧭 Sequential Thinking MCP (Análise Complexa)

### `mcp_sequential_thinking_sequentialthinking`
```typescript
function mcp_sequential_thinking_sequentialthinking(
  thought: string,
  nextThoughtNeeded: boolean,
  thoughtNumber: number,
  totalThoughts: number,
  isRevision?: boolean,
  revisesThought?: number,
  branchFromThought?: number,
  branchId?: string,
  needsMoreThoughts?: boolean
): Promise<ThinkingResult>
```
**Propósito**: Ferramenta de resolução de problemas dinâmica e reflexiva

**Características únicas**:
- Processo de pensamento adaptável que evolui
- Suporte a revisão e branching de pensamentos
- Geração e verificação de hipóteses
- Controle dinâmico de total de pensamentos

**Quando usar**:
- Problemas complexos multi-etapas
- Planejamento e design com necessidade de revisão
- Análise que pode precisar de correção de curso
- Problemas onde o escopo completo não é claro inicialmente

---

## 💻 Code Understanding MCP (Análise de Repositórios)

### Gestão de Repositórios
```typescript
// Status sem operações
function mcp_code_understanding_get_repo_status(
  repo_path: string,
  branch?: string,
  cache_strategy?: "shared" | "per-branch"
): Promise<RepoStatus>

// Listar repositórios em cache
function mcp_code_understanding_list_repos(
  random_string: string
): Promise<RepoList>

// Listar branches de repositório
function mcp_code_understanding_list_repository_branches(
  repo_url: string
): Promise<BranchList>

// Clonar/inicializar repositório
function mcp_code_understanding_clone_repo(
  url: string,
  branch?: string,
  cache_strategy?: "shared" | "per-branch"
): Promise<CloneResult>

// Atualizar repositório (manual apenas)
function mcp_code_understanding_refresh_repo(
  repo_path: string,
  branch?: string,
  cache_strategy?: string
): Promise<RefreshResult>

// Deletar repositório do cache
function mcp_code_understanding_delete_repo(
  repo_identifier: string
): Promise<DeleteResult>
```

### Análise de Conteúdo
```typescript
// Obter arquivos ou listagem
function mcp_code_understanding_get_repo_file_content(
  repo_path: string,
  resource_path?: string,
  branch?: string,
  cache_strategy?: string
): Promise<FileContent>

// Mapa semântico do código
function mcp_code_understanding_get_source_repo_map(
  repo_path: string,
  max_tokens?: number,
  files?: string[],
  directories?: string[],
  branch?: string,
  cache_strategy?: string
): Promise<RepoMap>

// Estrutura de diretórios
function mcp_code_understanding_get_repo_structure(
  repo_path: string,
  directories?: string[],
  include_files?: boolean,
  branch?: string,
  cache_strategy?: string
): Promise<RepoStructure>

// Arquivos mais importantes
function mcp_code_understanding_get_repo_critical_files(
  repo_path: string,
  directories?: string[],
  files?: string[],
  limit?: number,
  include_metrics?: boolean
): Promise<CriticalFiles>

// Documentação do repositório
function mcp_code_understanding_get_repo_documentation(
  repo_path: string
): Promise<Documentation>
```

**Estratégias de Cache**:
- `shared` (padrão): Um cache por repo, pode alternar branches
- `per-branch`: Cache separado por branch, útil para comparar branches

---

## 🕷️ Firecrawl MCP (Web Scraping)

### Operações Básicas
```typescript
// Scrape de página única
function mcp_firecrawl_scrape(
  url: string,
  formats?: ("markdown" | "html" | "rawHtml" | "screenshot" | "links" | "summary")[],
  maxAge?: number, // cache em ms
  onlyMainContent?: boolean,
  includeTags?: string[],
  excludeTags?: string[],
  waitFor?: number,
  mobile?: boolean,
  actions?: Action[], // click, wait, scroll, etc.
  location?: { country?: string; languages?: string[] },
  removeBase64Images?: boolean,
  skipTlsVerification?: boolean,
  storeInCache?: boolean
): Promise<ScrapeResult>

// Mapear website
function mcp_firecrawl_map(
  url: string,
  search?: string,
  limit?: number,
  includeSubdomains?: boolean,
  ignoreQueryParameters?: boolean,
  sitemap?: "include" | "skip" | "only"
): Promise<UrlMap>

// Busca na web com scraping
function mcp_firecrawl_search(
  query: string,
  limit?: number,
  sources?: { type: "web" | "images" | "news" }[],
  filter?: string,
  location?: string,
  tbs?: string,
  scrapeOptions?: ScrapeOptions
): Promise<SearchResults>
```

### Crawling Avançado
```typescript
// Iniciar crawling
function mcp_firecrawl_crawl(
  url: string,
  limit?: number,
  maxDiscoveryDepth?: number,
  allowExternalLinks?: boolean,
  allowSubdomains?: boolean,
  crawlEntireDomain?: boolean,
  deduplicateSimilarURLs?: boolean,
  delay?: number,
  maxConcurrency?: number,
  includePaths?: string[],
  excludePaths?: string[],
  ignoreQueryParameters?: boolean,
  sitemap?: "skip" | "include" | "only",
  scrapeOptions?: ScrapeOptions,
  prompt?: string,
  webhook?: Webhook
): Promise<CrawlJob>

// Verificar status do crawling
function mcp_firecrawl_check_crawl_status(
  id: string
): Promise<CrawlStatus>

// Extrair dados estruturados
function mcp_firecrawl_extract(
  urls: string[],
  prompt?: string,
  schema?: object,
  allowExternalLinks?: boolean,
  enableWebSearch?: boolean,
  includeSubdomains?: boolean
): Promise<ExtractionResult>
```

**Dica de Performance**: Use `maxAge` para scraping 500% mais rápido com cache

---

## 🖥️ Chrome DevTools MCP (Automação Browser)

### `mcp_chrome-devtools_navigate_page`
```typescript
function mcp_chrome-devtools_navigate_page(url: string): void
```
**Propósito**: Navega para URL específica no browser controlado

### `mcp_chrome-devtools_take_snapshot`
```typescript
function mcp_chrome-devtools_take_snapshot(): PageSnapshot
```
**Propósito**: Captura snapshot textual da página atual com elementos identificados

### `mcp_chrome-devtools_click`
```typescript
function mcp_chrome-devtools_click(uid: string, dblClick?: boolean): void
```
**Propósito**: Clica em elementos específicos da página usando UID

### `mcp_chrome-devtools_fill`
```typescript
function mcp_chrome-devtools_fill(uid: string, value: string): void
```
**Propósito**: Preenche campos de formulário identificados por UID

### `mcp_chrome-devtools_evaluate_script`
```typescript
function mcp_chrome-devtools_evaluate_script(function: string, args?: object[]): any
```
**Propósito**: Executa JavaScript personalizado na página atual

### `mcp_chrome-devtools_take_screenshot`
```typescript
function mcp_chrome-devtools_take_screenshot(uid?: string, fullPage?: boolean): Image
```
**Propósito**: Captura screenshots da página completa ou elementos específicos

**Recursos do Chrome DevTools MCP**:
-  **Automação completa** de browsers Chrome/Chromium
-  **Interação com elementos** via UID únicos
-  **Execução de JavaScript** customizado
-  **Screenshots e snapshots** para debug
-  **Navegação programática** entre páginas
-  **Preenchimento de formulários** automático

**Casos de uso típicos**:
- 🔧 **Testes E2E** automatizados
- 📊 **Scraping inteligente** de dados
- 🔄 **Automação de workflows** web
- 📸 **Documentação visual** de interfaces
- 🧪 **Validação de funcionalidades** web

**Exemplo de uso**:
```bash
# Navegar e capturar informações
mcp_chrome-devtools_navigate_page("https://example.com")
mcp_chrome-devtools_take_snapshot()  # Ver elementos disponíveis
mcp_chrome-devtools_click("button_uid_123")
mcp_chrome-devtools_take_screenshot()  # Capturar resultado
```

**Pré-requisitos**:
-  **Node.js v22.14.0+** instalado
-  **chrome-devtools-mcp@0.4.0+** disponível via npx
-  **Browser Chrome/Chromium** instalado

---

## ⚙️ NX Extension MCP (Framework NX)

### `mcp_extension_nx_docs`
```typescript
function mcp_extension_nx_docs(
  userQuery: string
): Promise<NxDocs>
```
**Propósito**: Obtenção de seções de documentação relevantes do NX

**Uso crítico**: SEMPRE use esta função para perguntas sobre NX. Nunca assuma conhecimento sobre NX pois pode estar desatualizado.

### `mcp_extension_nx_available_plugins`
```typescript
function mcp_extension_nx_available_plugins(
  random_string: string
): Promise<PluginList>
```
**Propósito**: Listagem de plugins disponíveis do NX (core team + workspace local)

---

## 💡 Dicas de Uso das Ferramentas

### **🚀 Para Máxima Performance**
1. **Use ferramentas paralelas**: Execute múltiplas operações read-only simultaneamente
2. **Cache inteligente**: Aproveite `maxAge` no Firecrawl e cache strategies no Code Understanding
3. **Bulk operations**: Prefira operações bulk do ClickUp para múltiplas tasks
4. **Filtros server-side**: Use filtros avançados em `get_workspace_tasks`

### **🎯 Para Precisão**
1. **Busca primeiro**: Use `grep` (regex/símbolos) e `find_path` (globs) para exploração antes de abrir arquivos
2. **IDs sempre preferidos**: Use taskId/listId ao invés de nomes quando possível
3. **Context window otimização**: Ajuste `max_tokens` e `detail_level` conforme necessidade
4. **Validação de estados**: Use `get_repo_status` antes de operações complexas

### **🔄 Para Workflows**
1. **Sequential thinking**: Para problemas complexos que podem mudar de direção
2. **Todo management**: Para rastreamento de progresso em tarefas multi-etapa
3. **Memory persistence**: Para informações importantes que devem persistir entre sessões
4. **Multi-edit atômico**: Para mudanças coordenadas em um arquivo

---

## 📊 Integração Entre Ferramentas

### **Pipeline Típico de Análise de Código**
```mermaid
graph TD
    A[clone_repo] --> B[get_repo_structure]
    B --> C[get_repo_critical_files]
    C --> D[get_source_repo_map]
    D --> E[grep / find_path específicos]
    E --> F[read_file detalhes]
```

### **Workflow de Desenvolvimento com ClickUp**
```mermaid
graph TD
    A[create_task] --> B[start_time_tracking]
    B --> C[edit_file código]
    C --> D[create_task_comment progresso]
    D --> E[update_task status]
    E --> F[stop_time_tracking]
```

### **Pesquisa e Documentação**
```mermaid
graph TD
    A[search_web contexto] --> B[resolve_library_id]
    B --> C[get_library_docs]
    C --> D[write_file documentação]
    D --> E[grava no docs/ persistir]
```

---

---

## 🌿 Skills Git Gitflow

Conjunto completo de skills Git com workflows Gitflow integrados ao Sistema Onion, em `.agents/skills/` e invocáveis por `/onion-git-*`. As skills são Markdown AI-interpretável; a execução real usa a tool nativa `terminal` (git).

### Skills disponíveis
```typescript
// Setup e Ajuda
'/onion-git-help': void;           // Sistema de ajuda interativo + guidance
'/onion-git-init': void;           // Setup Gitflow automático

// Feature Development
'/onion-git-feature-start': (nome: string) => void;    // Criar feature + task no Task Manager
'/onion-git-feature-finish': void;                     // Merge + cleanup automático

// Release Management
'/onion-git-release-start': (version: string) => void; // Release + versionamento
'/onion-git-release-finish': void;                     // Deploy production + tags

// Emergency Hotfix
'/onion-git-hotfix-start': (nome: string) => void;     // Emergency setup < 2h SLA
'/onion-git-hotfix-finish': void;                      // Deploy crítico emergencial

// Workflow Híbrido
'/onion-engineer-hotfix': (desc: string, params?: {
  'related-tasks'?: string;  // "id1,id2,id3"
  'tags'?: string;          // "urgent,critical"
  'status'?: string;        // "In Progress"
  'priority'?: number;      // 1=urgent, 4=low
}) => void;                 // Task no Task Manager + Git workflow completo

// Pós-Merge
'/onion-git-sync': (branch?: string) => void;          // Sincronização automática
```

### Funcionalidades Principais
- **Versionamento Semântico**: Auto-bump patch/minor/major + versões específicas
- **Task Manager Integration**: criação/atualização/comentários via abstração (Jira/ClickUp/Asana/Linear)
- **Master/Main Detection**: Auto-detecção de convenção do repositório
- **Emergency Workflows**: SLA < 2 horas com production-first strategy
- **Session Management**: Integração completa com as skills `/onion-engineer-*`
- **Error Recovery**: Graceful degradation e rollback preparation

### Exemplos de Uso
```bash
# Setup inicial
/onion-git-init

# Feature development
/onion-git-feature-start "oauth-authentication"
/onion-engineer-start oauth-authentication
/onion-git-feature-finish

# Release workflow
/onion-git-release-start "minor"    # 2.0.1 → 2.1.0
# ... testing ...
/onion-git-release-finish

# Emergency hotfix
/onion-engineer-hotfix "Critical payment timeout" --related-tasks="123,456" --tags="urgent"
# ... fix implementation ...
/onion-git-hotfix-finish

# Synchronization
/onion-git-sync develop
```

### Integração Sistema Onion
- **Workflows Completos**: Planejamento → Desenvolvimento → Deploy
- **Task Manager**: tracking automático de progresso e decisões técnicas
- **Session Context**: mantém estado entre skills e sessões via `.agents/onion/sessions/`
- **Specialist Integration**: complementa o specialist `gitflow-specialist` (guidance vs execution), delegado via `spawn_agent`

---

**Sistema Onion** - Desenvolvimento inteligente com IA 🧅 🚀

*Última atualização: 2026-06-03 — port nativo Zed (ADR 0001)*
