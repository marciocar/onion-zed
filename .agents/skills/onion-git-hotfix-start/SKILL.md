---
name: onion-git-hotfix-start
description: Iniciar hotfix branch para correção emergencial em produção. Use quando houver um problema crítico em produção que exija criar uma hotfix branch a partir de main/master rapidamente, com auto-versioning de patch e task urgente.
disable-model-invocation: true
---

# 🚨 Git Flow - Iniciar Hotfix

Iniciar correção emergencial criando hotfix branch a partir de main/master para resolver problemas críticos em produção. Workflow otimizado para máxima velocidade com validações de segurança essenciais.

> **Parâmetro**: nome da correção obrigatório, passado entre aspas. Faça o parsing do texto invocado para extraí-lo.

## 🎯 Funcionalidades

### Emergency Hotfix Workflow
- Criação imediata de hotfix branch a partir de main/master (produção)
- Auto-versioning para patch releases emergenciais
- Task urgente com prioridade máxima automática no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`; suporta Jira, ClickUp, Asana, Linear)
- Validações críticas de estado de produção
- Setup otimizado para correção imediata

### Safety-First Emergency Operations
- Detecção automática de primary branch (main/master)
- Validações essenciais de working directory
- Emergency override para uncommitted changes
- Rollback preparation automática
- Emergency documentation e tracking setup

### Criticidade e Tracking Urgente
- Tags de urgência automáticas no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`; ex.: ClickUp, Jira, Asana, Linear)
- Notificações escaladas para team awareness
- Emergency workflow prioritization
- Integration com o especialista GitFlow para emergency guidance
- Automated emergency documentation

## 🚀 Como Usar

```bash
/onion-git-hotfix-start "fix-payment-gateway"    # Emergency payment fix
/onion-git-hotfix-start "security-patch-auth"    # Security hotfix
/onion-git-hotfix-start "critical-memory-leak"   # Performance emergency
/onion-git-hotfix-start "api-timeout-fix"        # API emergency
```

**Pré-requisitos**: Nome da correção obrigatório, repositório Git válido

### Processo Executado
1. **Emergency Validation**: Verifica nome hotfix, repository state, critical validations
2. **Primary Branch Detection**: Detecta main/master automaticamente
3. **Emergency Override**: Handles uncommitted changes com emergency stashing
4. **Hotfix Branch Creation**: Cria hotfix/name branch a partir de produção
5. **Task Manager Emergency Setup** (condicional): Se `TASK_MANAGER_PROVIDER` != `none`, cria task urgente com máxima prioridade no provider ativo (ex.: ClickUp, Jira, Asana, Linear); caso contrário, etapa é ignorada
6. **Team Notification**: Alerta equipe sobre emergency fix em andamento

### Emergency Features
Durante execução emergencial:
- Working directory: Emergency stash se necessário
- Branch detection: Main/master identification automática  
- Priority escalation: task com urgência máxima no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`)
- Team alerts: Notification automática de emergency workflow

## 🤝 Integração com o especialista GitFlow

*Este comando sempre consulta o especialista GitFlow para emergency workflow guidance, critical branch validation, hotfix strategy optimization e troubleshooting de situações emergenciais complexas.* Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/gitflow-specialist.md` e atue como esse especialista para guidance de emergency workflow, validação de branch crítica e otimização da estratégia de hotfix."

## ⚠️ Resolução de Problemas

### Hotfix Name Required
- **Sintoma**: Comando executado sem nome da correção
- **Solução**: Fornecer nome descritivo da correção emergencial

### Uncommitted Changes Emergency
- **Causa**: Working directory não limpo durante emergency
- **Fix**: Comando oferece emergency stash automático ou manual commit

### Primary Branch Detection Failed  
- **Sintoma**: Não consegue detectar main/master branch
- **Solução**: o especialista GitFlow fornece detection strategy para repository

### Not Git Repository
- **Causa**: Executado fora de repositório Git válido
- **Fix**: `git init && /onion-git-init` para emergency setup

### Emergency Override Issues
- **Sintoma**: Problemas durante emergency workflow execution
- **Solução**: o especialista GitFlow fornece emergency guidance específica

### Critical State Validation Failed
- **Causa**: Repository em estado inconsistente para hotfix
- **Fix**: Emergency validation guidance via consulta ao especialista GitFlow
