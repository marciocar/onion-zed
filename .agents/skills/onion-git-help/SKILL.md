---
name: onion-git-help
description: Ajuda contextual para comandos GitFlow do Sistema Onion. Use quando o usuário precisar de orientação sobre quais comandos git usar, próximos passos no fluxo GitFlow, troubleshooting de operações git ou referência de comandos por situação.
disable-model-invocation: true
---

# 🆘 Git Flow - Sistema de Ajuda

Fornecer ajuda contextual e interativa para todos os comandos GitFlow do Sistema Onion. Detectar automaticamente o estado atual do repositório e sugerir próximos passos apropriados.

> **Parâmetro**: aceita um tópico opcional (`feature`, `release`, `hotfix`, `init`). Faça o parsing do texto invocado; sem argumento, mostra a ajuda completa.

## 🎯 Funcionalidades

### Detecção Inteligente de Contexto
- Verificar se Git Flow está inicializado no repositório atual
- Identificar branch ativa e sugerir workflows apropriados  
- Detectar estado do projeto e recomendar próximos passos
- Integração com o especialista GitFlow para guidance avançada

### Sistema de Ajuda Estruturado
- **Help geral**: Visão completa de todos os comandos disponíveis
- **Help específico**: Documentação detalhada por comando individual
- **Troubleshooting**: Soluções para problemas comuns
- **Quick reference**: Comandos essenciais por situação

## 🚀 Como Usar

```bash
/onion-git-help                    # Help completo interativo
/onion-git-help feature           # Ajuda específica para features
/onion-git-help release           # Ajuda específica para releases  
/onion-git-help hotfix            # Ajuda específica para hotfixes
/onion-git-help init              # Ajuda para inicialização
```

## 🤖 Integração com o especialista GitFlow

Para cada solicitação de ajuda:

1. **Consultar o especialista GitFlow** para análise contextual do repositório
2. **Receber guidance** específica baseada no estado atual
3. **Apresentar recomendações** personalizadas para o desenvolvedor  
4. **Fornecer exemplos práticos** para a situação detectada

Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/gitflow-specialist.md` e atue como esse especialista para analisar o estado do repositório e dar guidance contextual."

## 📋 Comandos Disponíveis

### Setup e Inicialização
- `/onion-git-init` - Configurar Git Flow no repositório
- `/onion-git-help` - Este sistema de ajuda

### Workflow de Features
- `/onion-git-feature-start "nome"` - Iniciar nova feature
- `/onion-git-feature-finish` - Finalizar e mergear feature
- `/onion-git-feature-publish` - Compartilhar feature em desenvolvimento

### Workflow de Releases
- `/onion-git-release-start "versão"` - Iniciar processo de release
- `/onion-git-release-finish` - Finalizar e deployar release

### Workflow de Hotfixes
- `/onion-git-hotfix-start "nome"` - Iniciar correção urgente
- `/onion-git-hotfix-finish` - Finalizar e deployar hotfix

### Sincronização
- `/onion-git-sync [branch]` - Sincronizar após merge de PR

## ⚠️ Troubleshooting Comum

### Repository não inicializado
**Problema**: Git Flow não configurado
**Solução**: Execute `/onion-git-init` para configuração automática

### Branch errada
**Problema**: Não está na branch correta para operação
**Solução**: Use comandos Git Flow que fazem checkout automaticamente  

### Conflitos de merge
**Problema**: Conflitos durante operações GitFlow
**Solução**: Resolva conflitos manualmente e continue com comando finish

### Estado inconsistente
**Problema**: Operação GitFlow interrompida
**Solução**: Consulte o especialista GitFlow para análise e recovery

## 💡 Próximos Passos Sugeridos

O sistema detectará automaticamente sua situação atual e sugerirá:

- **Se Git Flow não inicializado**: `/onion-git-init`
- **Se em develop**: `/onion-git-feature-start "nome-da-feature"`  
- **Se em feature branch**: `/onion-git-feature-finish` ou `/onion-git-feature-publish`
- **Se pronto para release**: `/onion-git-release-start "versão"`
- **Se problema em produção**: `/onion-git-hotfix-start "correção"`

---

*Este comando sempre consulta o especialista GitFlow para fornecer guidance contextual e personalizada.*
