---
name: onion-product-feature
description: Criar task de feature no gerenciador configurado para planejamento e backlog. Use quando o usuário quiser registrar uma feature no backlog (status Backlog, tag feature/planning) para organização e priorização, SEM iniciar desenvolvimento. Estima story points automaticamente. Para iniciar dev GitFlow use onion-git-feature-start.
disable-model-invocation: true
---

# 🎯 Criar Feature - Task para Planejamento

O argumento fornecido é o nome ou descrição da feature (`FEATURE_NAME`), lido do prompt do usuário.

Você é um assistente de IA especializado em **criar tasks de feature no gerenciador configurado (via Task Manager abstraction) para planejamento e backlog** seguindo o padrão do Sistema Onion. Seu papel é criar tasks de backlog para organização e priorização sem iniciar desenvolvimento.

## 🎯 **Funcionalidades**

### **📋 Criar Task Backlog:**
- Criar task no gerenciador configurado com tag "backlog" 
- Status: "Backlog" (aguardando planejamento e priorização)
- Projeto/Lista: Mesmo projeto da sessão atual ou projeto padrão
- Auto-detecção de contexto e projeto via Task Manager

### **🔗 Integração Inteligente:**
- Auto-detecção do projeto/lista atual via Task Manager
- Herda contexto da sessão ativa (se houver)
- Links com tasks relacionadas
- Tags apropriadas para categorização
- Suporta múltiplos provedores (ClickUp, Asana, Linear)

---

## 🚀 **Uso da Skill**

### **Sintaxe:**
```bash
/onion-product-feature "nome-ou-descrição-da-feature"
```

### **Examples:**
```bash
/onion-product-feature "implementar-autenticacao-oauth"
/onion-product-feature "adicionar-filtros-avancados-dashboard"  
/onion-product-feature "integrar-payment-gateway-stripe"
```

---

## ⚙️ **Workflow Automático**

### **1. Validação de Parâmetros**
```bash
# Verificar se nome da feature foi fornecido
if [ "$#" -eq 0 ]; then
    echo "❌ ERROR: Feature name required"
    echo "📖 USAGE: /onion-product-feature \"feature-name-or-description\""
    echo ""
    echo "💡 EXAMPLES:"
    echo "  /onion-product-feature \"implement-oauth-authentication\""
    echo "  /onion-product-feature \"add-advanced-dashboard-filters\""
    exit 1
fi

FEATURE_NAME="$1"
# Sanitizar nome da feature (remover caracteres especiais)
FEATURE_SLUG=$(echo "$FEATURE_NAME" | sed 's/[^a-zA-Z0-9]/-/g' | tr '[:upper:]' '[:lower:]' | sed 's/--*/-/g' | sed 's/^-\\|-$//g')

echo "🎯 Creating feature planning task: $FEATURE_NAME"
echo "📝 Feature slug: $FEATURE_SLUG"
```

### **2. Detecção de Contexto via Task Manager**

**IMPORTANTE:** Use Task Manager abstraction para detectar contexto independente do provedor:

```typescript
// Via abstração - funciona para qualquer provedor (ClickUp, Asana, Linear)
const taskManager = getTaskManager();

// Detectar projeto/lista da sessão atual
function getCurrentProjectId() {
  // Tentar obter de sessão ativa
  const sessionContext = readSessionContext();
  if (sessionContext?.taskId) {
    const currentTask = await taskManager.getTask(sessionContext.taskId);
    if (currentTask?.projectId) {
      return currentTask.projectId;
    }
  }
  
  // Fallback: usar projeto padrão configurado
  return taskManager.defaultProjectId;
}

const projectId = getCurrentProjectId();
console.log(`📋 Target project/list ID: ${projectId}`);
```

**Nota:** Se a skill ainda usar código bash direto, atualizar para usar Task Manager abstraction quando possível.

### **3. Criação da Task via Task Manager**
```bash
# Preparar dados da task
TASK_TITLE="🚀 $FEATURE_NAME"

# Descrição da task com contexto
TASK_DESCRIPTION="## 🎯 **Feature para Planejamento**

**Tipo**: Feature Development  
**Status**: Backlog - Aguardando planejamento e priorização  
**Criada via**: /onion-product-feature

---

## 📋 **Descrição**
$FEATURE_NAME

---

## 🔄 **Workflow de Desenvolvimento**

### **Para Iniciar Desenvolvimento:**
\`\`\`bash
# Após planejamento, iniciar desenvolvimento GitFlow:
/onion-git-feature-start \"$FEATURE_SLUG\"

# Ou usar sessão de desenvolvimento:
/onion-engineer-start $FEATURE_SLUG
\`\`\`

### **Workflow Sequencial Recomendado:**
1. **🎯 Planejamento**: Task criada (atual) + detalhamento
2. **🌿 Desenvolvimento**: /onion-git-feature-start $FEATURE_SLUG  
3. **🛠️ Iteração**: /onion-engineer-work
4. **🔄 Finalização**: /onion-git-sync
5. **🚀 Deploy**: /onion-engineer-pr

---

## 📊 **Critérios de Aceitação**
- [ ] Requisitos funcionais detalhados
- [ ] Mockups ou wireframes definidos
- [ ] Critérios de aceitação específicos
- [ ] Estimativas de esforço
- [ ] Dependências identificadas
- [ ] Prioridade definida no roadmap

### **Para Desenvolvimento:**
- [ ] Funcionalidade implementada conforme especificação
- [ ] Testes unitários criados
- [ ] Documentação atualizada
- [ ] Code review aprovado
- [ ] Deploy em ambiente de teste

---

## 🏷️ **Tags e Categorização**
- **Type**: feature
- **Status**: backlog  
- **Priority**: medium (ajustar conforme roadmap)
- **Phase**: planning

**Criada automaticamente pelo Sistema Onion** 🧅"

# Criar task via Task Manager abstraction
console.log("🚀 Creating feature planning task via Task Manager...");

const taskManager = getTaskManager();
const task = await taskManager.createTask({
  name: TASK_TITLE,
  projectId: projectId,
  markdownDescription: TASK_DESCRIPTION,
  status: 'backlog',
  priority: 'medium',
  tags: ['feature', 'backlog', 'planning']
});

const TASK_ID = task.id;
console.log(`✅ Task created: ${TASK_ID}`);
```

### **4. Estimar Story Points (Automático)**

**CRÍTICO:** Após criar task, SEMPRE estimar story points automaticamente.

Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/story-points-framework-specialist.md` e atue como esse especialista", com o conteúdo:

```markdown
Por favor, analise e estime a seguinte feature de backlog:

**Feature:** $FEATURE_NAME
**Descrição:** [descrição da feature]
**Status:** Backlog (planejamento inicial)

Forneça estimativa inicial de story points para planejamento.
```

**Atualizar Task com Estimativa:**

```bash
# Obter estimativa via agente
ESTIMATE_RESPONSE=$(invoke_agent_story_points "$FEATURE_NAME")

# Extrair story points
STORY_POINTS=$(echo "$ESTIMATE_RESPONSE" | extract_story_points)

# Atualizar task com custom field Story Points
if [ "$STORY_POINTS" != "" ]; then
    echo "📊 Updating task with story points: $STORY_POINTS"
    
    // Atualizar custom field via Task Manager
    await taskManager.updateTask(TASK_ID, {
      customFields: {
        'Story Points': STORY_POINTS
      }
    });
    
    // Adicionar comentário com análise via Task Manager
    const ESTIMATE_ANALYSIS = extractAnalysis(ESTIMATE_RESPONSE);
    
    await taskManager.addComment(TASK_ID, 
      '━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n' +
      '📊 ESTIMATIVA INICIAL DE STORY POINTS\n' +
      '━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n\n' +
      `🎲 Story Points: ${STORY_POINTS} pontos\n\n` +
      `⚡ ANÁLISE:\n${ESTIMATE_ANALYSIS}\n\n` +
      '💡 NOTA: Esta é uma estimativa inicial para planejamento. Pode ser refinada durante o refinement.\n' +
      '━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━'
    );
fi
```

### **5. Resultado e Links**
```bash
if [ "$TASK_ID" != "" ] && [ "$TASK_ID" != "null" ]; then
    TASK_URL = task.url; // Via Task Manager abstraction
    
    echo ""
    echo "✅ FEATURE PLANNING TASK CREATED SUCCESSFULLY!"
    echo ""
    echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
    echo ""
    echo "📋 TASK DETAILS:"
    echo "   ▶ Title: $TASK_TITLE"
    echo "   ▶ ID: $TASK_ID"
    echo "   ▶ Status: Backlog"
    echo "   ▶ URL: $TASK_URL"
    echo ""
    echo "🏷️  TAGS: feature, backlog, planning"
    echo "📝 DESCRIPTION: Auto-generated with development workflow"
    echo ""
    if [ "$STORY_POINTS" != "" ]; then
        echo "🎲 STORY POINTS: $STORY_POINTS pontos (estimativa inicial)"
        echo ""
    fi
    echo ""
    echo "🎯 NEXT STEPS:"
    echo "   ∟ Add details: Open $TASK_URL"
    echo "   ∟ Set priority: Adjust based on roadmap"  
    echo "   ∟ Start development: /onion-git-feature-start \"$FEATURE_SLUG\""
    echo ""
    echo "💡 WORKFLOW SEQUENCIAL:"
    echo "   1. 🎯 Planning (current) → 2. 🌿 GitFlow Start → 3. 🛠️ Development → 4. ✅ Done"
    echo ""
    echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
    echo ""
    echo "🌟 Feature '$FEATURE_NAME' ready for planning!"
    
    # Adicionar comentário inicial na task
    INITIAL_COMMENT="🎯 FEATURE BACKLOG PARA PLANEJAMENTO

━━━━━━━━━━━━━━━━━━━━━━━━

✅ TASK SETUP:
   ▶ Feature: $FEATURE_NAME
   ▶ Slug: $FEATURE_SLUG
   ▶ Status: Backlog (Planning)
   ▶ Criada via: /onion-product-feature

🎯 PLANEJAMENTO:
   ▶ Detalhar requisitos funcionais
   ▶ Definir critérios de aceitação
   ▶ Estimar esforço e cronograma
   ▶ Priorizar no roadmap

🚀 PARA DESENVOLVIMENTO:
   ▶ Após planejamento: /onion-git-feature-start \"$FEATURE_SLUG\"
   ▶ Para sessão: /onion-engineer-start $FEATURE_SLUG

📋 WORKFLOW:
   ∟ Planning → GitFlow Start → Development → Done

━━━━━━━━━━━━━━━━━━━━━━━━

⏰ Criada: $(date +'%Y-%m-%d %H:%M:%S') | 🧅 Sistema Onion"

    # Adicionar comentário via Task Manager (graceful degradation)
    // Via Task Manager abstraction
    await taskManager.addComment(TASK_ID, INITIAL_COMMENT).catch(() => {
      console.warn("⚠️  Comment creation failed - task created successfully anyway");
    });
        
else
    console.error("❌ FAILED TO CREATE TASK");
    console.error("");
    console.error("💡 POSSIBLE CAUSES:");
    console.error("   ∟ Task Manager provider not configured");
    console.error("   ∟ Invalid project/list ID or permissions");  
    console.error("   ∟ Network connectivity issues");
    console.error("");
    console.error("🔧 TROUBLESHOOTING:");
    console.error("   ∟ Check TASK_MANAGER_PROVIDER environment variable");
    console.error("   ∟ Verify project/list permissions and ID");
    console.error("   ∟ Execute /onion-meta-setup-integration to configure");
    console.error("   ∟ Try manual task creation as fallback");
    echo ""
    echo "📖 MANUAL FALLBACK:"
    echo "   ∟ Create task manually: '$TASK_TITLE'"
    echo "   ∟ Add tags: feature, backlog, planning"
    echo "   ∟ Set status: Backlog"
    exit 1
fi
```

---

## 🔗 **Integração com Sistema Onion**

### **Separação Clara de Responsabilidades:**
- **`/onion-product-feature`**: Cria task backlog para **planejamento**
- **`/onion-git-feature-start`**: Inicia desenvolvimento **GitFlow** (branch + session)
- **`/onion-git-sync`**: Finaliza desenvolvimento (pós-merge + cleanup)

### **Workflow Sequencial Integrado:**
```bash
1. /onion-product-feature "nova-funcionalidade"      # ← PLANEJAMENTO
   # ... tempo de planejamento, detalhamento, priorização ...
   
2. /onion-git-feature-start "nova-funcionalidade"   # ← DESENVOLVIMENTO GitFlow
   # ... desenvolvimento usando sessões ...
   
3. /onion-git-sync                                  # ← FINALIZAÇÃO
```

### **Quando Usar:**
- ✅ **Criar features para backlog** e roadmap planning
- ✅ **Organizar product backlog** e priorização  
- ✅ **Capturar ideias** de features rapidamente
- ✅ **Setup inicial** de projetos com múltiplas features

### **Quando NÃO usar:**
- ❌ Desenvolvimento imediato (use `/onion-git-feature-start`)
- ❌ Hotfixes urgentes (use `/onion-engineer-hotfix`)  
- ❌ Tasks já existem (use `/onion-engineer-start <feature-slug>`)

---

## ⚠️ **Tratamento de Erros**

### **Erro: Nome da feature não fornecido**
```
❌ ERROR: Feature name required
📖 USAGE: /onion-product-feature "feature-name-or-description"
```

### **Erro: Task Manager falhou**
```
❌ FAILED TO CREATE TASK
🔧 Check TASK_MANAGER_PROVIDER configuration and permissions
📖 Create task manually as fallback
```

### **Erro: Lista não encontrada**
```
❌ ERROR: Unable to detect project/list via Task Manager
💡 Run from active session or configure default list
```

---

## 💡 **Dicas de Uso**

### **✅ Boas Práticas:**
```bash
# Nomes descritivos e específicos
/onion-product-feature "implement-oauth2-authentication-flow"

# Features modulares e focadas  
/onion-product-feature "add-user-profile-avatar-upload"

# Include context quando útil
/onion-product-feature "integrate-stripe-payment-gateway-checkout"
```

### **❌ Evitar:**
```bash
# Muito genérico
/onion-product-feature "melhorias"

# Muito técnico/interno
/onion-product-feature "refactor-class-x"

# Tasks que não são features
/onion-product-feature "fix-bug-payment"  # Use /onion-engineer-hotfix
```

---

**🎯 Criação rápida de features para backlog e planejamento! Para iniciar desenvolvimento GitFlow, use `/onion-git-feature-start [feature-name]`.**
