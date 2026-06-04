---
name: onion-git-feature-finish
description: Finalizar feature com merge para develop e cleanup. Use quando uma feature estiver completa e precisar mergear de forma segura a feature branch para develop, com validações de conflito, confirmação obrigatória e limpeza de branches.
disable-model-invocation: true
---

# ✅ Git Flow - Finalizar Feature

Finalizar desenvolvimento de feature realizando merge seguro para develop branch com validações automáticas e cleanup completo. Processo seguro com confirmações obrigatórias para prevenir erros de produção.

## 🎯 Funcionalidades

### Safety-First e Validações
- Confirmação obrigatória antes de merge feature → develop
- Análise automática de conflitos e working directory  
- Validação de status da develop branch (sincronização)
- Preview detalhado das mudanças que serão mergeadas
- Guidance para resolução de problemas encontrados

### GitFlow Compliance e Automação
- Merge seguindo padrão oficial GitFlow (feature → develop)
- Cleanup automático de branch local e remote após merge
- Atualização de ClickUp task e session archival
- Integração preservada com o gitflow-specialist para operações complexas

### Educação e UX
- Context display mostrando impacto das mudanças
- Progress indicators durante operação
- Educational content sobre GitFlow workflow
- Next steps guidance após finalização

## 🚀 Como Usar

```bash
/onion-git-feature-finish                    # Auto-detecta branch atual
```

**Pré-requisitos**: Execute na branch de feature que deseja finalizar

### Processo Executado
1. **Análise**: Detecta branch atual e valida estado do repositório
2. **Validações**: Verifica working directory, conflicts e status develop
3. **Preview**: Exibe impacto das mudanças (commits, files, lines)
4. **Confirmação**: Solicita confirmação explícita do usuário  
5. **Merge**: Executa merge seguro feature → develop
6. **Cleanup**: Remove branch local/remote e atualiza ClickUp task
7. **Archive**: Move session para estado finalizado

### Educational Context
Durante execução, o comando ensina conceitos GitFlow:
- Visualização do workflow: `develop → feature/name → develop`
- Impacto da operação na team collaboration
- Best practices para feature development
- Guidance para próximos passos

## 🤝 Integração com o especialista GitFlow

*Este comando sempre consulta o especialista GitFlow para análise de conflitos, validação de merge strategy, execução segura do merge e guidance para resolução de problemas complexos.* Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/gitflow-specialist.md` e atue como esse especialista para analisar conflitos, validar a estratégia de merge e executar o merge com segurança."

## ⚠️ Resolução de Problemas

### Uncommitted Changes
- **Sintoma**: Working directory não está limpo
- **Solução**: `git add . && git commit -m "final changes"` antes de finalizar

### Merge Conflicts Detectados  
- **Causa**: Mudanças conflitantes entre feature e develop
- **Fix**: Resolver conflicts manualmente ou usar `git merge develop` na feature branch primeiro

### Develop Branch Desatualizada
- **Sintoma**: Develop branch está atrás do remote
- **Solução**: `git checkout develop && git pull origin develop` antes de finalizar feature

### Tests Failing
- **Sintoma**: Testes automatizados falhando
- **Solução**: Corrigir testes ou usar flag de override (não recomendado)

### Feature Branch Não Encontrada
- **Causa**: Not em uma feature branch ou branch name incorreto
- **Fix**: `git checkout feature/your-feature-name` antes de executar comando

### Remote Branch Issues
- **Sintoma**: Problemas com remote branch tracking
- **Solução**: `git push -u origin feature/name` para estabelecer tracking
