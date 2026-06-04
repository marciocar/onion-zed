---
name: onion-engineer-pr-update
description: Atualizar PR existente com mudanças adicionais. Use quando já executou /onion-engineer-pr mas fez mudanças subsequentes e precisa automatizar commit, push e documentação no Task Manager.
disable-model-invocation: true
---

# 🔄 Engineer PR Update

Atualizar um Pull Request existente com mudanças adicionais. Este comando automatiza o processo completo de commit, push e documentação quando você já executou `/onion-engineer-pr` mas fez mudanças subsequentes.

Parâmetros opcionais (parsing): `--type` (força tipo de commit: fix|feat|docs|refactor|style|test|chore), `--message` (mensagem personalizada), `--dry-run` (preview sem executar).

## 🎯 Funcionalidades

### Detecção Automática de Contexto
- Identifica automaticamente a branch de feature ativa
- Detecta mudanças pendentes (staged/unstaged/untracked)
- Valida se existe PR aberto para a branch atual
- Verifica se está na sessão de desenvolvimento correta

### Commit Inteligente e Descritivo
- Analisa arquivos modificados para categorizar mudanças
- Gera mensagem de commit contextual e descritiva
- Suporta diferentes tipos de mudanças (fix, feat, docs, refactor)
- Mantém histórico limpo com commits atômicos

### Sincronização Automática
- Push automático para branch do PR existente
- Atualização do Task Manager configurado com comentário detalhado (conforme `TASK_MANAGER_PROVIDER`)
- Validação de que PR foi atualizado com sucesso
- Timestamp e métricas das mudanças aplicadas

## 🚀 Como Usar

```bash
/onion-engineer-pr-update
```

### Exemplos com Parâmetros Opcionais
```bash
/onion-engineer-pr-update                           # Análise automática + commit inteligente
/onion-engineer-pr-update --type fix                # Força tipo de commit específico
/onion-engineer-pr-update --message "Custom msg"    # Mensagem personalizada
/onion-engineer-pr-update --dry-run                 # Preview sem executar
```

## 🤝 Integração com o Task Manager

Antes de operar com a task, carregue o `.env` e leia `TASK_MANAGER_PROVIDER` (`jira` | `clickup` | `asana` | `linear` | `none`) para rotear ao provider e adapter corretos. Se `none`, pule a atualização remota (apenas commit + push).

### Detecção de Task Ativa
- Lê task ID do arquivo `.agents/onion/sessions/[slug]/context.md`
- Identifica PR existente através da task ou branch
- Valida se task está em status "in progress" com tag "under-review"

### Comentário Automático Padronizado

O comentário de atualização deve documentar: tipo do commit (fix | feat | refactor | docs | chore), hash do commit, arquivos modificados, linhas adicionadas/removidas e descrição das mudanças.

**Roteamento por provider** (carregar `.env` → ler `TASK_MANAGER_PROVIDER` → seguir o adapter):

- **`clickup`** → comentário em formatação Unicode. Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/clickup-specialist.md` e atue como esse especialista...". Adapter: `.agents/onion/utils/task-manager/adapters/clickup.md`. Padrões: `.agents/onion/prompts/clickup-patterns.md`. Abstração MCP de referência: `commentPRUpdated()` em `.agents/onion/utils/clickup-mcp-wrappers.md` (linhas 632-661).
- **`jira`** → comentário em ADF. Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/jira-specialist.md` e atue como esse especialista...". Adapter: `.agents/onion/utils/task-manager/adapters/jira.md`.
- **`asana`** → comentário (story). Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/task-specialist.md` e atue como esse especialista...". Adapter: `.agents/onion/utils/task-manager/adapters/asana.md`.
- **`linear`** → comentário em Markdown. Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/task-specialist.md` e atue como esse especialista...". Adapter: `.agents/onion/utils/task-manager/adapters/linear.md`.
- **`none`** → não persistir comentário remoto.

## ⚙️ Processo Automático

1. **Validação de Contexto**: Confirma branch de feature + sessão ativa
2. **Análise de Mudanças**: Categoriza arquivos modificados por tipo
3. **Geração de Commit**: Cria mensagem contextual e descritiva
4. **Staging Inteligente**: Adiciona apenas arquivos relevantes
5. **Commit & Push**: Executa commit + push para branch do PR
6. **Atualização do Task Manager**: Documenta mudanças com comentário formatado no provider configurado (`TASK_MANAGER_PROVIDER`)
7. **Validação Final**: Confirma que PR foi atualizado com sucesso

## 🧠 Detecção Inteligente de Tipos

### Tipos de Commit Auto-Detectados
- **fix**: Correções de bugs, patches, hotfixes
- **feat**: Novas funcionalidades, enhancements
- **docs**: Mudanças apenas em documentação
- **refactor**: Refatoração sem mudança de funcionalidade
- **style**: Formatação, linting, style fixes
- **test**: Adição ou correção de testes
- **chore**: Tarefas de manutenção, configuração

### Análise de Arquivos Modificados
```markdown
## Categorização Automática:
- `.agents/skills/` → "feat/fix: Comando updates"
- `docs/` → "docs: Documentation updates"
- `tests/` → "test: Test updates"
- `*.md` (session files) → "chore: Session documentation"
- Múltiplos tipos → "chore: Multiple updates"
```

## ⚠️ Resolução de Problemas

### Problema: "Não há PR ativo para esta branch"
**Solução**: Executar `/onion-engineer-pr` primeiro para criar o PR inicial
```bash
# Se necessário, criar PR primeiro:
/onion-engineer-pr
```

### Problema: "Nenhuma mudança detectada"
**Solução**: Verificar se há arquivos modificados
```bash
git status  # Confirmar mudanças pendentes
```

### Problema: "Branch não está sincronizada"
**Solução**: Sincronizar branch antes de atualizar
```bash
git pull origin [branch-name]      # Sincronizar primeiro
/onion-engineer-pr-update          # Depois atualizar
```

### Problema: "Task do Task Manager não encontrada"
**Solução**: Verificar context.md da sessão ativa
- Confirmar task ID no arquivo `.agents/onion/sessions/[slug]/context.md`
- Validar se task existe e está acessível

## 💡 Casos de Uso Comuns

### 1. Correções Pós-Review
```bash
# Após feedback do code review:
# 1. Fazer correções solicitadas
# 2. Executar:
/onion-engineer-pr-update --type fix
```

### 2. Melhorias Adicionais
```bash
# Após pensar em melhorias:
# 1. Implementar enhancements
# 2. Executar:
/onion-engineer-pr-update --type feat
```

### 3. Documentação Esquecida
```bash
# Após lembrar de documentar:
# 1. Atualizar docs
# 2. Executar:
/onion-engineer-pr-update --type docs
```

### 4. Correções Arquiteturais
```bash
# Como no exemplo atual:
# 1. Implementar correções arquiteturais
# 2. Executar:
/onion-engineer-pr-update --type fix --message "Correção arquitetural - Phase→Subtask sync"
```

## 🔗 Integração com Workflow

### Fluxo Padrão Completo
1. `/onion-product-task` - Criar task no Task Manager configurado
2. `/onion-engineer-start` - Iniciar desenvolvimento  
3. `/onion-engineer-work` - Desenvolver features
4. `/onion-engineer-pre-pr` - Validações finais
5. `/onion-engineer-pr` - Criar Pull Request
6. **`/onion-engineer-pr-update`** - Atualizar PR com mudanças adicionais (quantas vezes necessário)
7. Merge do PR → Auto-sync `/onion-git-sync`

### Compatibilidade com Comandos Existentes
- ✅ Funciona após `/onion-engineer-pr`
- ✅ Integra com `/onion-engineer-work` progress tracking
- ✅ Compatível com `/onion-git-sync` automático pós-merge
- ✅ Respeita mapeamento Phase→Subtask do context.md

---

**🎯 VALOR AGREGADO: Este comando elimina o processo manual de atualização de PRs, automatizando commit inteligente, push, e documentação no Task Manager configurado em uma única operação otimizada.**

## 📈 Benefícios

- ⚡ **Automação completa** do processo de update
- 🧠 **Commits inteligentes** com mensagens contextuais
- 📝 **Documentação automática** no Task Manager configurado
- 🔄 **Consistência** no workflow de PRs
- ⏰ **Economia de tempo** significativa
- 🎯 **Redução de erros** manuais
