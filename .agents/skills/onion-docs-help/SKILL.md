---
name: onion-docs-help
description: Ajuda interativa para comandos de documentação Onion. Use quando o usuário quiser entender quais comandos de documentação existem, quando usar cada um, exemplos práticos ou detalhes profundos de um comando específico.
disable-model-invocation: true
---

Argumento opcional: o nome de um comando específico (ex.: `build-tech-docs`). Sem argumento, exibe o help geral; com argumento, exibe o help detalhado daquele comando. Faça o parsing do primeiro argumento (`$1`).

# 📚 Sistema de Ajuda - Comandos de Documentação

Você é um assistente de IA especializado em **fornecer ajuda interativa para comandos de documentação** do Sistema Onion. Seu papel é educar usuários sobre os comandos disponíveis através de uma interface clara e educativa.

## 🎯 **Funcionalidades**

### **📚 Sistema de Ajuda Educativo:**
- **Help geral** - Visão geral de todos os comandos de documentação
- **Help específico** - Detalhes profundos sobre cada comando individual
- **Orientação contextual** sobre quando usar cada ferramenta
- **Exemplos práticos** para acelerar a adoção
- **Workflows educativos** para dominar os comandos

### **🔍 Inteligência Contextual:**
- Detectar comando específico solicitado
- Fornecer orientação baseada no contexto
- Sugerir próximos passos lógicos
- Explicar diferenças entre comandos similares

---

## 📋 **Comandos Disponíveis**

O Sistema Onion oferece **4 comandos especializados** para documentação:

### **🔧 `/onion-docs-build-tech-docs`** - Documentação Técnica Completa
**Objetivo**: Gerar documentação técnica abrangente para projetos
**Quando usar**: Projetos que precisam de contexto técnico para desenvolvedores
**Workflow**: Análise codebase → Q&A interativo → Múltiplos arquivos técnicos
**Output**: project_charter.md, AGENTS.meta.md, CODEBASE_GUIDE.md, etc.

### **📊 `/onion-docs-build-business-docs`** - Contexto de Negócio
**Objetivo**: Criar inteligência de negócios otimizada para IA
**Quando usar**: Compreender clientes, mercado e estratégia de produto
**Workflow**: Análise produto → Q&A estratégico → Múltiplos arquivos de negócio  
**Output**: CUSTOMER_PERSONAS.md, COMPETITIVE_LANDSCAPE.md, etc.

### **🗂️ `/onion-docs-build-index`** - Construção de Índices
**Objetivo**: Organizar documentação através de índices estruturados
**Quando usar**: Múltiplos projetos precisam de organização centralizada
**Workflow**: Análise estrutura → Geração/atualização de índices
**Sintaxe**: 
- `/onion-docs-build-index` (índice geral de projetos)
- `/onion-docs-build-index <project-name>` (índice específico)

### **🚧 `/onion-docs-refine-vision`** - Refinamento de Visão *(Implementação Futura)*
**Status**: Em desenvolvimento  
**Objetivo**: Refinar e otimizar visão estratégica de projetos
**Disponibilidade**: Próxima versão do Sistema Onion

---

## 🚀 **Uso do Comando**

### **Sintaxe:**
```bash
/onion-docs-help                    # Help geral - todos os comandos
/onion-docs-help [comando]          # Help específico detalhado
```

### **Exemplos:**
```bash
/onion-docs-help                    # Visão geral completa
/onion-docs-help build-tech-docs    # Documentação técnica detalhada  
/onion-docs-help build-business-docs # Contexto de negócio detalhado
/onion-docs-help build-index        # Construção de índices detalhada
/onion-docs-help refine-vision      # Status de implementação futura
```

---

## ⚙️ **Sistema de Detecção de Argumentos**

<arguments>
#$ARGUMENTS
</arguments>

# Detectar comando específico ou help geral
COMANDO_ESPECIFICO="${1:-}"

echo ""
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "📚 SISTEMA DE AJUDA - COMANDOS DE DOCUMENTAÇÃO"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo ""

if [ "$COMANDO_ESPECIFICO" = "build-tech-docs" ]; then
    echo "🔧 **HELP ESPECÍFICO: /onion-docs-build-tech-docs**"
    echo ""
    echo "**📋 Objetivo Detalhado:**"
    echo "Gerador de documentação técnica especializado em criar contexto"
    echo "de projeto abrangente e otimizado para IA. Analisa codebase completo"
    echo "e gera estrutura multi-arquivo para desenvolvedores e sistemas IA."
    echo ""
    echo "**🏗️ Workflow Completo:**"
    echo "   ▶ **Fase 1**: Descoberta do Codebase"
    echo "     ∟ Análise da estrutura do projeto" 
    echo "     ∟ Reconhecimento de padrões arquiteturais"
    echo "     ∟ Descoberta do workflow de desenvolvimento"
    echo ""
    echo "   ▶ **Fase 2**: Q&A Interativo com Usuário"
    echo "     ∟ Perguntas estratégicas sobre arquitetura"
    echo "     ∟ Validação de decisões técnicas importantes" 
    echo "     ∟ Esclarecimento de contexto específico"
    echo ""
    echo "   ▶ **Fase 3**: Geração de Contexto Multi-Arquivo"
    echo "     ∟ project_charter.md (visão e objetivos)"
    echo "     ∟ AGENTS.meta.md (guia de desenvolvimento IA)"
    echo "     ∟ CODEBASE_GUIDE.md (navegação do código)"
    echo "     ∟ BUSINESS_LOGIC.md (regras de negócio)"
    echo "     ∟ API_SPECIFICATION.md (APIs e interfaces)"
    echo ""
    echo "**📚 Argumentos Obrigatórios:**"
    echo "Você deve fornecer links para arquivos, repositórios ou outras"
    echo "fontes de materiais para análise técnica."
    echo ""
    echo "**✅ Quando Usar:**"
    echo "   ▶ Novos desenvolvedores precisam entender o projeto rapidamente"
    echo "   ▶ Sistemas de IA precisam de contexto técnico abrangente"
    echo "   ▶ Decisões técnicas precisam de documentação arquitetural"
    echo "   ▶ Code reviews precisam focar em lógica vs arquitetura"
    echo ""
    echo "**🎯 Exemplo de Uso:**"
    echo '   /onion-docs-build-tech-docs "https://github.com/user/projeto"'
    echo ""

elif [ "$COMANDO_ESPECIFICO" = "build-business-docs" ]; then
    echo "📊 **HELP ESPECÍFICO: /onion-docs-build-business-docs**"
    echo ""
    echo "**📋 Objetivo Detalhado:**"
    echo "Analista de negócios especializado em criar inteligência de negócios"
    echo "abrangente e otimizada para IA. Analisa produto/projeto e gera"
    echo "contexto completo de negócio usando abordagem multi-arquivo."
    echo ""
    echo "**🏗️ Workflow Completo:**"
    echo "   ▶ **Fase 1**: Descoberta do Produto"
    echo "     ∟ Entendimento do produto e proposta de valor"
    echo "     ∟ Pesquisa de mercado e panorama competitivo"
    echo "     ∟ Coleta de inteligência do cliente"
    echo ""
    echo "   ▶ **Fase 2**: Q&A Estratégico com Usuário"
    echo "     ∟ Perguntas sobre visão do produto"
    echo "     ∟ Identificação de personas e concorrentes"
    echo "     ∟ Validação de estratégia de negócio"
    echo ""
    echo "   ▶ **Fase 3**: Geração de Contexto Multi-Arquivo"
    echo "     ∟ CUSTOMER_PERSONAS.md (perfis de cliente)"
    echo "     ∟ CUSTOMER_JOURNEY.md (jornada completa)" 
    echo "     ∟ VOICE_OF_CUSTOMER.md (feedback e padrões)"
    echo "     ∟ COMPETITIVE_LANDSCAPE.md (análise competitiva)"
    echo "     ∟ PRODUCT_STRATEGY.md (estratégia e posicionamento)"
    echo ""
    echo "**📚 Argumentos Obrigatórios:**"
    echo "Você deve fornecer links para materiais de produto, landing pages,"
    echo "documentação de marketing ou outras fontes de contexto de negócio."
    echo ""
    echo "**✅ Quando Usar:**"
    echo "   ▶ Times de vendas precisam alinhar mensagens com mercado"
    echo "   ▶ Sistemas de IA precisam fornecer suporte contextual ao cliente"  
    echo "   ▶ Decisões de produto precisam de contexto completo de mercado"
    echo "   ▶ Planejamento estratégico requer inteligência competitiva"
    echo ""
    echo "**🎯 Exemplo de Uso:**"
    echo '   /onion-docs-build-business-docs "https://empresa.com" "docs/produto/"'
    echo ""

elif [ "$COMANDO_ESPECIFICO" = "build-index" ]; then
    echo "🗂️ **HELP ESPECÍFICO: /onion-docs-build-index**"
    echo ""
    echo "**📋 Objetivo Detalhado:**" 
    echo "Construtor especializado de índices para organização de documentação"
    echo "de múltiplos projetos. Cria estrutura canônica de navegação que"
    echo "funciona como fonte única da verdade para todos os projetos."
    echo ""
    echo "**🏗️ Workflow Simplificado:**"
    echo "   ▶ **Análise**: Examina estrutura de pastas e arquivos existentes"
    echo "   ▶ **Organização**: Identifica projetos e recursos principais" 
    echo "   ▶ **Geração**: Cria/atualiza arquivos index.md estruturados"
    echo ""
    echo "**📚 Sintaxe e Argumentos:**"
    echo "   ▶ **Índice Geral**: /onion-docs-build-index"
    echo "     ∟ Constrói index.md raiz com todos os projetos"
    echo "     ∟ Informações: nome, descrição, ClickUp IDs, repositório"
    echo ""
    echo "   ▶ **Índice Específico**: /onion-docs-build-index <project-name>"  
    echo "     ∟ Reconstrói índice após mudanças estruturais"
    echo "     ∟ Mapeia recursos úteis dentro do projeto específico"
    echo ""
    echo "**✅ Quando Usar:**"
    echo "   ▶ Múltiplos projetos precisam de organização centralizada"
    echo "   ▶ Estrutura de diretórios foi modificada significativamente"
    echo "   ▶ Novos projetos foram adicionados à organização" 
    echo "   ▶ Navegação de documentação precisa ser atualizada"
    echo ""
    echo "**🎯 Exemplos de Uso:**"
    echo "   /onion-docs-build-index                    # Índice geral"
    echo "   /onion-docs-build-index projeto-mobile     # Índice específico"
    echo ""

elif [ "$COMANDO_ESPECIFICO" = "refine-vision" ]; then
    echo "🚧 **HELP ESPECÍFICO: /onion-docs-refine-vision**"
    echo ""
    echo "**📋 Status Atual:**"
    echo "Este comando está em **desenvolvimento ativo** e será incluído"
    echo "em uma próxima versão do Sistema Onion."
    echo ""  
    echo "**🔮 Objetivo Planejado:**"
    echo "Especialista em refinamento de visão estratégica de projetos."
    echo "Analisará documentação existente e facilitará processo colaborativo"
    echo "para refinar e otimizar visão de produto/projeto."
    echo ""
    echo "**🛠️ Funcionalidades Futuras:**"
    echo "   ▶ **Análise de Visão Atual**: Auditoria de documentação estratégica"
    echo "   ▶ **Workshop Guiado**: Facilitação de sessões de refinamento"  
    echo "   ▶ **Alinhamento de Stakeholders**: Validação com partes interessadas"
    echo "   ▶ **Documentação Atualizada**: Geração de artefatos refinados"
    echo ""
    echo "**📅 Timeline Estimado:**"
    echo "Implementação planejada para próximo release do Sistema Onion."
    echo ""
    echo "**💡 Alternativas Atuais:**"
    echo "   ▶ Use /onion-docs-build-business-docs para contexto estratégico"
    echo "   ▶ Use /onion-docs-build-tech-docs para visão técnica de produto"
    echo "   ▶ Combine ambos para contexto abrangente de projeto"
    echo ""

else
    echo "🎯 **HELP GERAL - VISÃO COMPLETA DOS COMANDOS**"
    echo ""
    echo "O Sistema Onion oferece **4 comandos especializados** para"
    echo "documentação inteligente otimizada para IA:"
    echo ""
    echo "**🔧 Documentação Técnica:**"
    echo "   ▶ **/onion-docs-build-tech-docs** - Contexto técnico completo"
    echo "     ∟ Para: Desenvolvedores, sistemas IA, decisões técnicas"
    echo "     ∟ Output: project_charter.md, AGENTS.meta.md, CODEBASE_GUIDE.md"
    echo "     ∟ Uso: /onion-docs-help build-tech-docs (detalhes)"
    echo ""
    echo "**📊 Contexto de Negócio:**" 
    echo "   ▶ **/onion-docs-build-business-docs** - Inteligência de mercado"
    echo "     ∟ Para: Produto, vendas, suporte contextual ao cliente"
    echo "     ∟ Output: CUSTOMER_PERSONAS.md, COMPETITIVE_LANDSCAPE.md"
    echo "     ∟ Uso: /onion-docs-help build-business-docs (detalhes)"
    echo ""
    echo "**🗂️ Organização:**"
    echo "   ▶ **/onion-docs-build-index** - Índices de documentação" 
    echo "     ∟ Para: Múltiplos projetos, navegação centralizada"
    echo "     ∟ Output: index.md estruturados e organizados"
    echo "     ∟ Uso: /onion-docs-help build-index (detalhes)"
    echo ""
    echo "**🚧 Em Desenvolvimento:**"
    echo "   ▶ **/onion-docs-refine-vision** - Refinamento estratégico"
    echo "     ∟ Status: Implementação futura (próximo release)"
    echo "     ∟ Uso: /onion-docs-help refine-vision (roadmap)"
    echo ""
    echo "**🚀 Para Help Específico:**"
    echo "   ▶ /onion-docs-help [comando]     # Detalhes profundos"  
    echo "   ▶ /onion-docs-help build-tech-docs"
    echo "   ▶ /onion-docs-help build-business-docs"
    echo "   ▶ /onion-docs-help build-index"
    echo ""
fi

echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo ""
echo "📚 **Sistema Onion** - Comandos inteligentes para desenvolvimento ágil"
echo "🆘 **Precisa de mais ajuda?** Use /onion-docs-help [comando] para detalhes específicos"
echo ""
