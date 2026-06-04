---
name: onion-git-feature-start
description: Iniciar feature branch GitFlow com ambiente configurado. Use quando começar uma nova funcionalidade, criando a branch feature/<nome> a partir de develop, configurando tracking e preparando a sessão de desenvolvimento.
disable-model-invocation: true
---

# 🌿 Git Flow - Iniciar Feature

Iniciar desenvolvimento de uma nova funcionalidade criando uma branch GitFlow apropriada e configurando ambiente de desenvolvimento. Integração obrigatória com o especialista GitFlow para guidance especializada.

> **Parâmetro**: recebe o nome da feature entre aspas. Faça o parsing do texto invocado para extraí-lo.

## 🎯 Funcionalidades

### Criação Inteligente de Feature Branch
- Criar branch GitFlow no formato `feature/nome-da-funcionalidade`
- Detectar automaticamente branch base apropriada (develop/main)
- Validar nomenclatura seguindo convenções GitFlow
- Configurar tracking com repositório remoto quando disponível

### Integração com o especialista GitFlow
- Consultar especialista para análise do repositório atual
- Receber estratégia de branching personalizada
- Validar compliance com workflows da equipe
- Guidance contextual para desenvolvimento

### Session Management Automático
- Criar diretório `.agents/onion/sessions/<feature-slug>/` automaticamente
- Gerar `context.md` com metadados da feature
- Criar `plan.md` com template de desenvolvimento
- Integração opcional com ClickUp tasks existentes

## 🚀 Como Usar

```bash
/onion-git-feature-start "nome-da-funcionalidade"
```

### Exemplos de Nomenclatura
```bash
/onion-git-feature-start "implement-oauth-authentication"
/onion-git-feature-start "add-user-dashboard-filters"  
/onion-git-feature-start "fix-payment-validation"
/onion-git-feature-start "update-api-documentation"
```

## 🤖 Integração com o especialista GitFlow

Para cada nova feature:

1. **Consultar o especialista GitFlow** para análise do estado atual do repositório
2. **Receber estratégia** de criação de branch baseada no contexto
3. **Validar nomenclatura** e compliance com padrões da equipe
4. **Executar criação** seguindo as recomendações do especialista
5. **Configurar ambiente** de desenvolvimento otimizado

Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/gitflow-specialist.md` e atue como esse especialista para analisar o repositório, definir a estratégia de criação de branch e validar a nomenclatura."

## 📋 Processo de Criação

### Validações Pré-Criação
- **Parameter check**: Verificar se nome da feature foi fornecido
- **Repository check**: Confirmar que GitFlow está inicializado  
- **Status check**: Garantir working directory limpo
- **Naming validation**: Validar convenções de nomenclatura

### Criação da Branch
- **Base detection**: Identificar branch base apropriada (develop)
- **Branch creation**: Criar `feature/nome` baseada na develop
- **Remote setup**: Configurar tracking se repositório remoto disponível
- **Checkout**: Trocar para a nova branch automaticamente

### Setup do Ambiente
- **Session creation**: Criar estrutura `.agents/onion/sessions/`
- **Context setup**: Gerar arquivos de contexto e planejamento
- **ClickUp integration**: Conectar com tasks existentes se detectadas
- **Development ready**: Ambiente pronto para desenvolvimento

## ⚙️ Estrutura Criada

### Branch GitFlow
```
feature/nome-da-funcionalidade ← nova branch
├── baseada em: develop (branch de desenvolvimento)  
├── tracking: origin/feature/nome (se remoto disponível)
└── estado: pronta para desenvolvimento
```

### Session Directory
```
.agents/onion/sessions/nome-da-funcionalidade/
├── context.md          # Metadados e objetivos da feature
├── plan.md            # Plano de desenvolvimento estruturado  
├── notes.md           # Notas de desenvolvimento
└── (outros arquivos conforme necessário)
```

## ✅ Resultado da Execução

Após execução bem-sucedida:

- ✅ **Feature branch criada** no padrão GitFlow
- ✅ **Branch checkout realizado** automaticamente
- ✅ **Session configurada** com estrutura completa
- ✅ **Ambiente pronto** para desenvolvimento
- ✅ **Próximos passos** fornecidos contextualmente

## 🔄 Fluxo de Desenvolvimento Sugerido

Após criar a feature:

1. **Desenvolvimento**: Implementar funcionalidade na branch criada
2. **Commits frequentes**: Usar conventional commits para histórico limpo
3. **Push regular**: `git push` para backup e colaboração  
4. **Compartilhamento**: `/onion-git-feature-publish` para code review
5. **Finalização**: `/onion-git-feature-finish` quando completo

## ⚠️ Tratamento de Problemas

### GitFlow não inicializado
**Problema**: Repository não tem GitFlow configurado
**Solução**: Execute `/onion-git-init` primeiro para configurar GitFlow

### Nome de feature inválido
**Problema**: Nome não segue convenções ou contém caracteres inválidos
**Solução**: Use nomes descritivos em kebab-case (letras, números, hífen)

### Working directory não limpo
**Problema**: Há mudanças não commitadas no repositório
**Solução**: Commit ou stash mudanças antes de criar nova feature

### Feature branch já existe
**Problema**: Já existe branch com mesmo nome
**Solução**: Use nome diferente ou finalize feature existente primeiro

### Branch develop não encontrada
**Problema**: Branch develop não existe (GitFlow mal configurado)
**Solução**: o especialista GitFlow fornecerá estratégia de resolução

## 💡 Melhores Práticas

### Nomenclatura de Features
- **Descritiva**: Nome deve explicar claramente a funcionalidade
- **Kebab-case**: Use hífens para separar palavras
- **Concisa**: Evite nomes muito longos, máximo 50 caracteres
- **Sem prefixos**: Não usar "feature-" pois já está no path da branch

### Desenvolvimento
- **Commits atômicos**: Commits pequenos e focados
- **Conventional commits**: Seguir padrão (feat:, fix:, docs:, etc.)
- **Push frequente**: Backup regular do trabalho
- **Testes**: Implementar testes conforme desenvolvimento

---

*Este comando sempre consulta o especialista GitFlow para garantir criação otimizada e compliance com padrões da equipe.*
