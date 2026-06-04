---
name: onion-product-convert-to-tasks
description: Converte documentos consolidados em tasks organizadas hierarquicamente. Use quando o usuário tiver um documento consolidado (reuniões, documentos) ou pasta de consolidações e quiser transformar o conhecimento em tasks acionáveis, delegando a criação para as skills existentes de tasks do Sistema Onion.
disable-model-invocation: true
---

# 🔄 Converter para Tasks

Converte documentos consolidados em tasks usando skills existentes do Sistema Onion.

Parâmetro (lido do prompt do usuário): `source` (arquivo consolidado ou pasta contendo consolidações, obrigatório).

## 🎯 Objetivo

Transformar conhecimento consolidado em tasks acionáveis delegando para skills especializadas:
- Analisa documento consolidado
- Extrai elementos acionáveis
- Cria tasks usando `/onion-product-task` ou `/onion-product-collect`

## ⚡ Fluxo de Execução

### Passo 1: Identificar Arquivos Relevantes (Análise Inteligente)

**SE `{{source}}` é arquivo:**
- Usar arquivo diretamente

**SE `{{source}}` é pasta:**
- Listar todos os arquivos markdown na pasta (recursivo)
- Delegar ao especialista product-agent para identificar arquivos relevantes

Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/product-agent.md` e atue como esse especialista", com o conteúdo:

```markdown
Analise os arquivos abaixo e identifique quais são relevantes para converter em tasks:

**Arquivos na pasta {{source}}:**
{{lista_de_todos_arquivos_markdown_com_paths}}

**Critérios para identificar arquivos relevantes:**
1. **Documentos Consolidados:**
   - Contém seções como "Tarefas", "Decisões", "Gaps", "Insights"
   - Estrutura de consolidação (divergências, convergências, etc)
   - Metadados de consolidação (data, origem, participantes)
   - Formato estruturado de conhecimento consolidado

2. **Arquivos Relacionados (que ajudam a construir contexto):**
   - Análises e estudos relacionados
   - Documentos de evolução e refinamento
   - Especificações e requisitos relacionados
   - Notas e rascunhos que complementam o conhecimento
   - Documentos de contexto de negócio ou técnico relacionados

3. **Evitar Duplicatas:**
   - Não incluir versões antigas de documentos já consolidados
   - Não incluir rascunhos que foram incorporados em documentos finais
   - Priorizar documentos mais recentes e completos
   - Identificar documentos que são evoluções/refinamentos de outros

**Para cada arquivo, forneça:**
- ✅/❌ Relevante para conversão em tasks?
- Tipo: consolidado | análise | especificação | contexto | outro
- Relação com outros arquivos (evolução de X, complementa Y, etc)
- Prioridade de inclusão (alta/média/baixa)
- Justificativa breve

**Output:** Lista de arquivos relevantes ordenados por prioridade, sem duplicatas, com tipo e relação identificados.
```

**Processar arquivos identificados:**
- Ordenar por prioridade (alta → média → baixa)
- Agrupar por tipo e relação
- Carregar conteúdo de todos os arquivos relevantes

### Passo 2: Analisar Arquivos Relevantes com o especialista product-agent

Delegar análise completa considerando todos os arquivos relevantes identificados. Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/product-agent.md` e atue como esse especialista", com o conteúdo:

```markdown
Analise os seguintes arquivos (consolidados e relacionados) e extraia elementos acionáveis organizados hierarquicamente:

**Arquivos Relevantes Identificados:**
{{lista_de_arquivos_relevantes_com_tipos_e_relações}}

**Conteúdo dos Arquivos:**
{{conteudo_de_todos_os_arquivos_relevantes}}

**Contexto:**
- Arquivos consolidados fornecem conhecimento estruturado
- Arquivos relacionados (análises, especificações, contexto) complementam e refinam
- Considerar evolução e refinamento entre documentos relacionados

**Extraia:**
- Tarefas identificadas (com título, descrição, owner, deadline, prioridade)
- Decisões que requerem implementação
- Gaps críticos que bloqueiam progresso
- Insights acionáveis de alto valor
- Hierarquia sugerida (Task → Subtasks)
- Contexto adicional de arquivos relacionados (para enriquecer tasks)

**Output:** Lista estruturada de tasks prontas para criação, considerando contexto completo de todos os arquivos relevantes.
```

### Passo 3: Criar Tasks usando Skills Existentes

Para cada task identificada, usar skills existentes:

**Para tasks estruturadas (com subtasks):**
```markdown
Use /onion-product-task para criar task principal com decomposição hierárquica:
/onion-product-task "{{descricao_completa_da_task}}"
```

**Para tasks simples ou ideias:**
```markdown
Use /onion-product-collect para criar task de ideia/bug:
/onion-product-collect "{{titulo_da_task}}"
```

**Nota:** As skills `/onion-product-task` e `/onion-product-collect` já:
- Detectam Task Manager automaticamente
- Criam tasks e subtasks hierarquicamente
- Estimam story points automaticamente
- Preservam contexto nos comentários

## 📤 Output Esperado

Lista de tasks criadas com IDs e URLs, apresentada de forma clara:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ CONVERSÃO CONCLUÍDA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📄 **Documento:** {{arquivo}}
📊 **Tasks Criadas:** {{total}}

{{lista_de_tasks_com_urls}}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🎯 Casos de Uso

### Após Consolidar Reuniões

```bash
# 1. Consolidar reuniões
/onion-product-consolidate-meetings "docs/meet/sprint-planning/"

# 2. Converter em tasks
/onion-product-convert-to-tasks "docs/meet/consolidation-*.md"
```

### Após Consolidar Documentos

```bash
# 1. Consolidar documentos
/onion-docs-consolidate-documents "docs/business-context/"

# 2. Converter em tasks
/onion-product-convert-to-tasks "docs/consolidated/"
```

## 🔗 Integração

Esta skill delega para:
- **especialista product-agent** - Análise e extração
- **`/onion-product-task`** - Criação de tasks estruturadas
- **`/onion-product-collect`** - Criação de tasks simples

Todas as skills já lidam com:
- Detecção automática de Task Manager
- Criação hierárquica (tasks e subtasks)
- Estimativas automáticas de story points
- Preservação de contexto
