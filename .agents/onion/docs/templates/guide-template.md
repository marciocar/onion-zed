---
# Claude Code - Guide Template Metadata
template:
  type: guide
  version: 2.0
  category: documentation
  name: "[TIPO DE GUIA]"

context:
  system: "[SISTEMA/COMPONENTE]"
  objective: "[OBJETIVO PRINCIPAL]"
  critical_context: "[CONTEXTOS CRÍTICOS OU LIMITAÇÕES]"
  specific_use_cases: "[CASOS DE USO ESPECÍFICOS]"
  reference_section: "[SEÇÃO ESPECÍFICA]"

guide_metadata:
  guide_type: "[Implementação | Arquitetura | Build/Setup | Troubleshooting | Integração]"
  target_audience: "[Público-alvo do guia]"
  difficulty_level: "[Iniciante | Intermediário | Avançado]"
  estimated_time: "[Tempo estimado para completar]"
  last_update: "[Data]"

ai_assistant:
  use_as_reference: true
  context_aware: true
  suggest_sections: true
---

# 📚 [Título do Guia] - [Sistema/Componente]

**Tipo de Guia:** [Implementação | Arquitetura | Build/Setup | Troubleshooting | Integração]  
**Versão:** [X.Y.Z]  
**Data:** [DD/MM/YYYY]  
**Status:** [Draft | In Review | Production Ready | Deprecated]  
**Progresso:** [X%] [descrição do progresso se aplicável]  
**Responsável:** [Nome/Equipe]

---

## 🎯 **VISÃO GERAL**

### Objetivo Principal
[Descrição clara do que este guia pretende ensinar ou resolver]

### Pré-requisitos
- ✅ **Técnicos**: [Conhecimentos necessários]
- ✅ **Ferramentas**: [Software/versões necessárias]
- ✅ **Ambiente**: [Configurações de ambiente]
- ✅ **Dependências**: [Outros sistemas/componentes]

### Resultados Esperados
Ao final deste guia, você será capaz de:
- [ ] [Resultado específico 1]
- [ ] [Resultado específico 2]
- [ ] [Resultado específico 3]

### Stack Tecnológico
```typescript
// Stack/Tecnologias Principais
[Tecnologia]: [Versão] + [Contexto]
Framework: [Nome] [Versão]
[Categoria]: [Tecnologia] + [Complementos]
Build: [Sistema] + [Ferramentas]
```

---

## 📋 **ÍNDICE**

1. [**Contexto e Problemas**](#contexto-e-problemas)
2. [**Arquitetura/Estrutura**](#arquiteturaestrutura)
3. [**Implementação Passo a Passo**](#implementação-passo-a-passo)
4. [**Configurações Detalhadas**](#configurações-detalhadas)
5. [**Resolução de Problemas**](#resolução-de-problemas)
6. [**Melhores Práticas**](#melhores-práticas)
7. [**Monitoramento e Validação**](#monitoramento-e-validação)
8. [**Referências e Conclusão**](#referências-e-conclusão)

---

## 🚨 **CONTEXTO E PROBLEMAS**

### Problemas Comuns Identificados
[Lista dos problemas que este guia resolve]

1. **[Nome do Problema 1]**: [Descrição e impacto]
2. **[Nome do Problema 2]**: [Descrição e impacto]
3. **[Nome do Problema 3]**: [Descrição e impacto]

### Solução Recomendada
[Descrição da abordagem escolhida e justificativa]

### Cenários de Uso
- **✅ Usar este guia quando:** [Cenários apropriados]
- **❌ NÃO usar quando:** [Cenários inadequados]

---

## 🏗️ **ARQUITETURA/ESTRUTURA**

### Diagrama de Componentes
```
[Diagrama ASCII ou referência a diagrama Mermaid]
┌─────────────────────────────────────────┐
│              [Componente A]              │
│  ┌─────────────┐ ┌─────────────┐        │
│  │[Sub-comp 1] │ │[Sub-comp 2] │        │
│  └─────────────┘ └─────────────┘        │
└─────────────┬───────────────────────────┘
              │
┌─────────────▼───────────────────────────┐
│              [Componente B]              │
└─────────────────────────────────────────┘
```

### Organização de Diretórios
```
[projeto]/
├── [categoria-1]/
│   ├── [sub-categoria]/
│   │   ├── [arquivo-config]
│   │   ├── [arquivo-implementação]
│   │   └── [arquivo-teste]
│   └── [sub-categoria-2]/
├── [categoria-2]/
└── [categoria-3]/
```

### Fluxo de Dados/Operações
```typescript
// Fluxo principal de [operação]
[Etapa 1] → [Validação] → [Processamento] → [Resultado]
     │              │              │              │
     ▼              ▼              ▼              ▼
[Input/Config] [Middlewares] [Core Logic] [Output/Storage]
```

---

## 🔧 **IMPLEMENTAÇÃO PASSO A PASSO**

### Fase 1: [Nome da Fase] 
**Duração Estimada:** [tempo]  
**Objetivo:** [descrição]

#### 1.1 [Sub-etapa]
```bash
# Comandos necessários
[comando 1]
[comando 2]

# Verificação
[comando de verificação]
```

#### 1.2 [Sub-etapa]
```typescript
// Código de implementação
[código TypeScript/JavaScript exemplificativo]
```

### Fase 2: [Nome da Fase]
**Duração Estimada:** [tempo]  
**Objetivo:** [descrição]

[Repetir estrutura similar...]

### Decision Tree / Árvore de Decisão
```
[Decisão Principal]?
├── [Condição A]?
│   ├── [Solução A1] → [Resultado A1]
│   └── [Solução A2] → [Resultado A2]
├── [Condição B]?
│   └── [Solução B] → [Resultado B]
└── [Condição C]?
    └── [Solução C] → [Resultado C]
```

---

## ⚙️ **CONFIGURAÇÕES DETALHADAS**

### [Tipo de Configuração 1]
```[formato]
# [Descrição do arquivo/configuração]
[conteúdo de configuração]
```

### [Tipo de Configuração 2]
```typescript
// [Descrição da configuração TypeScript]
interface [ConfigInterface] {
  [propriedade]: [tipo]; // [descrição]
  [propriedade2]: [tipo]; // [descrição]
}

export const [configName]: [ConfigInterface] = {
  [implementação]
};
```

### Variáveis de Ambiente
```bash
# [Categoria de variáveis]
[VAR_NAME]=[valor]                    # [descrição]
[VAR_NAME_2]=[valor]                  # [descrição]

# [Categoria 2]
[VAR_NAME_3]=[valor]                  # [descrição]
```

---

## 🚨 **RESOLUÇÃO DE PROBLEMAS**

### Erro Tipo 1: "[Nome do Erro]"

**Mensagem de Erro:**
```
[Mensagem completa do erro]
```

**Causa:** [Explicação da causa]

**Solução:**
```bash
# [Passos para resolver]
[comando 1]
[comando 2]
```

**Validação:**
```bash
# [Como verificar se foi resolvido]
[comando de verificação]
```

### Erro Tipo 2: "[Nome do Erro]"
[Repetir estrutura similar...]

### Quick Fixes (Soluções Rápidas)
```bash
# [Problema comum 1]
[solução em uma linha]

# [Problema comum 2] 
[solução em uma linha]

# Reset completo (último recurso)
[sequência de comandos para reset]
```

### Troubleshooting Checklist
- [ ] **[Item de verificação 1]**: [Como verificar]
- [ ] **[Item de verificação 2]**: [Como verificar]
- [ ] **[Item de verificação 3]**: [Como verificar]

---

## 🎯 **MELHORES PRÁTICAS**

### Convenções de Código
1. **[Categoria 1]**: [Regra específica]
2. **[Categoria 2]**: [Regra específica]
3. **[Categoria 3]**: [Regra específica]

### Padrões Recomendados
- **✅ FAZER**: [Prática recomendada]
- **❌ EVITAR**: [Prática não recomendada]

### Performance e Otimização
| Cenário | Solução Recomendada | Impacto |
|---------|-------------------|---------|
| [Cenário 1] | [Solução] | [Alto/Médio/Baixo] |
| [Cenário 2] | [Solução] | [Alto/Médio/Baixo] |

### Segurança
- 🔒 **[Aspecto de segurança 1]**: [Implementação]
- 🔒 **[Aspecto de segurança 2]**: [Implementação]

---

## 📊 **MONITORAMENTO E VALIDAÇÃO**

### Métricas de Sucesso
```typescript
interface [NomeMetricas] {
  [metrica1]: {
    target: [valor];
    current: [valor];
    status: 'OK' | 'WARNING' | 'ERROR';
  };
  [metrica2]: {
    target: [valor];
    current: [valor];
    status: 'OK' | 'WARNING' | 'ERROR';
  };
}
```

### Comandos de Validação
```bash
# Validação completa
[comando para validação completa]

# Validações específicas
[comando 1]                           # [O que valida]
[comando 2]                           # [O que valida]

# Health check
[comando de health check]
```

### Logs e Debugging
```typescript
// Configuração de logs para debugging
const logger = [configuração de logger];

// Pontos de debug importantes
logger.debug('[Contexto]', { [dados] });
```

---

## 📚 **REFERÊNCIAS E CONCLUSÃO**

### Status Final Esperado
- ✅ **[Componente 1]**: [Estado final]
- ✅ **[Componente 2]**: [Estado final]  
- ✅ **[Componente 3]**: [Estado final]

### Próximos Passos
1. **[Próximo passo 1]**: [Descrição e contexto]
2. **[Próximo passo 2]**: [Descrição e contexto]
3. **[Próximo passo 3]**: [Descrição e contexto]

### Links Úteis
- 📖 **[Tipo de referência]**: [URL ou caminho]
- 🔧 **[Tipo de ferramenta]**: [URL ou comando]
- 📊 **[Tipo de dashboard]**: [URL]

### Templates Relacionados
- 📄 **[Template relacionado 1]**: [Caminho]
- 📄 **[Template relacionado 2]**: [Caminho]

### Histórico de Mudanças
| Versão | Data | Mudanças | Autor |
|--------|------|----------|-------|
| [X.Y.Z] | [DD/MM/YYYY] | [Descrição das mudanças] | [Nome] |

---

**💡 Dicas de Uso deste Template:**

1. **📝 Adapte as seções** conforme o tipo de guia (implementação, arquitetura, etc.)
2. **🔧 Remova seções** não aplicáveis ao seu contexto específico
3. **📊 Adicione diagramas** Mermaid quando necessário para clareza
4. **🎯 Mantenha foco** no objetivo principal declarado no início
5. **✅ Teste todos** os comandos e códigos antes de publicar
6. **📚 Referencie** outros guias e templates quando apropriado

---

**📅 Última Atualização do Template:** [DD/MM/YYYY]  
**📚 Versão do Template:** 1.0.0  
**🔗 Baseado em:** Análise de [build-guide.md, implementation-guide.md, architecture-guide.md, lib-build-system-guide.md, SCHEMA-INTEGRATION-GUIDE.md] 