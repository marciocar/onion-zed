---
name: onion-product-warm-up
description: Preparação de contexto de produto e negócio. Use quando precisar carregar contexto focado em documentação de produto, especificações de features, knowledge bases de negócio e frameworks de produto (Story Points), antes de trabalhar com tasks, reuniões, estimativas ou análise de requisitos.
disable-model-invocation: true
---

# 🔥 Warm-up de Produto

Preparação específica para trabalho de produto, negócio e gestão de features.

## 🎯 Objetivo

Estabelecer contexto focado em:
- Documentação de produto e negócio
- Especificações de features e domínios
- Knowledge bases de produto
- Frameworks de produto (Story Points, etc)
- Comandos e agentes de produto

## 📋 Checklist de Preparação

### 1. Contexto Geral (Base)
- ✅ Revisar `README.md` para visão geral do Sistema Onion
- ✅ Entender estrutura de documentação em `docs/`

### 2. Meta Especificações
- ✅ Revisar `docs/meta-specs/index.md`
- ✅ Memorizar hierarquia de especificações
- ✅ Entender quando consultar meta specs para decisões de produto

### 3. Documentação de Produto
- ✅ Revisar `docs/onion/commands-guide.md` - Seção "Comandos de Produto"
- ✅ Mapear comandos de produto disponíveis:

**Gestão de Tasks:**
- `/onion-product-task` - Criar tasks com estimativas automáticas
- `/onion-product-feature` - Criar tasks de feature para backlog
- `/onion-product-collect` - Coletar ideias de features/bugs
- `/onion-product-task-check` - Verificar status de tasks
- `/onion-product-validate-task` - Validar tasks contra meta-specs
- `/onion-product-checklist-sync` - Sincronizar checklists

**Especificação e Refinamento:**
- `/onion-product-spec` - Especificações técnicas
- `/onion-product-refine` - Refinamento de requisitos
- `/onion-product-estimate` - Estimar story points
- `/onion-product-light-arch` - Arquitetura leve

**Processamento de Reuniões:**
- `/onion-product-extract-meeting` - Extrair insights de reuniões (Framework EXTRACT)
- `/onion-product-consolidate-meetings` - Consolidar múltiplas reuniões

**Análise e Validação:**
- `/onion-product-check` - Verificar requisitos contra meta-specs
- `/onion-product-analyze-pain-price` - Analisar dor do cliente e precificação

**Comunicação:**
- `/onion-product-branding` - Trabalhar em branding e posicionamento
- `/onion-product-presentation` - Criar apresentações

**Documentação Relacionada:**
- `/onion-docs-consolidate-documents` - Consolidar documentos de produto/negócio

### 4. Knowledge Bases de Produto
- ✅ Revisar `docs/knowledge-base/frameworks/framework_story_points.md`
- ✅ Revisar `docs/knowledge-base/concepts/task-manager-abstraction.md`
- ✅ Revisar `docs/knowledge-base/concepts/meeting-transcription-to-knowledge-base.md`
- ✅ Revisar `docs/knowledge-base/concepts/identificar-precificar-dor-cliente.md`
- ✅ Revisar `docs/knowledge-base/concepts/branding-posicionamento-marca.md`

### 5. Agentes de Produto
- ✅ Conhecer agentes especializados:
  - `.agents/onion/specialists/product-agent.md` - Orquestração estratégica
  - `.agents/onion/specialists/story-points-framework-specialist.md` - Estimativas ágeis
  - `.agents/onion/specialists/extract-meeting-specialist.md` - Extração de reuniões
  - `.agents/onion/specialists/meeting-consolidator.md` - Consolidação de reuniões
  - `.agents/onion/specialists/storytelling-business-specialist.md` - Narrativas de negócio
  - `.agents/onion/specialists/branding-positioning-specialist.md` - Branding e posicionamento

### 6. Task Manager Integration
- ✅ Verificar `TASK_MANAGER_PROVIDER` no `.env`
- ✅ Entender abstração de Task Manager (ClickUp, Asana, Linear)
- ✅ Revisar `docs/knowledge-base/concepts/task-manager-abstraction.md`

### 7. Especificações de Features
- ✅ Mapear estrutura de especificações:
  - Domain Specs (L1) - Regras de negócio
  - Feature Specs (L2) - Especificações de features
- ✅ Entender formato de especificações do projeto

## 🔍 Contexto Específico de Produto

### Documentação Essencial
- `docs/onion/commands-guide.md` - Comandos de produto
- `docs/onion/practical-examples.md` - Exemplos práticos
- `docs/knowledge-base/frameworks/framework_story_points.md` - Framework de estimativas
- `docs/knowledge-base/concepts/meeting-transcription-to-knowledge-base.md` - Processamento de reuniões

### Workflows de Produto

**Workflow Completo de Feature:**
1. **Coletar**: `/onion-product-collect` → Coletar ideias de features/bugs
2. **Criar Task**: `/onion-product-task` → Cria com story points automáticos
3. **Criar Feature**: `/onion-product-feature` → Criar task de feature para backlog
4. **Validar**: `/onion-product-check` → Verificar requisitos contra meta-specs
5. **Especificar**: `/onion-product-spec` → Documenta feature completa
6. **Estimar**: `/onion-product-estimate` → Ajusta estimativas
7. **Refinar**: `/onion-product-refine` → Recalcula estimativas após mudanças
8. **Arquitetura**: `/onion-product-light-arch` → Arquitetura leve da feature

**Workflow de Reuniões:**
1. **Extrair Reunião**: `/onion-product-extract-meeting` → Framework EXTRACT (7 dimensões)
2. **Consolidar**: `/onion-product-consolidate-meetings` → Análise de múltiplas reuniões
3. **Consolidar Docs**: `/onion-docs-consolidate-documents` → Consolidar documentos relacionados

**Workflow de Validação:**
1. **Validar Task**: `/onion-product-validate-task` → Validar task contra meta-specs
2. **Verificar**: `/onion-product-task-check` → Verificar status e completude
3. **Sincronizar**: `/onion-product-checklist-sync` → Sincronizar checklists com Task Manager

**Workflow de Análise:**
1. **Analisar Dor**: `/onion-product-analyze-pain-price` → Analisar dor do cliente e precificação
2. **Branding**: `/onion-product-branding` → Trabalhar em branding e posicionamento
3. **Apresentação**: `/onion-product-presentation` → Criar apresentações

## 💡 Quando Usar Este Warm-up

- ✅ Trabalho em especificações de features
- ✅ Criação ou refinamento de tasks (`/onion-product-task`, `/onion-product-feature`, `/onion-product-collect`)
- ✅ Estimativas de story points (`/onion-product-estimate`)
- ✅ Processamento de reuniões (`/onion-product-extract-meeting`, `/onion-product-consolidate-meetings`)
- ✅ Consolidação de documentos (`/onion-docs-consolidate-documents`)
- ✅ Análise de requisitos de negócio (`/onion-product-check`, `/onion-product-validate-task`)
- ✅ Análise de dor do cliente (`/onion-product-analyze-pain-price`)
- ✅ Trabalho com Product Owners
- ✅ Branding e posicionamento (`/onion-product-branding`)
- ✅ Criação de apresentações (`/onion-product-presentation`)

## 🔗 Integração com Engenharia

Após preparar contexto de produto:
- Tasks criadas são validadas por `/onion-engineer-start`
- Story points são verificados antes de iniciar desenvolvimento
- Especificações alimentam sessões de engenharia

## ⚠️ Notas

- Foco em contexto de negócio e produto, não técnico
- Mantenha conhecimento de frameworks de produto no contexto
- Use agentes especializados para tarefas específicas
- Sempre sincronize tasks com Task Manager configurado
