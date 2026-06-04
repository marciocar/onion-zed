# 🚀 Guia de Início Rápido (nativo Zed)

> **Última atualização**: 2026-06-03 | **Plataforma**: Zed (ver [ADR 0001](../meta-specs/adr/0001-zed-native-port.md))

Bem-vindo ao Sistema Onion! Este guia vai te ajudar a começar rapidamente com as skills em `.agents/skills/`, os specialists em `.agents/onion/specialists/` e a integração com gerenciadores de tarefas através do **Task Manager Abstraction**.

## 📊 Visão Geral

| Componente | Quantidade | Descrição |
|------------|------------|-----------|
| Skills | 81 | `/onion-<categoria>-<comando>`, catálogo flat em `.agents/skills/` |
| Specialists | 49 | personas em `.agents/onion/specialists/`, delegadas via `spawn_agent` |
| Rules | `AGENTS.md` | regras lidas nativamente pelo Zed |
| Config | `.zed/settings.json` | `agent.tool_permissions` + `context_servers` (MCP) |
| Knowledge Bases | — | Documentação estruturada em `docs/knowledge-base/` |

## 🧩 Instalar e preparar o Zed

1. **Instale o Zed** — baixe em https://zed.dev/download (em Linux/macOS: `curl -f https://zed.dev/install.sh | sh`).
2. **Configure um provedor de LLM** — abra o **Agent Panel** do Zed e configure ao menos um provider (Anthropic, OpenAI, ou via `claude-acp`/Zed AI). Sem provider, skills e `spawn_agent` não executam.
3. **Abra o projeto que contém o Onion** (com `.agents/`, `.zed/settings.json` e `AGENTS.md` na raiz).
4. **Conceda o worktree trust** — ao abrir o projeto, o Zed pede confiança na worktree. **É obrigatório**: sem ele o Zed **não descobre** as skills locais em `.agents/skills/` nem inicializa os `context_servers` (MCPs) declarados em `.zed/settings.json`.

> A instalação do framework em um projeto-alvo (copiar `.agents/` + `.zed/settings.json` + `AGENTS.md`) está detalhada em [`docs/applying/`](../applying/).

## 📋 Checklist de Setup

### **✅ Pré-requisitos**
- [ ] **Zed** instalado
- [ ] **Provedor de LLM** configurado no Agent Panel do Zed
- [ ] **Worktree trust** concedido ao projeto
- [ ] Git inicializado no projeto
- [ ] `.agents/`, `.zed/settings.json` e `AGENTS.md` presentes no projeto

### **✅ Configuração de Integrações**

#### **⚙️ Método Recomendado: Skill `/onion-meta-setup-integration`**

O Sistema Onion oferece uma skill interativa para configurar todas as integrações de forma segura:

```bash
# Configuração interativa (recomendado)
/onion-meta-setup-integration

# Ou especificar integração diretamente
/onion-meta-setup-integration task-manager  # Configurar gerenciador de tarefas
/onion-meta-setup-integration jira          # Configurar Jira especificamente
/onion-meta-setup-integration clickup       # Configurar ClickUp especificamente
/onion-meta-setup-integration asana         # Configurar Asana especificamente
/onion-meta-setup-integration linear        # Configurar Linear especificamente
/onion-meta-setup-integration gamma         # Configurar Gamma.App
```

**O que a skill faz:**
- ✅ **Guia passo a passo** na configuração de cada integração
- ✅ **Cria/atualiza `.env`** automaticamente
- ✅ **Valida segurança** (verifica `.gitignore`, protege credenciais)
- ✅ **Testa conectividade** quando aplicável
- ✅ **Fornece instruções** específicas para cada provedor

**Integrações suportadas:**
- **Task Managers**: Jira, ClickUp, Asana, Linear (via Task Manager Abstraction)
- **Gamma.App**: API para apresentações
- **PostgreSQL**: Banco de dados

#### **📝 Método Alternativo: Configuração Manual**

Se preferir configurar manualmente, edite o arquivo `.env`:

A primeira variável define **qual provedor está ativo**. Configure apenas o bloco
do provedor escolhido — os demais ficam comentados.

```bash
# ═══════════════════════════════════════
# GERENCIADOR DE TAREFAS (escolha UM provedor)
# ═══════════════════════════════════════
TASK_MANAGER_PROVIDER=jira  # jira | clickup | asana | linear | none

# ─── Opção: Jira ───
JIRA_HOST=https://your-domain.atlassian.net
JIRA_EMAIL=you@example.com
JIRA_API_TOKEN=xxxxx
# JIRA_PROJECT_KEY=PROJ
# JIRA_AUTH_TYPE=basic   # basic | bearer
# JIRA_API_VERSION=3     # 3 (Cloud) | 2 (Server/DC)

# ─── Opção: ClickUp ───
# CLICKUP_API_TOKEN=pk_xxxxx
# CLICKUP_WORKSPACE_ID=your_workspace_id
# CLICKUP_DEFAULT_LIST_ID=your_list_id

# ─── Opção: Asana ───
# ASANA_ACCESS_TOKEN=1/xxxxx
# ASANA_WORKSPACE_ID=1234567890
# ASANA_DEFAULT_PROJECT_ID=1234567890

# ─── Opção: Linear ───
# LINEAR_API_KEY=lin_api_xxxxx
# LINEAR_TEAM_ID=your_team_id

# ═══════════════════════════════════════
# OUTRAS INTEGRAÇÕES
# ═══════════════════════════════════════
GITHUB_TOKEN=ghp_xxxxx
GAMMA_API_KEY=gm_xxxxx
```

> **💡 Para trocar de provedor:** altere `TASK_MANAGER_PROVIDER`, descomente o
> bloco correspondente e comente o anterior. Nenhum comando ou workflow precisa
> mudar — a abstração resolve o roteamento.

> **💡 Dica:** Use `/onion-meta-setup-integration` para garantir que todas as variáveis estão corretas e o `.env` está protegido no `.gitignore`.

> **Referência**: Veja `.env.example` para todas as variáveis disponíveis.

### **🔄 Task Manager Abstraction - Conceito Central**

O Sistema Onion usa uma **camada de abstração** que permite trabalhar com múltiplos gerenciadores de tarefas sem modificar skills ou workflows. Você escolhe o provedor e todas as skills funcionam automaticamente.

**Como funciona:**
- ✅ **Interface unificada**: Skills como `/onion-product-task` funcionam com qualquer provedor
- ✅ **Troca fácil**: Mude `TASK_MANAGER_PROVIDER` no `.env` e tudo continua funcionando
- ✅ **Fallback gracioso**: Sistema funciona mesmo sem gerenciador configurado (modo offline)

**Provedores suportados:**

| Provedor | Configuração | Specialist / roteamento | Notas |
|----------|--------------|---------------------|-------|
| **Jira** | `TASK_MANAGER_PROVIDER=jira` | `spawn_agent` → `jira-specialist` | REST v3/v2, JQL, ADF, transitions, bulk |
| **ClickUp** | `TASK_MANAGER_PROVIDER=clickup` | `spawn_agent` → `clickup-specialist` | Via ClickUp MCP, formatação Unicode |
| **Asana** | `TASK_MANAGER_PROVIDER=asana` | `spawn_agent` → `task-specialist` (agnóstico) | Notes HTML / plain text |
| **Linear** | `TASK_MANAGER_PROVIDER=linear` | `spawn_agent` → `task-specialist` (agnóstico) | Markdown nativo |
| **None** | `TASK_MANAGER_PROVIDER=none` | `spawn_agent` → `task-specialist` (offline) | Modo local sem sincronização |

**Vantagens da abstração:**
- 🎯 **Flexibilidade**: Escolha o gerenciador que sua equipe já usa
- 🔄 **Portabilidade**: Troque de provedor sem refatorar código
- 🛡️ **Resiliência**: Funciona mesmo se o gerenciador estiver offline
- 🚀 **Consistência**: Mesmos comandos, mesma experiência, qualquer provedor

> **📚 Documentação completa**: `docs/knowledge-base/concepts/task-manager-abstraction.md`

### **✅ Validação**

Após configurar o Task Manager, valide a configuração:

```bash
# Verificar comandos disponíveis
/onion-meta-all-tools  # Deve mostrar comandos disponíveis

# Testar integração de Task Manager (se configurado)
/onion-product-task "Task de teste do sistema"
# → Deve criar task no provedor ativo (Jira, ClickUp, Asana ou Linear)

# Validar conectividade (depende do provedor configurado)
/onion-warmup  # Valida conectividade do Task Manager configurado
```

**Se algo não funcionar:**
- Execute `/onion-meta-setup-integration` novamente para revisar configuração
- Verifique se `.env` está no `.gitignore` (o comando faz isso automaticamente)
- Consulte o roteamento conforme o provedor ativo:
  - `spawn_agent` → `jira-specialist` para problemas com Jira (verifique `JIRA_HOST`, `JIRA_EMAIL`, `JIRA_API_TOKEN`)
  - `spawn_agent` → `clickup-specialist` para problemas com ClickUp (verifique `CLICKUP_API_TOKEN`)
  - Para Asana, verifique a variável `ASANA_ACCESS_TOKEN` no `.env` (roteamento via `spawn_agent` → `task-specialist`)
  - Para Linear, verifique a variável `LINEAR_API_KEY` no `.env` (roteamento via `spawn_agent` → `task-specialist`)
  - Para modo offline, certifique-se que `TASK_MANAGER_PROVIDER=none`

---

## 🎯 Seus Primeiros 5 Minutos

### **1. Criar Sua Primeira Task (1 min)**
```bash
/onion-product-task "Implementar página de sobre da empresa"
```

**Resultado esperado**: Task criada no gerenciador configurado com ID (ex: ABOUT-123)

### **2. Iniciar Desenvolvimento (1 min)**
```bash
/onion-engineer-start
```

**Input quando solicitado**: `ABOUT-123`

**Resultado**: Ambiente configurado, sessão criada, plano gerado

### **3. Desenvolver Funcionalidade (2 min)**
```bash
/onion-engineer-work .agents/onion/sessions/about-page/
```

**Resultado**: Implementação guiada passo-a-passo

### **4. Criar Pull Request (1 min)**
```bash
/onion-engineer-pr
```

**Resultado**: PR criado, Task Manager atualizado com status "in_review"

### **✨ Parabéns!** 
Você completou seu primeiro ciclo completo de desenvolvimento com integração ao Task Manager! 🎉

---

## 🏃‍♂️ Fluxos Rápidos por Cenário

### **🆕 Nova Funcionalidade**
```bash
/onion-product-task "Nova funcionalidade X"      # → Task criada no Task Manager
/onion-engineer-start                           # → Input: TASK-ID  
/onion-engineer-work .agents/onion/sessions/feature-x/ # → Desenvolvimento
/onion-engineer-pr                              # → PR + Task Manager atualizado
```

### **🐛 Correção de Bug**
```bash
/onion-product-collect "Bug: X não funciona"    # → Bug task criada
/onion-engineer-start                           # → Fix mode ativo
/onion-engineer-work "corrigir bug X"           # → Implementação rápida
/onion-engineer-pr                              # → Hotfix PR
```

### **📚 Documentação**
```bash
/onion-docs-build-tech-docs                     # → Docs técnicos
/onion-docs-build-business-docs                 # → Docs de negócio
/onion-docs-build-index                         # → Índice de projetos
```

### **⚡ Emergência**
```bash
/onion-product-collect "CRÍTICO: Sistema fora do ar" # → Priority 1 automático
/onion-engineer-start                                 # → Hotfix mode
/onion-engineer-work "fix crítico"                    # → Solução rápida
/onion-engineer-pr                                    # → Deploy imediato
```

---

## 🎯 Skills Essenciais

### **📋 Mais Usadas (80% dos casos)**
| Skill | Uso | Frequência |
|---------|-----|------------|
| `/onion-product-task` | Criar nova task | 35% |
| `/onion-engineer-start` | Iniciar desenvolvimento | 25% |
| `/onion-engineer-work` | Desenvolver funcionalidade | 20% |
| `/onion-engineer-pr` | Criar Pull Request | 15% |
| `/onion-meta-all-tools` | Ver skills/ferramentas disponíveis | 5% |

### **🔧 Para Situações Específicas**
| Skill | Quando Usar |
|---------|-------------|
| `/onion-product-collect` | Reportar bugs ou ideias rápidas |
| `/onion-product-refine` | Melhorar especificação existente |
| `/onion-product-light-arch` | Esboçar arquitetura inicial |
| `/onion-engineer-pre-pr` | Validações antes do PR |
| `/onion-docs-build-*` | Gerar documentação automática |

---

## 🤖 Specialists - Quando Delegar

Delegue via tool `spawn_agent` apontando para `.agents/onion/specialists/<slug>.md`:

### **🔵 Para Desenvolvimento**
```bash
spawn_agent → python-developer: "implementar API de usuários"    # Python backend
spawn_agent → react-developer: "criar dashboard interativo"      # React frontend
```

### **🧪 Para Testes**
```bash
spawn_agent → test-engineer: "adicionar testes para função X"    # Testes unitários
spawn_agent → test-planner: "estratégia de testes para módulo Y" # Plano de testes
```

### **🔍 Para Pesquisa**
```bash
spawn_agent → research-agent: "melhores práticas OAuth2 2024"    # Pesquisa tecnológica
spawn_agent → code-reviewer: "revisar qualidade do código Z"     # Code review
```

---

## 📊 Integração com Task Manager - Visão Rápida

O Sistema Onion sincroniza automaticamente com o Task Manager ativo (Jira, ClickUp, Asana ou Linear), atualizando status, comentários e tags conforme você desenvolve.

### **Estados Automáticos**
```mermaid
graph LR
    A[/onion-product-task] --> B[to do]
    B --> C[/onion-engineer-start] --> D[in progress]  
    D --> E[/onion-engineer-pr] --> F[in progress + under-review]
    F --> G[Merge] --> H[done]
```

**Status normalizados** (funcionam em qualquer provedor):
- `backlog` → `todo` → `in_progress` → `in_review` → `done`
- `blocked` e `cancelled` também suportados

### **Tags Automáticas**
- **Por tipo**: `feature`, `bug`, `refactor`, `docs`
- **Por status**: `development`, `under-review`, `blocked`
- **Por prioridade**: `urgent`, `high`, `normal`, `low`

### **Comentários Automáticos**
- 🚀 Desenvolvimento iniciado
- 📊 Progresso por fase
- 🔍 PR criado com detalhes
- ✅ Conclusão com métricas

**Nota:** Todos esses recursos funcionam igualmente com Jira, ClickUp, Asana ou Linear através da abstração. A *forma* de cada recurso adapta-se ao provedor — por exemplo, descrições em ADF no Jira (Cloud), Markdown nativo no ClickUp/Linear e notes HTML no Asana —, mas o comportamento do workflow permanece o mesmo.

---

## 🔧 Troubleshooting Rápido

### **❌ Problema: Skill não encontrada**
```bash
# Verificar se o projeto está aberto como worktree no Zed e com trust concedido
# Skills ficam em .agents/skills/ (catálogo flat) e exigem worktree trust
ls -la .agents/skills/

# Listar skills/ferramentas disponíveis
/onion-meta-all-tools
```

> Se as skills não aparecem no Agent Panel, quase sempre é **worktree trust** não concedido ou subpasta indevida dentro de `.agents/skills/` (o catálogo deve ser flat).

### **❌ Problema: Task Manager não conecta**
```bash
# Validar configuração
/onion-warmup

# Verificar provedor configurado primeiro:
echo $TASK_MANAGER_PROVIDER

# Se falhar, verificar variáveis conforme o provedor ativo:

# Para Jira:
echo $JIRA_HOST; echo $JIRA_EMAIL; echo $JIRA_API_TOKEN

# Para ClickUp:
echo $CLICKUP_API_TOKEN; echo $CLICKUP_WORKSPACE_ID

# Para Asana:
echo $ASANA_ACCESS_TOKEN; echo $ASANA_WORKSPACE_ID

# Para Linear:
echo $LINEAR_API_KEY; echo $LINEAR_TEAM_ID
```

### **❌ Problema: Task não encontrada**
```
# Verificar formato do ID
Correto: AUTH-123, BUG-456, PROJ-789
Incorreto: 123, auth123, AUTH123
```

### **❌ Problema: PR falha**
```bash
# Executar validações antes
/onion-engineer-pre-pr

# Se testes falharem, corrigir primeiro
npm test  # ou comando apropriado do projeto
```

---

## 💡 Dicas Pro

### **🚀 Para Eficiência Máxima**
1. **Use as skills frequentes** diretamente no Agent Panel (digite `/onion-` para autocompletar):
   - `/onion-product-task`
   - `/onion-engineer-start`
   - `/onion-engineer-work`
   - `/onion-engineer-pr`

2. **Prepare templates** para tasks comuns:
   ```bash
   /onion-product-task "Feature: Nova página X com layout responsivo e formulário de contato"
   /onion-product-task "Bug: Problema Y em ambiente Z com logs detalhados"
   ```

3. **Monitore métricas** no seu Task Manager:
   - Cycle time por feature
   - Bug rate pós-deploy
   - Review time médio

### **🎯 Para Qualidade**
1. **Sempre execute pre-pr** antes de PR crítico
2. **Use code-reviewer** para mudanças arquiteturais
3. **Documente decisões** importantes com `/onion-docs-build-*`

### **🔄 Para Colaboração**
1. **Tags consistentes** facilitam filtros no Task Manager
2. **Comentários descritivos** em PRs
3. **Sincronização regular** com workspace/projeto configurado

---

## 📚 Próximos Passos

### **📖 Aprofundar Conhecimento**
1. **[Guia de Skills](commands-guide.md)** - Documentação completa
2. **[Referência de Ferramentas](tools-reference.md)** - Todas as ferramentas nativas do Zed
3. **[Fluxos de Engenharia](engineering-flows.md)** - Workflows detalhados  
4. **[Task Manager Abstraction](../knowledge-base/concepts/task-manager-abstraction.md)** - Entenda como funciona a abstração
5. **Adapters por provedor** - Detalhes específicos de cada um em `.agents/onion/utils/task-manager/adapters/` (`jira.md`, `clickup.md`, `asana.md`, `linear.md`)

### **🎯 Cenários Avançados**
1. **[Exemplos Práticos](practical-examples.md)** - Casos reais de uso
2. **[Referência de Specialists](agents-reference.md)** - Specialists disponíveis

### **🔧 Personalização**
1. Configurar webhooks do Task Manager (Jira, ClickUp, Asana ou Linear)
2. Customizar templates de PR
3. Criar dashboards/relatórios específicos no seu gerenciador
4. Ajustar notificações e workflows

---

## 🆘 Suporte e Ajuda

### **📞 Onde Buscar Ajuda**
1. **Skills**: `/onion-meta-all-tools` lista tudo disponível
2. **Status**: `/onion-warmup` valida configuração do Task Manager
3. **Documentação**: Arquivos nesta pasta `docs/`
4. **Task Manager**: Interface web do seu gerenciador (Jira, ClickUp, Asana ou Linear) para validar dados

### **🐛 Reportar Problemas**
Se algo não funciona:
1. Execute `/onion-warmup` e cole o resultado
2. Descreva a skill executada
3. Inclua mensagem de erro completa
4. Mencione ID da task e provedor configurado (Jira, ClickUp, Asana ou Linear)
5. Verifique se `TASK_MANAGER_PROVIDER` está configurado corretamente

### **💬 Comunidade**
- Compartilhe workflows que funcionam
- Sugira melhorias nas skills
- Documente casos especiais descobertos

---

## 🎉 Você Está Pronto!

Agora você tem tudo para ser produtivo com o sistema Onion:

-  **Setup validado** e funcionando
-  **Primeiras skills** executadas com sucesso  
-  **Fluxos principais** compreendidos
-  **Task Manager configurado** e sincronizando (Jira, ClickUp, Asana, Linear ou modo offline)
-  **Troubleshooting** na ponta da língua

**Comece pequeno, pratique os fluxos básicos, e gradualmente explore funcionalidades mais avançadas!**

---

## 🛠️ Troubleshooting Node.js v22.14.0+

### **Problema**: chrome-devtools-mcp não funciona
**Erro comum**: `chrome-devtools-mcp does not support Node v20.x.x`

#### **Solução 1: Verificar versão do Node.js**
```bash
# Verificar versão atual
node --version

# Se for menor que v22.14.0, atualizar via NVM:
nvm install v22.14.0
nvm use v22.14.0
nvm alias default v22.14.0

# Confirmar atualização
node --version  # Deve mostrar v22.14.0
```

#### **Solução 2: Limpar cache do NPX**
```bash
# Limpar cache do npx
rm -rf ~/.npm/_npx/

# Testar novamente
npx chrome-devtools-mcp@latest --version
```

#### **Solução 3: Verificar instalação do Chrome**
```bash
# Ubuntu/Debian
which google-chrome || which chromium-browser

# Se não encontrar, instalar Chrome:
sudo apt update
sudo apt install google-chrome-stable
```

### **Problema**: Skills do Onion não funcionam
**Sintomas**: Skills `/onion-*` não são reconhecidas no Agent Panel

#### **Solução**:
```bash
# 1. Verificar estrutura .agents/skills/ (catálogo flat, sem subpastas)
ls -la .agents/skills/

# 2. Verificar se o projeto está aberto como worktree no Zed
pwd  # Deve estar na raiz com .agents/ e AGENTS.md

# 3. Confirmar worktree trust concedido (skills locais + context_servers dependem disso)

# 4. Delegar ao specialist de Zed
spawn_agent → zed-specialist: "skills /onion-* não funcionam — verificar trust e descoberta"
```

### **Problema**: Integração Task Manager falha
**Sintomas**: Tasks não são criadas/atualizadas

#### **Soluções**:
```bash
# 1. Verificar provedor configurado
echo $TASK_MANAGER_PROVIDER

# 2. Verificar variáveis de ambiente conforme provedor ativo:

# Se usando Jira:
echo $JIRA_HOST; echo $JIRA_EMAIL; echo $JIRA_API_TOKEN

# Se usando ClickUp:
echo $CLICKUP_API_TOKEN; echo $CLICKUP_WORKSPACE_ID; echo $CLICKUP_DEFAULT_LIST_ID

# Se usando Asana:
echo $ASANA_ACCESS_TOKEN; echo $ASANA_WORKSPACE_ID

# Se usando Linear:
echo $LINEAR_API_KEY; echo $LINEAR_TEAM_ID

# 3. Testar conectividade
/onion-warmup

# 4. Delegar ao specialist conforme provedor ativo (via spawn_agent):
spawn_agent → jira-specialist: "integração não funciona"     # Para Jira
spawn_agent → clickup-specialist: "integração não funciona"  # Para ClickUp
# Para Asana/Linear, delegue a task-specialist ou execute /onion-meta-setup-integration <provedor>
```

---

**Sistema Onion** - Desenvolvimento inteligente com IA 🧅 🚀
