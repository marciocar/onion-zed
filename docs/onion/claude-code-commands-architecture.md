# 🎮 Arquitetura de Skills - Sistema Onion

> ⚠️ Doc legado — em revisão para o modelo Zed (ver [ADR 0001](../meta-specs/adr/0001-zed-native-port.md)). No modelo nativo Zed os antigos comandos `/cat/cmd` viraram **skills** `/onion-<categoria>-<comando>` em `.agents/skills/`. Use [commands-guide.md](commands-guide.md) e [tools-reference.md](tools-reference.md) como referência atual.

Este documento explica como funcionam as skills do Sistema Onion e a diferença crítica entre **Skills do Zed** (invocadas no Agent Panel) e comandos de terminal.

## ⚡ **CONCEITO FUNDAMENTAL: Claude Code Commands**

### 🎯 **O que são Claude Code Commands**
Claude Code Commands são comandos personalizados executados diretamente no **chat da Claude Code**, conforme a [documentação oficial da Claude Code](https://docs.claude.com/en/docs/claude-code/slash-commands).

### ✅ **Como Usar (CORRETO)**
```markdown
# No chat da Claude Code:
/onion-git-init                      # Inicializar Git Flow
/onion-git-feature-start "login"     # Criar feature branch
/onion-engineer-work "implement API" # Iniciar desenvolvimento
/onion-product-task "add dashboard"  # Criar task no ClickUp
```

### ❌ **Como NÃO Usar (INCORRETO)**
```bash
# ❌ NO TERMINAL - NÃO FUNCIONA:
$ /onion-git-init                    # Comando não encontrado
$ ./onion-git-feature-start          # Arquivo não executável
$ bash /onion-git-init               # Não é script bash direto
```

---

## 🏗️ **Arquitetura do Sistema**

### 📁 **Estrutura de Arquivos**
```
.agents/skills/
├── git/
│   ├── init.md               # Define /onion-git-init command
│   ├── help.md               # Define /onion-git-help command
│   └── feature/
│       ├── start.md          # Define /onion-git-feature-start command
│       ├── publish.md        # Define /onion-git-feature-publish command
│       └── finish.md         # Define /onion-git-feature-finish command
├── engineer/
│   ├── start.md              # Define /onion-engineer-start command
│   └── work.md               # Define /onion-engineer-work command
└── product/
    ├── task.md               # Define /onion-product-task command
    └── spec.md               # Define /onion-product-spec command
```

### 🔄 **Fluxo de Execução**

| Passo | Camada | Tecnologia | Função |
|-------|--------|------------|--------|
| 1 | **Interface** | Claude Code Chat | Usuário digita `/onion-git-init` |
| 2 | **Detecção** | Claude Code AI | Reconhece comando personalizado |
| 3 | **Carregamento** | File System | Lê `.agents/skills/onion-git-init.md` |
| 4 | **Interpretação** | Claude Code AI | Analisa workflow definido |
| 5 | **Execução** | Scripts | Executa bash/python dentro do workflow |
| 6 | **UX** | Modern CLI | `.agents/onion/utils/modern-cli-ux.sh` |
| 7 | **Feedback** | Claude Code Chat | Resposta rica e educativa |

---

## 🎯 **Exemplo Detalhado: `/onion-git-init`**

### 📝 **1. Usuário Executa Comando**
```markdown
# No chat da Claude Code:
User: /onion-git-init
```

### 📄 **2. Claude Code Carrega Definição**
```markdown
# Arquivo: .agents/skills/onion-git-init.md
# Define workflow completo de inicialização Git Flow
```

### 🧠 **3. Claude Code AI Interpreta Workflow**
```bash
# Bash scripts embutidos no markdown executam:
# - Detecção master/main
# - Criação develop branch  
# - Configuração Git Flow
# - Validações de segurança
```

### 💬 **4. Resposta Rica no Chat**
```markdown
🔧 GIT FLOW - Modern Initialization Wizard
━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 REPOSITORY ANALYSIS:
   ▶ Primary branch: ✅ main detected
   ▶ Git Flow status: ⚠️ Not initialized
   
❓ Initialize Git Flow in this repository? [Y/n]
```

---

## 🎨 **Vantagens dos Claude Code Commands**

### 🤖 **AI-Powered Intelligence**
-  **Context Awareness**: Claude Code entende arquivos abertos, histórico, projeto
-  **Adaptive Execution**: Workflows se adaptam ao contexto atual
-  **Error Recovery**: Sugestões inteligentes para problemas
-  **Educational Feedback**: Explica o que está fazendo

### 🎯 **Developer Experience**
-  **Natural Interface**: Chat natural, sem sintaxe complexa
-  **Rich Responses**: Feedback visual rico com cores e formatação
-  **Context Preservation**: Mantém estado entre comandos
-  **Universal Access**: Funciona em qualquer pasta, qualquer projeto

### 🔒 **Enterprise Features**
-  **Safety First**: Confirmações para operações críticas
-  **Team Integration**: ClickUp, sessions, project management
-  **Audit Trail**: Histórico completo no chat da Claude Code
-  **Knowledge Sharing**: Comandos compartilháveis entre equipe

---

## 🛠️ **Para Desenvolvedores do Sistema**

### 📝 **Criando Novos Comandos**
```markdown
# 1. Criar arquivo markdown:
.agents/skills/category/command.md

# 2. Definir cabeçalho com metadados
# 3. Escrever workflow em bash/python
# 4. Usar funções UX: cli_header, cli_success_box, etc.
# 5. Testar via chat: /category/command
```

### 🎨 **Padrões UX**
```bash
# Usar funções da biblioteca UX:
source "$HOME/.agents/onion/utils/modern-cli-ux.sh"

cli_header "TITLE" "color"          # Headers consistentes
cli_success_box "TITLE" "message"   # Success feedback  
cli_error_box "TITLE" "message"     # Error handling
cli_progress_start "message"        # Progress indicators
```

### 🔗 **Integrações**
```bash
# ClickUp MCP
clickup_get_task_id_from_session    # Detectar task ativa
clickup_add_comment $TASK_ID        # Adicionar comentário
clickup_update_task $TASK_ID        # Atualizar status

# Session Management  
session_create $NAME                # Criar sessão desenvolvimento
session_update $NAME                # Atualizar contexto
```

---

## 📚 **Recursos Adicionais**

### 🔗 **Documentação Oficial**
- [Claude Code Commands Documentation](https://docs.claude.com/en/docs/claude-code/slash-commands)
- [Sistema Onion Commands Guide](commands-guide.md)
- [Engineering Flows](engineering-flows.md)

### 🎯 **Exemplos Práticos**
- [Practical Examples](practical-examples.md)
- [ClickUp Integration](clickup-integration.md)
- [Tools Reference](tools-reference.md)

### 🚀 **Getting Started**
- [Configuração Inicial](getting-started.md)
- [Primeiro Uso](getting-started.md#primeiro-uso)
- [Troubleshooting](getting-started.md#troubleshooting)

---

## ⚠️ **Avisos Importantes**

### 🔴 **NÃO Confundir Com:**
- ❌ **Bash scripts diretos** (não são executáveis de terminal)
- ❌ **NPM scripts** (não estão no package.json)  
- ❌ **Make targets** (não usam Makefile)
- ❌ **CLI tools globais** (não são instalados via npm/pip)

### ✅ **São Claude Code Commands Porque:**
-  **Executados no chat** da Claude Code
-  **Definidos em markdown** na pasta `.agents/skills/`
-  **Interpretados pela Claude Code AI** com context awareness
-  **Integrados ao ambiente** de desenvolvimento

---

**🎯 Lembre-se sempre: Sistema Onion = Claude Code Commands executados no chat da Claude Code!** 🚀
