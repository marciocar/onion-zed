---
name: onion-git-hotfix-finish
description: Finalizar hotfix com merge para main e develop, tag e deploy. Use quando uma correção emergencial estiver pronta para produção, fazendo merge para main/master, back-merge para develop, criando tag de patch e preparando deploy.
disable-model-invocation: true
---

# ✅ Git Flow - Finalizar Hotfix

Finalizar correção emergencial realizando deploy para produção com merge em main/master e develop, criação de tags emergenciais e cleanup. Workflow crítico para release de emergency fixes.

> **Parâmetro**: aceita um nome de hotfix opcional. Faça o parsing do texto invocado; sem argumento, auto-detecta a hotfix branch atual.

## 🎯 Funcionalidades  

### Emergency Release e Deployment
- Merge emergencial de hotfix branch para main/master
- Back-merge imediato para develop branch
- Criação automática de emergency patch tags
- Deploy preparation para production environment
- Cleanup automático pós-deployment

### Production Safety e Validation
- Validações críticas pré-merge para produção
- Emergency conflict detection e resolution guidance
- Production readiness verification
- Rollback preparation automática
- Emergency testing validation

### Critical Operations Management
- Conclusão da task com emergency status no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`; suporta Jira, ClickUp, Asana, Linear)
- Team notification de emergency deployment
- Emergency documentation automática
- Production deployment tracking
- Integration com CI/CD emergency pipelines

## 🚀 Como Usar

```bash
/onion-git-hotfix-finish                        # Auto-detecta hotfix atual
/onion-git-hotfix-finish fix-payment-gateway    # Finaliza hotfix específica  
```

**Pré-requisitos**: Em hotfix branch ou especificar nome da correção

### Processo Executado
1. **Emergency Detection**: Detecta hotfix branch atual ou busca específica
2. **Critical Validation**: Verifica hotfix state, conflicts, production readiness
3. **Production Preview**: Exibe impacto do emergency deployment
4. **Emergency Confirmation**: Solicita confirmação para production release
5. **Production Merge**: Executa merge para main + back-merge para develop
6. **Emergency Tagging**: Cria patch tag com emergency release notes
7. **Production Deploy**: Prepara deployment e, se `TASK_MANAGER_PROVIDER` != `none`, atualiza status da task no provider ativo (ex.: ClickUp, Jira, Asana, Linear)
8. **Emergency Cleanup**: Remove hotfix branch e finaliza emergency workflow

### Emergency Deployment Strategy
Durante finalização emergencial:
- Production merge: Emergency-optimized merge strategy
- Conflict handling: Priority resolution guidance
- Tag creation: Emergency patch version increment
- Team communication: Immediate notification de production changes

## 🤝 Integração com o especialista GitFlow

*Este comando sempre consulta o especialista GitFlow para emergency merge validation, critical conflict resolution, production deployment strategy e troubleshooting de emergency release complexos.* Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/gitflow-specialist.md` e atue como esse especialista para validar o merge emergencial, resolver conflitos críticos e definir a estratégia de deployment."

## ⚠️ Resolução de Problemas

### Hotfix Branch Not Found
- **Sintoma**: Não consegue detectar hotfix branch ativa
- **Solução**: `git checkout hotfix/name` ou especificar nome no comando

### Emergency Merge Conflicts
- **Causa**: Conflicts críticos entre hotfix e production branches
- **Fix**: Emergency resolution guidance via especialista GitFlow

### Production Branch Protection
- **Sintoma**: Branch protection impede emergency merge
- **Solução**: Emergency override procedures ou Pull Request workflow

### Critical Tests Failing
- **Causa**: Emergency tests falham durante validation
- **Fix**: Emergency test strategy ou production override (com approval)

### Emergency Tag Creation Failed
- **Sintoma**: Problemas na criação de emergency patch tags
- **Solução**: o especialista GitFlow orienta sobre tag strategy e conflicts

### Production Deployment Issues
- **Causa**: Problemas durante emergency deployment preparation
- **Fix**: Emergency deployment guidance e rollback preparation

### Team Notification Failed  
- **Sintoma**: Task Manager configurado (ex.: ClickUp, Jira, Asana, Linear) ou team notifications não funcionando
- **Solução**: Verificar `TASK_MANAGER_PROVIDER` e credenciais via `/onion-meta-setup-integration`; manual emergency communication + guidance do especialista GitFlow
