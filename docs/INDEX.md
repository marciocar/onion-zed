# 📚 Índice Central de Documentação

> **Última atualização**: 2026-06-04 | **Gerado por**: `/onion-docs-build-index` | **Revisado**: auditoria de compatibilidade Zed

Bem-vindo ao índice central de documentação do projeto. Este documento serve como hub de navegação para toda a documentação disponível.

---

## 🎯 Visão Geral

Este projeto é o **Sistema Onion** — um framework **nativo do Zed** (`.agents/` + `.zed/`) para uso interno com:

> 🦓 **Port nativo Zed (2026-06-03)** — o framework foi migrado de Claude Code para os primitivos nativos do Zed. Comandos viraram **skills** (`/onion-<cat>-<cmd>`), agentes viraram **specialists** (delegados via `spawn_agent`). Ver [ADR 0001](meta-specs/adr/0001-zed-native-port.md).

- 🤖 **81 skills invocáveis** (`/onion-<categoria>-<comando>`) — 72 ex-comandos + 5 core (`onion`, `onion-warmup`, `onion-patterns`, `onion-validation`, `language-standards`) + 4 do piloto de engenharia
- 🎯 **49 specialists** em `.agents/onion/specialists/`, delegados via `spawn_agent`
- 🧅 **Skill `onion`** — orquestrador/ponto de entrada com ativação automática
- 🔗 **Task Manager Abstraction** plugável (Jira, ClickUp, Asana, Linear) em `.agents/onion/utils/task-manager/`
- 📚 **Knowledge Bases estruturadas** para consumo por IA
- 🏗️ **Spec as Code Multi-Context** — separação entre business, technical e meta-specs
- ⚙️ **Config nativa Zed** em `.zed/settings.json`; rules em `AGENTS.md`

---

## 📊 Estatísticas da Documentação

### Documentação Principal
- **66 arquivos markdown** em `docs/`
- **11 arquivos** em `docs/onion/` (Sistema Onion)
- **25 arquivos** em `docs/knowledge-base/` (Knowledge Bases)
  - 13 arquivos em `concepts/` (Conceitos fundamentais)
  - 7 arquivos em `frameworks/` (Frameworks e metodologias)
  - 3 arquivos em `tools/` (Ferramentas, incl. Agent Skills)
  - 1 arquivo em `platforms/` (Plataformas)
  - 1 `index.md`
- **1 arquivo** em `docs/meta-specs/` (Meta Especificações)
- Arquivos adicionais em `docs/analysis/`, `docs/plans/`, `docs/business-context/`, `docs/technical-context/`

### Sistema Onion — Estado atual (nativo Zed)
- **81 skills invocáveis** em `.agents/skills/` (catálogo flat `onion-<cat>-<cmd>`)
- **49 specialists** delegáveis via `spawn_agent` em `.agents/onion/specialists/`
- **5 core skills**: `onion`, `onion-warmup`, `onion-patterns`, `onion-validation`, `language-standards`

> **Referência histórica (legado Claude Code):** antes do port nativo Zed (2026-06-03), o sistema tinha 78 comandos em `.claude/commands/` e 49 agentes nomeados em `.claude/agents/`. Esses artefatos foram migrados para skills e specialists. Ver [ADR 0001](meta-specs/adr/0001-zed-native-port.md).

### Total
- **66 arquivos** de documentação markdown
- **81 skills** em `.agents/skills/`
- **49 specialists** em `.agents/onion/specialists/`

---

## 📁 Estrutura de Documentação

```
docs/
├── INDEX.md                    # Este arquivo (hub central)
│
├── onion/                      # Sistema Onion (11 arquivos)
│   ├── index.md                # Índice da seção
│   ├── commands-guide.md       # Guia completo de comandos
│   ├── agents-reference.md     # Referência de agentes
│   ├── engineering-flows.md    # Fluxos de engenharia
│   ├── practical-examples.md   # Exemplos práticos
│   ├── getting-started.md      # Configuração inicial
│   ├── testing-validation-system.md  # Sistema de testes e validação
│   ├── tools-reference.md      # Referência de ferramentas
│   ├── claude-code-commands-architecture.md  # Arquitetura de comandos
│   ├── end-to-end-validation-tests.md  # Testes de validação E2E
│   └── sistema-engenharia-reversa-guia-uso.md  # Engenharia reversa
│
├── knowledge-base/             # Knowledge Bases (25 arquivos)
│   ├── concepts/               # Conceitos fundamentais (13 arquivos)
│   │   ├── abstraction-patterns-catalog.md
│   │   ├── ai-agent-design-patterns.md
│   │   ├── branding-posicionamento-marca.md
│   │   ├── configuration-management.md
│   │   ├── context-window-optimization.md
│   │   ├── framework_story_points.md
│   │   ├── framework_testes.md
│   │   ├── identificar-precificar-dor-cliente.md
│   │   ├── meeting-transcription-to-knowledge-base.md
│   │   ├── spec-as-code-strategy.md
│   │   ├── spec-driven-development.md  # ✨ NOVO
│   │   ├── specification-driven-ai-abstraction-layer.md
│   │   └── task-manager-abstraction.md
│   ├── frameworks/             # Frameworks e metodologias (7 arquivos)
│   │   ├── framework_story_points.md
│   │   ├── framework_testes.md
│   │   ├── onion-complete-cycle-understanding.md
│   │   ├── onion-ide-integration-strategy.md
│   │   ├── onion-multi-context-orchestrator-vision.md
│   │   ├── onion-system-critical-analysis-2025.md
│   │   └── spec-driven-development-tools-2025.md
│   ├── platforms/              # Plataformas e tecnologias (1 arquivo)
│   │   └── runflow.md
│   ├── providers/              # Provedores de serviços (1 arquivo)
│   │   └── microsoft-graph-teams-api-guia-completo.md
│   └── tools/                  # Ferramentas e recursos (2 arquivos)
│       ├── claude-code-commands-best-practices-2025.md
│       └── whisper.md          # Knowledge base do Whisper
│
├── meta-specs/                 # Meta Especificações (1 arquivo)
│   └── index.md                # Índice de meta specs
│
├── analysis/                   # Análises
│   └── unleash-alternatives-analysis.md
│
├── plans/                      # Planos de execução
│   └── [arquivos de planejamento]
│
├── sdaal/                      # Specification-Driven AI Abstraction Layer
│   └── [documentação SDAAL]
│
└── tools/                      # Ferramentas e recursos
    └── [documentação de ferramentas]
```

---

## 🧅 Sistema Onion

### 📖 Documentação Principal

#### Guias Essenciais
- **[Guia de Comandos](onion/commands-guide.md)** - Documentação completa de todos os comandos disponíveis
- **[Referência de Agentes](onion/agents-reference.md)** - Lista e descrição de todos os agentes especializados
- **[Fluxos de Engenharia](onion/engineering-flows.md)** - Workflows detalhados para desenvolvimento
- **[Sistema de Testes e Validação](onion/testing-validation-system.md)** - Framework completo de testes e validação

#### Integrações e Configuração
- **[Configuração Inicial](onion/getting-started.md)** - Setup completo do sistema
- **[Guias de Aplicação](applying/README.md)** - Aplicar o Onion em projetos novos, legados ou regulados
- Integração com Task Manager (Jira/ClickUp/Asana/Linear): use `/meta:setup-integration` — adapters em `.claude/utils/task-manager/adapters/`

#### Referências Técnicas
- **[Exemplos Práticos](onion/practical-examples.md)** - Casos de uso reais com exemplos
- **[Referência de Ferramentas](onion/tools-reference.md)** - Todas as ferramentas disponíveis
- **[Arquitetura de Comandos](onion/claude-code-commands-architecture.md)** - Estrutura interna dos comandos

#### Documentação Avançada
- **[Testes de Validação E2E](onion/end-to-end-validation-tests.md)** - Testes end-to-end do sistema
- **[Guia de Engenharia Reversa](onion/sistema-engenharia-reversa-guia-uso.md)** - Engenharia reversa de projetos

### 🚀 Início Rápido

**Novo no sistema?** Comece aqui:

1. **[Configuração Inicial](onion/getting-started.md)** - Setup do ambiente
2. **[Guia de Comandos](onion/commands-guide.md)** - Aprenda os comandos principais
3. **[Exemplos Práticos](onion/practical-examples.md)** - Veja casos de uso reais

**Comando de entrada:**
```bash
/onion "Sou novo aqui, me ajude a começar"
```

---

## 📚 Knowledge Bases

Knowledge Bases estruturadas para consumo por IA e referência técnica:

### Conceitos Fundamentais (13 arquivos)
- **Task Manager Abstraction** - Abstração de gerenciadores de tarefas
- **Framework de Story Points** - Sistema de estimativas ágeis
- **Framework de Testes** - Metodologias de teste completas
- **Spec-as-Code Strategy** - Estratégia de especificações como código
- **Spec-Driven Development** - Metodologia emergente de desenvolvimento com IA ✨ NOVO
- **AI Agent Design Patterns** - Padrões de design para agentes IA
- **Abstraction Patterns Catalog** - Catálogo de padrões de abstração
- **Context Window Optimization** - Otimização de contexto para IA
- **Configuration Management** - Gestão de configurações
- **Branding e Posicionamento** - Estratégias de marca
- **Identificar e Precificar Dor do Cliente** - Metodologias de produto
- **Meeting Transcription to Knowledge Base** - Processamento de reuniões
- **Specification-Driven AI Abstraction Layer** - Camada de abstração orientada a especificações

### Frameworks e Metodologias (7 arquivos)
- **Framework de Story Points** - Estimativas ágeis
- **Framework de Testes** - White-box, Grey-box, Black-box
- **Onion Complete Cycle Understanding** - Sistema completo de 5 camadas
- **Onion IDE Integration Strategy** - Estratégia multi-IDE
- **Onion Multi-Context Orchestrator Vision** - Visão arquitetural
- **Onion System Critical Analysis 2025** - Análise crítica do sistema
- **Spec-Driven Development Tools 2025** - Ferramentas e análise

### Plataformas e Tecnologias (1 arquivo)
- **Runflow** - Documentação da plataforma

### Provedores de Serviços (1 arquivo)
- **Microsoft Graph Teams API** - Guia completo de integração

### Ferramentas (3 arquivos)
- **Claude Code Commands Best Practices 2025** - Boas práticas de comandos Claude Code
- **Whisper** - Sistema de transcrição de áudio (OpenAI)
- **Zed** ✨ NOVO - Editor de código open-source em Rust com IA integrada, MCP, Skills e External Agents

**Localização:** `docs/knowledge-base/`

---

## 🏗️ Meta Especificações

Especificações de nível mais alto que servem como "constituição" do Sistema Onion. **As 5 meta-specs L0 foram reescritas em 2026-06-03 para Zed** (v2.0.0). Validação via `spawn_agent` → `specialists/metaspec-gate-keeper.md`:

- **[Índice de Meta Specs](meta-specs/index.md)** - Visão geral das meta especificações
- **[agents.md](meta-specs/agents.md)** ✨ NOVO - Padrões obrigatórios para agentes (YAML, categorias, naming, limites)
- **[commands.md](meta-specs/commands.md)** ✨ NOVO - Padrões para comandos + **workflows faseados como invariante**
- **[architecture.md](meta-specs/architecture.md)** ✨ NOVO - Estrutura de diretórios, framework instalável, dependências
- **[code-standards.md](meta-specs/code-standards.md)** ✨ NOVO - Idioma, formatação, naming, estilo
- **[integrations.md](meta-specs/integrations.md)** ✨ NOVO - Task Manager Abstraction como referência canônica, padrão de adapter, MCPs

**Localização:** `docs/meta-specs/`

---

## 🚀 Aplicação em Projetos-alvo

Guias de aplicação do Onion em projetos novos, legados ou regulados:

- **[Guias de Aplicação — README](applying/README.md)** ✨ NOVO - Visão geral e árvore de decisão
- **[Onion em Projeto Novo (Greenfield)](applying/applying-greenfield.md)** ✨ NOVO - Passo a passo desde `git init`
- **[Onion em Projeto Legado](applying/applying-legacy.md)** ✨ NOVO - Engenharia reversa + migração gradual
- **[Onion em Projeto Regulado](applying/applying-regulated.md)** ✨ NOVO - ISO 27001, ISO 22301, SOC2, PMBOK

---

## 📊 Análises e Planos

### Análises
- **[Revisão Analítica do Sistema Onion — Maio/2026](analysis/onion-review-2026-05.md)** ✨ NOVO - Análise crítica completa sob a lente "framework template instalável"; documenta abandono de `.onion/`, plano v4.0 e `packages/onion-cli/`
- **[Retrospectiva T2.6 — Validação das Meta-specs](analysis/metaspec-validation-2026-05-18.md)** ✨ NOVO - Validação das 5 meta-specs contra artefatos reais
- **[Retrospectiva P1 — Saneamento Estrutural](analysis/p1-saneamento-retrospectiva-2026-05-18.md)** ✨ NOVO - Decisões registradas das tarefas T1.1-T1.5
- **[Baseline de Verificação e Validação — Junho/2026](analysis/onion-vv-baseline-2026-06.md)** ✨ NOVO - Estado "antes" (tamanhos + conformidade de plataforma) do plano de V&V
- **[Retrospectiva T3.2 — Validação de build-*-docs](analysis/t32-pilot-retrospectiva-2026-06.md)** ✨ NOVO - Auto-piloto que validou os comandos de geração de docs; resolve T3.6
- **[Análise de Alternativas Unleash](analysis/unleash-alternatives-analysis.md)** - Análise comparativa

### Planos de Execução
- **[Plano de Saneamento Onion 2026-05](plans/onion-saneamento-plan-2026-05.md)** - Roadmap executável das 19 recomendações da análise de maio/2026 (status: executado-parcialmente)

> Os planos `onion-v4-epic` e `onion-v4-migration-plan` (visão v4.0/CLI) foram **abandonados em 2026-05-18** e removidos em 2026-06-03 (recuperáveis via histórico git).

---

## 🧭 Navegação por Perfil

### 👨‍💻 Para Desenvolvedores

**Comece com:**
1. [Configuração Inicial](onion/getting-started.md)
2. [Guia de Comandos](onion/commands-guide.md) - Seção "Comandos de Engenharia"
3. [Fluxos de Engenharia](onion/engineering-flows.md)
4. [Sistema de Testes e Validação](onion/testing-validation-system.md)

**Skills essenciais:**
- `/onion-engineer-start` — iniciar desenvolvimento
- `/onion-engineer-work` — trabalhar em feature
- `/onion-engineer-pr` — criar Pull Request
- `/onion-test-unit` — testes unitários
- `/onion-test-integration` — testes de integração

**Specialists (delegue via `spawn_agent`):**
- `react-developer` — desenvolvimento React
- `nodejs-specialist` — backend Node.js
- `nx-monorepo-specialist` — monorepos NX
- `c4-architecture-specialist` — arquitetura C4
- `whisper-specialist` — transcrição de áudio com Whisper

### 📋 Para Product Owners

**Comece com:**
1. [Guia de Comandos](onion/commands-guide.md) - Seção "Comandos de Produto"
2. [Exemplos Práticos](onion/practical-examples.md)
3. [Knowledge Base - Story Points](knowledge-base/frameworks/framework_story_points.md)
4. [Knowledge Base - Spec-Driven Development](knowledge-base/concepts/spec-driven-development.md) ✨ NOVO

**Skills essenciais:**
- `/onion-product-task` — criar tasks estruturadas
- `/onion-product-spec` — especificações técnicas
- `/onion-product-estimate` — estimar story points
- `/onion-product-extract-meeting` — extrair insights de reuniões
- `/onion-product-whisper` — facilitador para uso do Whisper
- `/onion-docs-consolidate-documents` — consolidar múltiplos documentos
- `/onion-validate-collab-three-amigos` — sessões colaborativas

**Specialists (delegue via `spawn_agent`):**
- `product-agent` — orquestração de produto
- `story-points-framework-specialist` — estimativas ágeis
- `storytelling-business-specialist` — narrativas de negócio
- `branding-positioning-specialist` — branding e posicionamento
- `extract-meeting-specialist` — extração de reuniões
- `meeting-consolidator` — consolidação de reuniões

### 🧪 Para QA/Test Engineers

**Comece com:**
1. [Sistema de Testes e Validação](onion/testing-validation-system.md)
2. [Framework de Testes](knowledge-base/frameworks/framework_testes.md)
3. [Guia de Comandos](onion/commands-guide.md) - Seção "Comandos de Validação"

**Skills essenciais:**
- `/onion-test-unit` — testes unitários (White-box)
- `/onion-test-integration` — testes de integração (Grey-box)
- `/onion-test-e2e` — testes end-to-end (Black-box)
- `/onion-validate-test-strategy-create` — criar estratégias de teste
- `/onion-validate-qa-points-estimate` — estimar QA points
- `/onion-validate-collab-pair-testing` — teste em par

**Specialists (delegue via `spawn_agent`):**
- `test-agent` — estratégias completas de teste
- `test-engineer` — implementação prática
- `test-planner` — planejamento e cobertura

### 🏗️ Para Arquitetos

**Comece com:**
1. [Arquitetura de Comandos](onion/claude-code-commands-architecture.md)
2. [Meta Especificações](meta-specs/index.md)
3. [Revisão Analítica do Sistema Onion — Maio/2026](analysis/onion-review-2026-05.md)

**Recursos:**
- Specialists de arquitetura: `c4-architecture-specialist`, `mermaid-specialist` (via `spawn_agent`)
- Skills de documentação: `/onion-docs-build-tech-docs`, `/onion-docs-reverse-consolidate`
- Knowledge Bases: [SDAAL](knowledge-base/concepts/specification-driven-ai-abstraction-layer.md), [Spec-Driven Development](knowledge-base/concepts/spec-driven-development.md) ✨ NOVO

### 🔧 Para Administradores do Sistema

**Comece com:**
1. [Configuração Inicial](onion/getting-started.md)
2. [Guias de Aplicação](applying/README.md)
3. [Referência de Ferramentas](onion/tools-reference.md)

**Skills essenciais:**
- `/onion-meta-setup-integration` — configurar Task Manager (Jira/ClickUp/Asana/Linear) e demais integrações
- `/onion-meta-all-tools` — listar todas as ferramentas
- `/onion-docs-build-index` — reconstruir índices

### 🛡️ Para Compliance/Security

**Comece com:**
1. [Agentes de Compliance](onion/agents-reference.md#️-agentes-de-compliance)
2. [Comandos de Validação](onion/commands-guide.md#-comandos-de-validação)

**Specialists (delegue via `spawn_agent`):**
- `iso-27001-specialist` — ISO 27001:2022
- `iso-22301-specialist` — ISO 22301:2019
- `soc2-specialist` — SOC2 Type II
- `security-information-master` — segurança da informação
- `corporate-compliance-specialist` — compliance corporativo

---

## 🗺️ Mapa de Navegação Rápida

### Por Tipo de Documento

| Tipo | Localização | Descrição |
|------|-------------|-----------|
| 📖 **Guias** | `docs/onion/` | Guias de uso e referência |
| 📚 **Knowledge Bases** | `docs/knowledge-base/` | Conhecimento estruturado para IA |
| 🏗️ **Meta Specs** | `docs/meta-specs/` | Especificações de alto nível |
| 📊 **Análises** | `docs/analysis/` | Análises e estudos |
| 📋 **Planos** | `docs/plans/` | Planos de execução |
| 🔧 **SDAAL** | `docs/sdaal/` | Specification-Driven AI Abstraction Layer |

### Por Categoria de Comando

| Categoria | Skills | Documentação |
|-----------|--------|--------------|
| 🔧 **Engenharia** | `/onion-engineer-*` | [Guia de Comandos](onion/commands-guide.md) |
| 📋 **Produto** | `/onion-product-*` | [Guia de Comandos](onion/commands-guide.md) |
| 🧪 **Testes** | `/onion-test-*` | [Sistema de Testes](onion/testing-validation-system.md) |
| ✅ **Validação** | `/onion-validate-*` | [Sistema de Testes](onion/testing-validation-system.md) |
| 📚 **Documentação** | `/onion-docs-*` | [Guia de Comandos](onion/commands-guide.md) |
| 🌿 **Git** | `/onion-git-*` | [Guia de Comandos](onion/commands-guide.md) |
| ⚙️ **Meta** | `/onion-meta-*` | [Guia de Comandos](onion/commands-guide.md) |
| 🧅 **Core** | `/onion`, `/onion-warmup` | [Sistema Onion](onion/) |
| ⚡ **Quick / Dev** | `/onion-quick-*`, `/onion-development-*` | [Guia de Comandos](onion/commands-guide.md) |

### Por Specialist

| Família | Função | Exemplos de slug |
|---------|--------|------------------|
| **Desenvolvimento** | Stack técnica | `react-developer`, `nodejs-specialist`, `postgres-specialist` |
| **Produto** | Discovery e spec | `product-agent`, `task-specialist`, `extract-meeting-specialist` |
| **Compliance** | Frameworks regulatórios | `iso-27001-specialist`, `soc2-specialist`, `pmbok-specialist` |
| **Meta** | Orquestração e criação | `onion`, `metaspec-gate-keeper`, `agent-creator-specialist` |
| **Git** | GitFlow e review | `gitflow-specialist`, `branch-code-reviewer` |
| **Testes** | Estratégia e implementação | `test-agent`, `test-engineer`, `test-planner` |
| **Task Manager** | Provider ativo | `jira-specialist`, `clickup-specialist` |

> Delegue via `spawn_agent` → `"Leia .agents/onion/specialists/<slug>.md e atue como esse especialista para: <tarefa>"`

---

## 🔗 Links Rápidos

### Documentação Essencial
- [README Principal](../README.md) - Visão geral do Sistema Onion
- [Guia de Comandos](onion/commands-guide.md) - Todos os comandos
- [Referência de Agentes](onion/agents-reference.md) - Todos os agentes
- [Sistema de Testes e Validação](onion/testing-validation-system.md) - Framework completo
- [Guias de Aplicação](applying/README.md) - Onion em projetos novos, legados ou regulados

### Knowledge Bases
- [Task Manager Abstraction](knowledge-base/concepts/task-manager-abstraction.md)
- [Framework de Story Points](knowledge-base/frameworks/framework_story_points.md)
- [Framework de Testes](knowledge-base/frameworks/framework_testes.md)
- [AI Agent Design Patterns](knowledge-base/concepts/ai-agent-design-patterns.md)
- [Spec-as-Code Strategy](knowledge-base/concepts/spec-as-code-strategy.md)
- [Spec-Driven Development](knowledge-base/concepts/spec-driven-development.md) ✨ NOVO
- [Whisper](knowledge-base/tools/whisper.md) - Transcrição de áudio
- [Zed](knowledge-base/tools/zed.md) ✨ NOVO - Editor IA-first (Rust, MCP, Skills, External Agents)

### Configuração
- [Configuração Inicial](onion/getting-started.md)
- [Guias de Aplicação](applying/README.md)
- [Adapters de Task Manager](../.agents/onion/utils/task-manager/adapters/) (Jira, ClickUp, Asana, Linear)

---

## 🆕 Novidades

### ✨ Documentação Adicionada Recentemente

- **[Spec-Driven Development](knowledge-base/concepts/spec-driven-development.md)** (2025-12-02)
  - Knowledge base completa sobre metodologia emergente
  - Análise de ferramentas (Kiro, Spec-Kit, Tessl)
  - Níveis de implementação (Spec-First, Spec-Anchored, Spec-as-Source)
  - Comparação com TDD, BDD, MDD
  - Benefícios e desafios

- **[Sistema de Testes e Validação](onion/testing-validation-system.md)** (2025-12-02)
  - Framework completo de testes e validação
  - 4 camadas integradas (Knowledge Base, Agentes, Comandos de Teste, Comandos de Validação)
  - Guia completo para desenvolvedores, QA e times cross-funcionais

- **Comandos de Produto Expandidos**
  - `/product/extract-meeting` - Extração inteligente de insights de reuniões
  - `/product/consolidate-meetings` - Consolidação de múltiplas reuniões
  - `/product/convert-to-tasks` - Converter documentos consolidados em tasks
  - `/product/whisper` - Facilitador para uso do Whisper
  - `/docs/consolidate-documents` - Consolidar múltiplos documentos
  - Agente `@meeting-consolidator` - Consolidação avançada de reuniões
  - Agente `@whisper-specialist` - Especialista em transcrição de áudio
  - Knowledge Base Whisper - Documentação completa do Whisper

---

## 📞 Suporte e Recursos

### 🆘 Resolução de Problemas

1. **Comandos**: Consulte [Guia de Comandos](onion/commands-guide.md)
2. **Exemplos**: Veja casos práticos em [Exemplos Práticos](onion/practical-examples.md)
3. **Configuração**: Siga [Configuração Inicial](onion/getting-started.md)
4. **Aplicação em projetos**: Consulte [Guias de Aplicação](applying/README.md)
5. **Testes**: Consulte [Sistema de Testes e Validação](onion/testing-validation-system.md)

### 🔧 Comandos de Debug

```bash
/onion "ajuda"                  # Skill orquestradora (ponto de entrada)
/onion-meta-all-tools           # Lista todas as ferramentas nativas Zed
/onion-docs-build-index         # Reconstruir este índice
/onion-warmup                   # Warm-up geral do projeto
```

---

## 🔄 Manutenção

Este índice é gerado automaticamente pelo comando `/docs/build-index`.

**Para atualizar:**
```bash
/docs/build-index              # Reconstruir índice principal
/docs/build-index onion        # Reconstruir índice da seção onion
```

**Última atualização:** 2026-05-15
**Mantido por:** Sistema Onion

---

**Sistema Onion** - Multi-Context Development Orchestrator 🧅
