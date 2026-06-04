---
# Claude Code - Phase Execution Prompt Template Metadata
template:
  type: phase-execution-prompt
  version: 2.0
  category: execution
  name: "[TIPO DE FASE]"
  architectural_pattern: "[Padrão Arquitetural/Metodologia Principal]"

context:
  title: "🤖 INSTRUÇÕES PARA EXECUÇÃO SISTEMÁTICA"
  phase_type: "[TIPO DE FASE]"
  current_state: "[DESCRIÇÃO DO ESTADO - ex: PROJETO 67% IMPLEMENTADO - FASE X.Y PENDENTE]"
  base: "[TECNOLOGIAS/COMPONENTES JÁ FUNCIONANDO]"
  last_update: "[TIMESTAMP]"

usage:
  instructions:
    - "✅ SUBSTITUA todos os placeholders [VARIÁVEL] pelos valores específicos"
    - "📝 MARQUE checkboxes [ ] → [x] conforme progresso nas validações"
    - "📅 ATUALIZE datas, responsável e observações durante execução"
    - "🔄 EXECUTE comandos listados rigorosamente na ordem especificada"
    - "🏷️ DOCUMENTE problemas encontrados para correção imediata"
    - "💰 MONITORE métricas críticas durante implementação"
    - "🔄 ATUALIZE status de cada validação no documento ao final"

standards:
  validation_status:
    approved: "[x] - Validação/Tarefa APROVADA - funcionando 100%"
    pending: "[ ] - Validação/Tarefa PENDENTE - ainda não executada"
    failed: "❌ - Validação/Tarefa FALHADA - problema crítico encontrado"
    partial: "⚠️ - Validação/Tarefa PARCIAL - funciona com ressalvas"
    in_progress: "🔄 - Validação/Tarefa EM PROGRESSO - execução iniciada"
  
  documentation_patterns:
    feature: "feat([módulo]): implementa [funcionalidade] - [Padrão] compliance"
    docs: "docs([fase]): atualiza documentação fase [X.Y] - [status]"
    fix: "fix([área]): corrige [problema] - validação [critério]"
    test: "test([componente]): valida [funcionalidade] - [resultado]"
  
  essential_commands:
    package_manager: "📦 SEMPRE usar [gerenciador de pacotes] para comandos"
    main: "[comando-principal] - [descrição da função principal]"
    validation: "[comando-validação] - [descrição da validação]"
    regeneration: "[comando-regeneração] - [descrição da regeneração]"

critical_attention:
  warnings:
    - "⚠️ SEMPRE mantenha [sistema principal] funcionando durante execução"
    - "⚠️ USE [padrão de qualidade] para validação contínua"
    - "⚠️ EXECUTE cada validação completamente antes de prosseguir"
    - "⚠️ DOCUMENTE incompatibilidades no log imediatamente"
    - "⚠️ MONITORE [métricas críticas] em tempo real"
    - "⚠️ VALIDE [padrão arquitetural] em cada transição de estado"
    - "⚠️ MANTENHA backup do estado atual antes de mudanças críticas"

milestones:
  checkpoint_1: "🎯 [Primeiro checkpoint da fase]"
  checkpoint_2: "🎯 [Segundo checkpoint da fase]"
  checkpoint_3: "🎯 [Checkpoint final da fase]"

success_metrics:
  - metric: "📊 [Métrica 1]"
    target: "[Target específico]"
    criteria: "[critério de medição]"
  - metric: "🤖 [Métrica 2]"
    target: "[Target específico]"
    criteria: "[critério de medição]"
  - metric: "🏷️ [Métrica 3]"
    target: "[Target específico]"
    criteria: "[critério de medição]"
  - metric: "⚡ [Métrica 4]"
    target: "[Target específico]"
    criteria: "[critério de medição]"
  - metric: "🔒 [Métrica 5]"
    target: "[Target específico]"
    criteria: "[critério de medição]"

ai_assistant:
  auto_update: true
  track_validations: true
  enforce_sequence: true
  monitor_compliance: true
---

# 🎯 TEMPLATE: PROMPT DE EXECUÇÃO DE FASE DE PROJETO
## Seguir rigorosamente a sequência [Padrão Arquitetural/Metodologia Principal]

---

## 📋 **INFORMAÇÕES DA FASE**

**Fase:** [X.Y] - [Nome Descritivo da Fase]  
**Plano Base:** [Nome do Plano de Implementação/Projeto] - **FASE [X.Y]**  
**Documento de Referência:** @[arquivo-plano-base.md]  
**Data de Execução:** [YYYY-MM-DD HH:MM]  
**Responsável:** [Nome/Equipe]  
**Estimativa:** [X] horas  
**Prioridade:** [🔴 CRÍTICA | 🟡 ALTA | 🟢 MÉDIA]

---

## 1. CARREGAR E VALIDAR PRÉ-REQUISITOS

### 🎯 **REGRA DE OURO SEMPRE:**
> **[Padrão Arquitetural Principal] é a ÚNICA fonte de verdade**  
> **Tudo deriva do [Sistema Principal], nunca o contrário**

### **Pré-requisitos Obrigatórios:**
Carregar, entender e seguir rigorosamente as instruções do **@[documento-base.md]**

**Verificar pré-requisitos da Fase [X.Y]:**
- [ ] **Fase anterior** (Fase [X.Y-1]) marcada como [x] COMPLETO
- [ ] **Todos os checkboxes** da fase anterior em ✅
- [ ] **Nenhum erro crítico** nos logs de sistema
- [ ] **Ambiente de desenvolvimento** validado e funcional
- [ ] **[Dependências específicas]** instaladas e funcionando
- [ ] **[Padrão de Qualidade]** compliance validado

**Comandos de Validação de Pré-requisitos:**
```bash
# Verificar status geral do sistema
[comando-status-geral]

# Validar [padrão principal] compliance
[comando-validação-padrão] --enforce-[padrão-flag]

# Verificar integridade das dependências
[comando-verificação-dependências]

# Health check completo
[comando-health-check]
```

**Critérios de Aprovação:**
- [ ] ✅ Todos os comandos retornam status OK
- [ ] ✅ Logs sem erros críticos
- [ ] ✅ [Padrão] compliance 100%
- [ ] ✅ Ambiente pronto para desenvolvimento

---

## 2. EXECUTAR IMPLEMENTAÇÃO

### **Objetivo da Fase [X.Y]:**
[Descrição específica do que será implementado/executado nesta fase]

### **Deliverables Esperados:**
- [ ] **[Deliverable 1]**: [Descrição específica]
- [ ] **[Deliverable 2]**: [Descrição específica]  
- [ ] **[Deliverable 3]**: [Descrição específica]
- [ ] **[Deliverable N]**: [Descrição específica]

### **Comandos Obrigatórios DURANTE Implementação:**

#### **Regeneração [Sistema Principal] ANTES de qualquer mudança:**
```bash
# Regenerar [sistema principal] sempre antes de mudanças
[comando-regeneração-principal]

# Verificar regeneração bem-sucedida
[comando-verificação-regeneração]
```

#### **Validação [Padrão] Compliance DURANTE Desenvolvimento:**
```bash
# Validar [padrão] compliance contínua
[comando-validação-compliance] --enforce-[padrão-flag]

# Verificar integrações não foram quebradas
[comando-teste-integrações]
```

#### **Testes Específicos da Fase:**
```bash
# Testar implementação específica da fase
[comando-teste-fase] --phase=[X.Y]

# Validar funcionalidade implementada
[comando-validação-funcionalidade]
```

### **Implementação Passo a Passo:**

#### **Passo 1: [Nome do Primeiro Passo]**
**Duração:** [X] minutos  
**Objetivo:** [Descrição específica]

```[linguagem]
// [Comentário explicativo]
[Código ou configuração específica]
```

**Validação do Passo:**
```bash
# Verificar se passo 1 foi executado corretamente
[comando-validação-passo-1]
```

**Critério de Sucesso:**
- [ ] ✅ [Critério específico do passo 1]

#### **Passo 2: [Nome do Segundo Passo]**
[Repetir estrutura similar para cada passo...]

### **Log de Execução da Implementação:**
```bash
# [TIMESTAMP] - IMPLEMENTAÇÃO PASSO [N] - [Status]
# - [Descrição do que foi implementado]
# - [Validações realizadas]
# - [Problemas encontrados (se houver)]
# - [Performance observada]
```

---

## 3. VALIDAÇÃO PÓS-IMPLEMENTAÇÃO

### **Validações Obrigatórias:**

#### **Validação Completa do Sistema:**
```bash
# Validação 100% do sistema
[comando-validação-completa]

# Regeneração completa [Sistema Principal]
[comando-regeneração-completa]

# Testes específicos da fase implementada
[comando-teste-específico] --testNamePattern="Fase [X.Y]"
```

#### **Validação de Integridade:**
```bash
# Verificar que nada foi quebrado
[comando-teste-regressão]

# Validar performance não degradou
[comando-teste-performance]

# Verificar [padrão] ainda é fonte única de verdade
[comando-validação-fonte-verdade]
```

### **Métricas de Sucesso da Fase:**
- [ ] **[Métrica 1]**: [Valor esperado] → [Valor atual] ([✅/❌])
- [ ] **[Métrica 2]**: [Valor esperado] → [Valor atual] ([✅/❌])
- [ ] **[Métrica 3]**: [Valor esperado] → [Valor atual] ([✅/❌])
- [ ] **[Métrica N]**: [Valor esperado] → [Valor atual] ([✅/❌])

### **Critérios de Aceitação Final:**
- [ ] ✅ Todos os deliverables implementados e funcionando
- [ ] ✅ [Padrão] compliance mantido como Single Source of Truth
- [ ] ✅ Nenhuma regressão introduzida
- [ ] ✅ Performance dentro dos targets esperados
- [ ] ✅ Logs limpos sem erros críticos
- [ ] ✅ Testes automatizados passando

---

## 4. ATUALIZAR DOCUMENTAÇÃO

### **Atualizações Obrigatórias na Documentação:**
- [ ] **Marcar todos os checkboxes** da Fase [X.Y] como [x] COMPLETO
- [ ] **Atualizar status geral**: `- [x] **Fase [X.Y]**: [Nome da Fase] - **100% COMPLETO**`
- [ ] **Documentar no LOG DE EXECUÇÃO** com timestamp e resultados detalhados
- [ ] **Verificar se Score Atual** aumentou adequadamente ([X]/100 → [Y]/100)
- [ ] **Atualizar métricas** de progresso do projeto
- [ ] **Documentar lições aprendidas** e pontos de atenção

### **Arquivos a Atualizar:**
```bash
# Arquivos de documentação que devem ser atualizados
[arquivo-1]: [Seção específica a atualizar]
[arquivo-2]: [Seção específica a atualizar]
[arquivo-plano-principal]: [Status da fase]
[arquivo-progresso]: [Métricas atualizadas]
```

### **Template de Documentação:**
```markdown
### 📅 [TIMESTAMP] - FASE [X.Y] CONCLUÍDA

**Fase:** [X.Y] - [Nome da Fase]  
**Status:** ✅ **COMPLETO**  
**Duração:** [X]h ([tempo real] vs [estimado])  
**Score:** [anterior]/100 → [novo]/100  

**Deliverables Concluídos:**
- [x] [Deliverable 1]: [Status/observações]
- [x] [Deliverable 2]: [Status/observações]  
- [x] [Deliverable N]: [Status/observações]

**Métricas Atingidas:**
- [Métrica 1]: [Valor] ([Status])
- [Métrica 2]: [Valor] ([Status])

**Observações:**
- [Ponto importante 1]
- [Ponto importante 2]
- [Lições aprendidas]
```

---

## 5. COMMIT PADRONIZADO

### **Estrutura do Commit:**
```bash
git add .
git commit -m "feat([módulo-fase]): implementa [Nome da Fase [X.Y]] - [Padrão] compliance

✅ Fase [X.Y] - [Nome da Fase] implementada
📊 Score: [anterior]/100 → [novo]/100  
🎯 [Padrão]: Mantido como Single Source of Truth
🧪 Testes: [X] passando ([detalhes dos testes])
📝 Docs: Atualizadas com checkboxes completos

Components:
- [componente1]: implementado/melhorado ([detalhes])
- [componente2]: implementado/melhorado ([detalhes])  
- [componenteN]: implementado/melhorado ([detalhes])

Validation:
- [comando-validação-completa]: ✅ passing
- [comando-regeneração]: ✅ successful  
- [comando-teste-fase]: ✅ phase [X.Y] complete
- [comando-performance]: ✅ targets met

Metrics:
- [Métrica 1]: [valor] ([status])
- [Métrica 2]: [valor] ([status])  
- [Métrica N]: [valor] ([status])"

git push
```

### **Validação do Commit:**
```bash
# Verificar que commit foi bem-sucedido
git log --oneline -1

# Verificar que push foi bem-sucedido
git status
```

---

## 6. VERIFICAÇÃO FINAL

### **Checklist de Verificação Final:**
- [ ] **Todos os checkboxes** da Fase [X.Y] estão marcados como [x]
- [ ] **Score foi atualizado** corretamente no documento principal
- [ ] **Log de execução** documentado com timestamp e detalhes
- [ ] **Commit segue padrão** estabelecido e foi realizado com sucesso
- [ ] **[Padrão] permanece** como fonte única da verdade
- [ ] **Nenhuma regressão** foi introduzida no sistema
- [ ] **Todos os testes** estão passando
- [ ] **Performance está** dentro dos targets esperados
- [ ] **Documentação está** atualizada e consistente

### **Comandos de Verificação Final:**
```bash
# Status geral pós-implementação
[comando-status-final]

# Verificação de integridade completa
[comando-integridade-completa]

# Confirmação [padrão] compliance
[comando-confirmação-padrão]

# Health check final
[comando-health-final]
```

### **Sign-off da Fase:**
```bash
# [TIMESTAMP] - VERIFICAÇÃO FINAL FASE [X.Y]
# Status: [✅ APROVADO | ❌ REPROVADO | ⚠️ COM RESSALVAS]  
# Responsável: [Nome]
# Próxima fase: [X.Y+1] - [Nome da próxima fase]
# Observações: [Pontos importantes para próxima fase]
```

---

## 🔄 **ROLLBACK DE EMERGÊNCIA**

### **Se Algo Der Errado:**

#### **Rollback Rápido:**
```bash
# Reverter para estado anterior conhecido
git revert HEAD

# Restaurar [sistema principal]
[comando-restauração-sistema]

# Verificar integridade pós-rollback
[comando-verificação-rollback]
```

#### **Rollback Completo:**
```bash
# Voltar para tag da fase anterior
git checkout [tag-fase-anterior]

# Regenerar sistema completo
[comando-regeneração-completa]

# Validar estado anterior
[comando-validação-estado-anterior]
```

### **Documentação de Rollback:**
```markdown
### 🚨 [TIMESTAMP] - ROLLBACK FASE [X.Y]

**Motivo:** [Descrição do problema]  
**Ação:** [Tipo de rollback executado]  
**Estado Atual:** [Descrição do estado pós-rollback]  
**Próximos Passos:** [O que fazer a seguir]  
```

---

## 📊 **TRACKING E MÉTRICAS**

### **Status da Fase [X.Y]:**
- [ ] **Pré-requisitos**: ❌ **PENDENTE** ([X] de [Y] validados)
- [ ] **Implementação**: ❌ **PENDENTE** ([X] de [Y] passos concluídos)  
- [ ] **Validação**: ❌ **PENDENTE** ([X] de [Y] validações aprovadas)
- [ ] **Documentação**: ❌ **PENDENTE** ([X] de [Y] arquivos atualizados)
- [ ] **Commit**: ❌ **PENDENTE** (estrutura padrão seguida)
- [ ] **Verificação Final**: ❌ **PENDENTE** ([X] de [Y] critérios atendidos)

### **Progresso Total do Projeto:**
**Antes da Fase:** `[X]h/[Y]h ([Z]%)` - [Status Anterior]  
**Após a Fase:** `[X+A]h/[Y]h ([Z+B]%)` - [Status Esperado]  

### **Próximas Etapas:**
1. **Executar esta fase** seguindo rigorosamente este prompt
2. **Validar completamente** antes de marcar como concluída  
3. **Preparar Fase [X.Y+1]** - [Nome da próxima fase]
4. **Continuar** mantendo [padrão] como fonte única de verdade

---

## 🤖 **ORIENTAÇÕES PARA AI ASSISTANT**

### **Ao Executar Esta Fase:**
```
SEMPRE seguir esta sequência:

1. 🔍 VERIFICAR todos os pré-requisitos antes de começar
2. ✅ MARCAR checkboxes conforme progresso real
3. 📝 ATUALIZAR log de execução com detalhes específicos:
   - Timestamp atual
   - Fase executada
   - Status detalhado  
   - Observações técnicas
   - Problemas encontrados
   - Métricas coletadas
4. 📊 ATUALIZAR status geral no topo do documento
5. 🎯 VERIFICAR métricas de sucesso e marcar as atingidas
6. 💾 DOCUMENTAR tudo antes de prosseguir

TEMPLATE PARA LOG DURANTE EXECUÇÃO:
### 📅 [TIMESTAMP] - EXECUÇÃO FASE [X.Y] - [PASSO]
**Responsável:** AI Assistant  
**Status:** [Em Progresso/Concluído/Com Erro]  
**Passo Atual:** [X.Y.Z] - [Nome do passo]  
**Observações:**
```
- [x] [Ação específica realizada]
- [x] [Validação executada]  
- [x] [Resultado obtido]
- [ ] [Pendências identificadas]
- ❌ [Problemas encontrados (se houver)]
```

COMANDOS A EXECUTAR SEMPRE:
- [comando-status] (verificar estado atual)
- [comando-validação] (validar mudanças)
- [comando-regeneração] (manter [padrão] atualizado)
- git status (verificar mudanças no código)
```

### **Para Desenvolvedores:**
- 📖 **Leia o prompt completo** antes de iniciar a execução
- ✅ **Execute cada seção** na ordem especificada
- 📝 **Documente problemas** imediatamente no log
- 🔄 **Execute comandos** de validação obrigatórios
- 🏷️ **Crie tags** antes de mudanças críticas

---

**Status Final: PROMPT PRONTO PARA EXECUÇÃO DA FASE [X.Y]** 🚀

*Template de Prompt de Execução de Fase v1.0*  
*Baseado em: prompt-execucao-padronizado-completo.md*  
*Última atualização do template: [TIMESTAMP]*  
*Compatível com: [Padrões/Metodologias aplicáveis]* 