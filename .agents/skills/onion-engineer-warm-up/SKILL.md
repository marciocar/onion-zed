---
name: onion-engineer-warm-up
description: Preparação de contexto técnico e de engenharia. Use ao iniciar desenvolvimento de feature, retornar a trabalho técnico, mudar de contexto técnico ou preparar code review/refatoração. Foca em arquitetura, padrões de código, estrutura do projeto, comandos de desenvolvimento e frameworks técnicos.
disable-model-invocation: true
---

# 🔥 Warm-up de Engenharia

Preparação específica para trabalho técnico e de desenvolvimento.

## 🎯 Objetivo

Estabelecer contexto focado em:
- Arquitetura técnica do projeto
- Padrões de código e convenções
- Estrutura de código e organização
- Comandos e workflows de desenvolvimento
- Frameworks técnicos e ferramentas
- Sistema de testes e validação

## 📋 Checklist de Preparação

### 1. Contexto Geral (Base)
- ✅ Revisar `README.md` para visão geral do Sistema Onion
- ✅ Entender estrutura de documentação em `docs/`

### 2. Meta Especificações Técnicas
- ✅ Revisar `docs/meta-specs/index.md`
- ✅ Focar em:
  - `architecture.md` (quando disponível) - Padrões arquiteturais
  - `code-standards.md` (quando disponível) - Padrões de código
  - `agents.md` (quando disponível) - Padrões para agentes
  - `commands.md` (quando disponível) - Padrões para comandos
- ✅ Entender hierarquia de especificações para decisões técnicas

### 3. Documentação Técnica
- ✅ Revisar `docs/onion/commands-guide.md` - Seção "Comandos de Engenharia"
- ✅ Revisar `docs/onion/engineering-flows.md` - Fluxos de desenvolvimento
- ✅ Revisar `docs/onion/testing-validation-system.md` - Sistema de testes
- ✅ Mapear comandos de engenharia:
  - `/onion-engineer-start` - Iniciar desenvolvimento (valida story points)
  - `/onion-engineer-work` - Trabalhar em feature
  - `/onion-engineer-plan` - Planejar implementação
  - `/onion-engineer-pre-pr` - Preparar Pull Request
  - `/onion-engineer-pr` - Criar Pull Request
  - `/onion-engineer-docs` - Documentar código
  - `/onion-engineer-warm-up` - Este comando

### 4. Estrutura do Projeto
- ✅ Mapear estrutura de diretórios do código
- ✅ Entender organização de módulos/pacotes
- ✅ Identificar tecnologias principais (linguagens, frameworks)
- ✅ Localizar arquivos de configuração importantes

### 5. Padrões de Código
- ✅ Revisar convenções de nomenclatura
- ✅ Entender estilo de código esperado
- ✅ Conhecer ferramentas de linting/formatting
- ✅ Verificar arquivos de configuração (.eslintrc, .prettierrc, etc)

### 6. Knowledge Bases Técnicas
- ✅ Revisar `docs/knowledge-base/concepts/spec-as-code-strategy.md`
- ✅ Revisar `docs/knowledge-base/concepts/ai-agent-design-patterns.md`
- ✅ Revisar `docs/knowledge-base/concepts/abstraction-patterns-catalog.md`
- ✅ Revisar `docs/knowledge-base/frameworks/framework_testes.md`
- ✅ Revisar `docs/knowledge-base/concepts/context-window-optimization.md`

### 7. Agentes de Desenvolvimento
- ✅ Conhecer agentes especializados (delegue via tool `spawn_agent`, lendo `.agents/onion/specialists/<nome>.md`):
  - `react-developer` - Desenvolvimento React
  - `nodejs-specialist` - Backend Node.js/TypeScript
  - `postgres-specialist` - PostgreSQL
  - `nx-monorepo-specialist` - Monorepos NX
  - `docker-specialist` - Docker e containers
  - `c4-architecture-specialist` - Arquitetura C4
  - `mermaid-specialist` - Diagramas Mermaid
  - `test-engineer` - Testes e QA
  - `code-reviewer` - Code review

### 8. Sistema de Testes
- ✅ Revisar `docs/onion/testing-validation-system.md`
- ✅ Entender comandos de teste:
  - `/onion-test-unit` - Testes unitários (White-box)
  - `/onion-test-integration` - Testes de integração (Grey-box)
  - `/onion-test-e2e` - Testes end-to-end (Black-box)
- ✅ Conhecer frameworks de teste utilizados

### 9. Git e Versionamento
- ✅ Revisar comandos Git disponíveis:
  - `/onion-git-feature-start` - Criar branch de feature
  - `/onion-git-sync` - Sincronizar após merge
- ✅ Entender workflow Git do projeto
- ✅ Conhecer convenções de branching

### 10. Validação de Story Points
- ✅ Entender que `/onion-engineer-start` valida estimativas
- ✅ Conhecer processo de validação antes de iniciar desenvolvimento
- ✅ Saber como lidar com tasks sem estimativas

## 🔍 Contexto Específico de Engenharia

### Documentação Essencial
- `docs/onion/engineering-flows.md` - Fluxos de desenvolvimento
- `docs/onion/testing-validation-system.md` - Sistema de testes
- `docs/onion/commands-guide.md` - Comandos de engenharia
- `docs/knowledge-base/frameworks/framework_testes.md` - Framework de testes

### Workflows de Desenvolvimento
1. **Iniciar**: `/onion-engineer-start` → Valida story points, cria branch, sessão
2. **Trabalhar**: `/onion-engineer-work` → Loop de desenvolvimento
3. **Planejar**: `/onion-engineer-plan` → Planejar implementação detalhada
4. **Pre-PR**: `/onion-engineer-pre-pr` → Preparar Pull Request
5. **PR**: `/onion-engineer-pr` → Criar Pull Request (testes, build, PR)
6. **Sync**: `/onion-git-sync` → Sincronizar após merge

### Estrutura de Sessões
- ✅ Entender `.agents/onion/sessions/<feature>/` para contexto de trabalho
- ✅ Conhecer formato de arquivos de sessão

## 💡 Quando Usar Este Warm-up

- ✅ Início de desenvolvimento de feature
- ✅ Retorno a trabalho técnico após período ausente
- ✅ Mudança de contexto técnico (nova tecnologia/framework)
- ✅ Necessidade de entender arquitetura do projeto
- ✅ Preparação para code review ou refatoração

## 🔗 Integração com Produto

- Tasks vêm de `/onion-product-task` com story points
- Especificações vêm de `/onion-product-spec`
- Validação de estimativas antes de iniciar
- Sincronização com Task Manager durante desenvolvimento

## ⚠️ Notas

- Foco em contexto técnico e de código
- Mantenha conhecimento de padrões e convenções no contexto
- Use agentes especializados para tecnologias específicas
- Sempre valide story points antes de iniciar (`/onion-engineer-start`)
- Mantenha código sincronizado com Task Manager durante desenvolvimento
