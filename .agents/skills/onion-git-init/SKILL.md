---
name: onion-git-init
description: Inicializar repositório com GitFlow e convenções padrão. Use quando precisar configurar GitFlow em um repositório, detectando main/master automaticamente, criando a branch develop e configurando os prefixos feature/release/hotfix.
disable-model-invocation: true
---

# 🔧 Git Flow - Inicialização

Configurar repositório Git com GitFlow seguindo as melhores práticas. Detectar automaticamente se deve usar `main` ou `master` como branch principal e configurar todas as branches e convenções necessárias.

## 🎯 Funcionalidades

### Detecção Automática Inteligente
- Verificar se Git Flow já está inicializado  
- Detectar branch principal existente (main/master) automaticamente
- Configurar develop branch baseado na convenção detectada
- Validar se repositório está em estado adequado para inicialização

### Setup Seguro e Educativo
- Configuração automática de prefixos GitFlow padrão (feature/, release/, hotfix/)
- Verificações de integridade do repositório antes da inicialização
- Criação da branch develop se não existir
- Integração com o especialista GitFlow para guidance personalizada

## 🚀 Como Usar

```bash
/onion-git-init                    # Inicialização completa automática
```

## 🤖 Integração com o especialista GitFlow

Para cada inicialização:

1. **Consultar o especialista GitFlow** para análise do repositório atual
2. **Receber estratégia** de inicialização baseada no contexto
3. **Executar setup** seguindo as recomendações do especialista
4. **Validar configuração** final e fornecer próximos passos

Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/gitflow-specialist.md` e atue como esse especialista para analisar o repositório e definir a estratégia de inicialização do GitFlow."

## 📋 Processo de Inicialização

### Verificações Pré-Inicialização
- **Repository check**: Verificar se estamos em um repositório Git válido
- **Status check**: Garantir que não há mudanças não commitadas
- **Remote check**: Verificar configuração de repositório remoto
- **GitFlow check**: Detectar se já está inicializado

### Configuração Automática
- **Branch detection**: Identificar main/master existente
- **Develop setup**: Criar develop branch baseada na principal  
- **Prefix configuration**: Configurar prefixos padrão GitFlow
- **Validation**: Verificar configuração final

## ⚙️ Configurações Aplicadas

### Branches Principais
```
main/master  (produção) ← detectado automaticamente
develop      (desenvolvimento) ← criado se não existir
```

### Prefixos de Branch
```
feature/     (novas funcionalidades)
release/     (preparação de releases)
hotfix/      (correções urgentes)
```

## ✅ Resultado da Inicialização

Após execução bem-sucedida:

- ✅ **Git Flow configurado** com branches apropriadas
- ✅ **Branch develop criada** e configurada como development branch  
- ✅ **Prefixos definidos** para todos os tipos de branch
- ✅ **Configuração validada** e testada
- ✅ **Próximos passos** fornecidos baseados no contexto

## 🔄 Próximos Passos Sugeridos

Após inicialização, o sistema recomendará:

- **Primeira feature**: `/onion-git-feature-start "nome-da-funcionalidade"`
- **Sincronização**: `/onion-git-sync` se houver repositório remoto
- **Ajuda contextual**: `/onion-git-help` para entender os workflows disponíveis

## ⚠️ Tratamento de Problemas

### Repository não é Git
**Problema**: Pasta atual não é um repositório Git  
**Solução**: Execute `git init` primeiro, depois `/onion-git-init`

### GitFlow já inicializado  
**Problema**: GitFlow já está configurado
**Resultado**: Mostra configuração atual e próximos passos sugeridos

### Branch develop conflitante
**Problema**: Já existe branch develop com conteúdo divergente
**Solução**: o especialista GitFlow fornecerá estratégia de resolução

### Repositório remoto não configurado
**Problema**: Não há origin configurado
**Resultado**: Configuração local apenas, com sugestão de setup remoto

---

*Este comando sempre consulta o especialista GitFlow para garantir inicialização otimizada para seu contexto específico.*
