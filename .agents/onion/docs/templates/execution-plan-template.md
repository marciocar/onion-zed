---
# Claude Code - Execution Plan Template Metadata
template:
  type: execution-plan
  version: 2.0
  category: implementation
  name: "[TIPO DE IMPLEMENTAÇÃO]"
  
context:
  title: "🤖 INSTRUÇÕES PARA [IMPLEMENTAÇÃO/EXECUÇÃO/FINALIZAÇÃO]"
  system: "[NOME DO SISTEMA/COMPONENTE]"
  current_state: "[DESCRIÇÃO DO ESTADO ATUAL - ex: ARQUITETURA 85% IMPLEMENTADA]"
  base: "[TECNOLOGIAS/COMPONENTES JÁ FUNCIONANDO]"
  last_update: "[DATA/HORA]"

usage:
  instructions:
    - "✅ MARQUE checkboxes [ ] → [x] conforme progresso nas tarefas"
    - "📝 ATUALIZE o log de execução a cada sessão de implementação"
    - "📅 PREENCHA datas, responsável e observações detalhadas"
    - "🔄 EXECUTE comandos listados na seção 'Comandos de Execução'"
    - "🏷️ CRIE tags git para pontos de rollback de emergência"
    - "💰 MONITORE custos [se aplicável] e performance durante implementação"
    - "🔄 ATUALIZE as marcações de status de cada fase no documento ao final de cada etapa"

standards:
  task_status:
    completed: "[x] - Tarefa CONCLUÍDA com sucesso e validada"
    pending: "[ ] - Tarefa PENDENTE ou não iniciada"
    error: "❌ - Tarefa com ERRO crítico (documentar no log)"
    partial: "⚠️ - Tarefa PARCIAL ou com ressalvas importantes"
    in_progress: "🔄 - Tarefa EM PROGRESSO (marcar data de início)"
  
  commit_patterns:
    feature: "feat([módulo]): implementa [funcionalidade principal]"
    addition: "feat([área]): adiciona [componente/sistema específico]"
    fix: "fix([módulo]): corrige [problema específico]"
    docs: "docs([plano]): atualiza documentação fase X"
    refactor: "refactor([área]): [tipo de refatoração]"
  
  essential_commands:
    package_manager: "📦 SEMPRE usar [gerenciador de pacotes]: [comandos principais]"
    main: "[Comando principal]: [descrição]"
    test: "[Comando de teste]: [descrição]"
    validation: "[Comando de validação]: [descrição]"
    build: "[Comando de build]: [descrição]"

critical_attention:
  warnings:
    - "⚠️ SEMPRE mantenha [sistema principal] funcionando durante implementação"
    - "⚠️ USE feature flags para rollback instantâneo"
    - "⚠️ TESTE cada fase completamente antes de prosseguir"
    - "⚠️ DOCUMENTE incompatibilidades no log imediatamente"
    - "⚠️ MONITORE [métricas críticas] com dados reais"
    - "⚠️ VALIDE [critérios de qualidade] em cada transição"
    - "⚠️ MANTENHA backup dos componentes funcionais"

milestones:
  phase_1: "🎯 [Primeiro marco importante]"
  phase_2: "🎯 [Segundo marco importante]"
  phase_3: "🎯 [Terceiro marco importante]"
  phase_n: "🎯 [Marco final]"

success_metrics:
  - metric: "📊 [Métrica 1]"
    target: "[Target numérico]"
    criteria: "[critério de medição]"
  - metric: "🤖 [Métrica 2]"
    target: "[Target percentual]"
    criteria: "[critério de medição]"
  - metric: "🏷️ [Métrica 3]"
    target: "[Target qualitativo]"
    criteria: "[critério de medição]"
  - metric: "⚡ [Métrica 4]"
    target: "[Target de performance]"
    criteria: "[critério de medição]"
  - metric: "🔒 [Métrica 5]"
    target: "[Target de segurança/qualidade]"
    criteria: "[critério de medição]"
  - metric: "🧪 [Métrica 6]"
    target: "[Target de testes]"
    criteria: "[critério de medição]"

ai_assistant:
  auto_update: true
  track_progress: true
  log_execution: true
  validate_metrics: true
---

# Plano de [Tipo]: [Nome do Sistema/Componente]
## [Subtítulo descritivo do objetivo]

**Documento de Controle e Execução [Final/Implementação/Migração]**  
**Objetivo:** [Descrição clara do objetivo principal]  
**Data de Início:** [YYYY-MM-DD HH:MM]  
**Responsável:** [Nome/Equipe responsável]  
**Base:** [Estado atual - ex: 67% implementado (37h/55h) - Sistema funcional mas incompleto]

---

## 📋 Resumo Executivo

Este documento detalha o plano de [tipo de ação] para [objetivo específico], focando nas **[lacunas/objetivos principais identificados]** na análise sistemática.

### 🎯 **SITUAÇÃO ATUAL ([X%] IMPLEMENTADO)**

**✅ FUNDAÇÃO SÓLIDA IMPLEMENTADA:**
- ✅ **[Componente 1]** [status] com [detalhe técnico]
- ✅ **[Componente 2]** [status] operacionais
- ✅ **[Componente 3]** com [detalhes específicos]
- ✅ **[Componente 4]** com [número] funcionais

### 🚨 **LACUNAS CRÍTICAS IDENTIFICADAS ([Y%] FALTANDO)**

**❌ COMPONENTES CRÍTICOS AUSENTES:**
- ❌ **[Sistema 1]** ([componentes específicos])
- ❌ **[Sistema 2]** ([funcionalidades faltantes])
- ❌ **[Sistema 3]** ([integrações necessárias])
- ❌ **[Sistema 4]** ([métricas e monitoramento])

### 🎯 Objetivos do [Plano]
- **Implementar [sistema 1]** [descrição específica]
- **Desenvolver [sistema 2]** [descrição específica]
- **Configurar [sistema 3]** [descrição específica]
- **Finalizar [sistema 4]** [descrição específica]
- **Otimizar sistema** [descrição específica]

---

## 📊 Status Geral

- [ ] **Fase 1**: [Nome da Fase] - **[X%] [STATUS]** ([Xh]) - 1.1 ❌ + 1.2 ❌ + 1.3 ❌
- [ ] **Fase 2**: [Nome da Fase] - **[X%] [STATUS]** ([Xh]) - 2.1 ❌ + 2.2 ❌ + 2.3 ❌
- [ ] **Fase 3**: [Nome da Fase] - **[X%] [STATUS]** ([Xh]) - 3.1 ❌ + 3.2 ❌
- [ ] **Fase 4**: [Nome da Fase] - **[X%] [STATUS]** ([Xh]) - 4.1 ❌ + 4.2 ❌
- [ ] **Fase 5**: [Nome da Fase] - **[X%] [STATUS]** ([Xh]) - 5.1 ❌ + 5.2 ❌

**Progresso Total Projeto:** `0h/Xh (0%) ❌ [SISTEMA] PENDENTE`

---

## 🏗️ FASE 1: [NOME DA PRIMEIRA FASE]

**Objetivo:** [Descrição do objetivo específico]  
**Prioridade:** 🔴 CRÍTICA / 🟡 MÉDIA / 🟢 BAIXA  
**Estimativa:** X horas  
**Pré-requisitos:** [Lista de pré-requisitos]

### Tarefas:

#### 1.1 [Nome da Primeira Tarefa] (X horas)
**Responsável:** [Role específico + Especialista]  
**Entregável:** [Descrição clara do entregável]  
**Status:** ❌ **PENDENTE**

- [ ] **Implementar** [funcionalidade principal]
  - [ ] [Sub-tarefa específica 1]
  - [ ] [Sub-tarefa específica 2]
  - [ ] [Sub-tarefa específica 3]
  - [ ] [Integração ou configuração]
- [ ] **Configurar** [configurações necessárias]
  - [ ] [Configuração específica 1]
  - [ ] [Configuração específica 2]
  - [ ] [Validações necessárias]
- [ ] **Implementar** [funcionalidade secundária]
  - [ ] [Detalhe de implementação 1]
  - [ ] [Detalhe de implementação 2]
  - [ ] [Integração com sistema existente]

**[Implementação/Código/Configuração] Exemplo:**
```[linguagem]
// [Comentário explicativo]
[Código exemplo ou configuração]
```

**Critérios de Sucesso:**
- [ ] ✅ [Critério específico 1]
- [ ] ✅ [Critério específico 2]
- [ ] ✅ [Critério específico 3]

#### 1.2 [Nome da Segunda Tarefa] (X horas)
**Responsável:** [Role específico]  
**Entregável:** [Descrição do entregável]
**Status:** ❌ **PENDENTE**

- [ ] **Implementar** [funcionalidade]
- [ ] **Configurar** [configurações]
- [ ] **Testar** [aspectos a testar]

**Critérios de Sucesso:**
- [ ] ✅ [Critério específico]

### Comandos de Validação Fase 1

```bash
# Testar [funcionalidade]
[comando específico]

# Validar [aspecto]
[comando específico]

# Verificar [integração]
[comando específico]

# Health check [sistema]
[comando específico]
```

**📝 LOG DE EXECUÇÃO - FASE 1:**
```bash
# [Data/Hora] - FASE 1.X - [Nome da tarefa] (STATUS)
# - [Descrição do que foi implementado]
# - [Detalhes técnicos importantes]
# - [Resultados de testes]
# - [Arquivos criados/modificados]
```

---

## [Repetir estrutura para outras fases]

---

## 📈 MÉTRICAS DE SUCESSO FINAL

### **[Área 1] (Fase X)**
- [ ] ✅ [Métrica específica com target numérico]
- [ ] ✅ [Métrica de qualidade]
- [ ] ✅ [Métrica de performance]
- [ ] ✅ [Métrica de funcionalidade]

### **[Área 2] (Fase Y)**
- [ ] ✅ [Métrica específica]
- [ ] ✅ [Métrica de integração]
- [ ] ✅ [Métrica de escalabilidade]

### **Sistema Completo (Todas as Fases)**
- [ ] ✅ [Métrica final de sistema]
- [ ] ✅ [Métrica de qualidade geral]
- [ ] ✅ [Métrica de prontidão para produção]

---

## 🔄 ESTRATÉGIA DE ROLLBACK

### **Rollback por Fase**

#### **Rollback Fase X → Fase Y**
```bash
# [Ação específica para rollback]
[comandos de rollback]
```

#### **Rollback Completo → Estado Inicial**
```bash
# [Ação para volta completa]
[comandos de rollback completo]
```

---

## 📅 CRONOGRAMA DE [IMPLEMENTAÇÃO/EXECUÇÃO]

| Fase | Duração | Dependências | Risco | Responsável | Status |
|------|---------|--------------|-------|-------------|--------|
| **Fase 1** | Xh ([X] dia[s]) | [Dependências] | 🟢/🟡/🔴 [Nível] | [Role] + [Especialista] | ❌ **PENDENTE** |
| **Fase 2** | Xh ([X] dia[s]) | Fase 1 concluída | 🟢/🟡/🔴 [Nível] | [Role] + [Especialista] | ❌ **PENDENTE** |
| **Fase 3** | Xh ([X] dia[s]) | Fases 1-2 concluídas | 🟢/🟡/🔴 [Nível] | [Role] + [Especialista] | ❌ **PENDENTE** |

**Total Estimado: [X] horas ([Y] dias úteis)**

### **Marcos de [Implementação/Execução]**
- [ ] **Dia 1**: [Marco importante 1]
- [ ] **Dia X**: [Marco importante 2]
- [ ] **Dia Y**: [Marco final]

---

## 🎯 CONSIDERAÇÕES FINAIS

### **Benefícios do [Plano/Implementação]**
- 🚀 **[Benefício 1]** [descrição específica]
- 🧠 **[Benefício 2]** [descrição específica]
- 🤖 **[Benefício 3]** [descrição específica]
- 📊 **[Benefício 4]** [descrição específica]

### **Riscos Mitigados**
- ⚠️ **[Risco 1]**: [Estratégia de mitigação]
- ⚠️ **[Risco 2]**: [Estratégia de mitigação]
- ⚠️ **[Risco 3]**: [Estratégia de mitigação]

### **Garantia de Sucesso**
- ✅ **[Garantia 1]**: [Justificativa]
- ✅ **[Garantia 2]**: [Justificativa]
- ✅ **[Garantia 3]**: [Justificativa]

---

## 📊 TRACKING DE EXECUÇÃO FINAL

### Status Global de [Implementação/Execução]
- [ ] **Fase 1:** ❌ **[X%] [STATUS]** - [Nome da Fase] ([X]h)
- [ ] **Fase 2:** ❌ **[X%] [STATUS]** - [Nome da Fase] ([X]h)
- [ ] **Fase 3:** ❌ **[X%] [STATUS]** - [Nome da Fase] ([X]h)
- [ ] **Fase 4:** ❌ **[X%] [STATUS]** - [Nome da Fase] ([X]h)
- [ ] **Fase 5:** ❌ **[X%] [STATUS]** - [Nome da Fase] ([X]h)

**Progress [Implementação/Execução]:** `0h/[X]h (0%)` → **Meta: Sistema 100% Completo** ❌ **PENDENTE**

**Progress Total Projeto:** `[X]h/[Y]h ([Z]%)` → **Meta Total: [Sistema] 100% Funcional**

### Próximas Etapas Imediatas
1. **Aprovar este plano** de [implementação/execução]
2. **Iniciar Fase 1** - [Nome da primeira fase]
3. **Implementar [componente principal]** como prioridade máxima
4. **Validar cada fase** antes de prosseguir

**[Sistema]: Pronto para [ação] em [X] dias úteis**

---

## 📝 LOG DE EXECUÇÃO DETALHADO

### Data: [YYYY-MM-DD HH:MM]
**Fase executada:** [Fase X] - [Nome da tarefa]  
**Responsável:** [Nome/Role]  
**Status:** [Concluído/Em Progresso/Com Erro/Pendente]  
**Observações:**
```
- [x] [Descrição específica do que foi realizado]
- [x] [Validações executadas]
- [x] [Testes realizados]
- [x] [Arquivos modificados/criados]
- [ ] [Itens pendentes se houver]
- ❌ [Problemas encontrados (se houver)]
```

### Comandos Executados:
```bash
# [Descrição do comando]
[comando executado]

# [Resultado do comando]
[output relevante]
```

### Métricas Atingidas:
- ✅ [Métrica específica]: [Valor alcançado] / [Target]
- ✅ [Métrica de performance]: [Valor] ([Comparação com target])

---

## 🔄 COMANDOS DE EXECUÇÃO

### Validação Geral:
```bash
# Verificar status atual
[comando de status]

# Validar [componente específico]
[comando de validação]
```

### Execução por Fase:
```bash
# Fase 1
[comandos específicos da fase 1]

# Fase 2
[comandos específicos da fase 2]
```

### Rollback de Emergência:
```bash
# Se algo der errado, voltar para tag anterior
[comandos de rollback]
```

---

## 🤖 Orientações de Auto-Atualização

### Para o AI Assistant:
```
SEMPRE que interagir com este documento:

1. 🔍 VERIFICAR status atual dos checkboxes
2. ✅ MARCAR [x] tarefas concluídas durante a sessão
3. 📝 ATUALIZAR log de execução com:
   - Data/hora atual
   - Fase executada
   - Status (Concluído/Em Progresso/Com Erro)
   - Observações detalhadas
4. 📊 ATUALIZAR status geral das fases na seção inicial
5. 🎯 VERIFICAR métricas de sucesso e marcar as atingidas

TEMPLATE PARA LOG:
### Data: [DATA ATUAL]
**Fase executada:** Fase X - [Nome da tarefa]
**Responsável:** AI Assistant
**Status:** [Concluído/Em Progresso/Com Erro]
**Observações:**
```
- [x] Tarefa específica realizada
- [x] Verificações executadas
- [ ] Pending items se houver
- ❌ Problemas encontrados (se houver)
```

COMANDOS A EXECUTAR QUANDO APLICÁVEL:
- [comando de validação principal]
- [comando de build específico]
- git status (verificar mudanças)
```

### Para Desenvolvedores:
- 📖 **Leia o auto prompt** no início do arquivo antes de começar
- ✅ **Marque progressos** conforme avança nas tarefas
- 📝 **Documente problemas** no log de execução
- 🔄 **Execute comandos** de validação listados
- 🏷️ **Crie tags** antes de mudanças críticas

---

**Status Final: [PLANO PRONTO PARA EXECUÇÃO/IMPLEMENTAÇÃO CONCLUÍDA]** 🚀

*Última atualização: [YYYY-MM-DD HH:MM] BRT*  
*Versão: [X.Y] - [Descrição da versão]*  
*Responsável: [Nome/Equipe]* 