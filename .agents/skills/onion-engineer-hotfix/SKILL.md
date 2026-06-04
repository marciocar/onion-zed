---
name: onion-engineer-hotfix
description: Emergency workflow completo (task no Task Manager + branch hotfix + desenvolvimento). Use para correções urgentes em produção, executando o fluxo de hotfix end-to-end em um único comando.
disable-model-invocation: true
---

# 🔥 Engineer Hotfix

Parâmetros: `description` (obrigatório — descrição do hotfix), `related_tasks` (opcional — IDs de tasks relacionadas, comma-separated), `tags` (opcional — tags adicionais, comma-separated).

Emergency workflow completo: Task + Branch + Desenvolvimento.

## 🎯 Objetivo

Executar workflow de hotfix end-to-end em um único comando.

## ⚡ Fluxo de Execução

### Passo 1: Validar Input

```bash
# Verificar descrição
if [ -z "{{description}}" ]; then
  echo "❌ Descrição obrigatória"
  exit 1
fi

# Verificar branch atual
CURRENT=$(git branch --show-current)
if [[ ! "$CURRENT" =~ ^(main|master|develop)$ ]]; then
  echo "⚠️ Recomendado: iniciar de main/master"
fi
```

### Passo 2: Criar Task Emergencial (provider-aware)

Detectar o provider ativo lendo `.env`:

```bash
grep TASK_MANAGER_PROVIDER .env
```

Delegar via `spawn_agent` ao specialist correto:

| Provider | Specialist |
|----------|------------|
| `jira` | `specialists/jira-specialist.md` |
| `clickup` | `specialists/clickup-specialist.md` |
| `none` / ausente | `specialists/task-specialist.md` (offline) |

**Prompt para spawn_agent** (adapte o specialist ao provider detectado):

```
"Leia .agents/onion/specialists/<provider>-specialist.md e atue como esse
especialista para: criar uma task/issue emergencial de hotfix com:
- Título: '🔥 HOTFIX: {{description}}'
- Prioridade: máxima (urgent/highest)
- Labels/tags: hotfix, urgent{{tags}}
- Status inicial: Em progresso
- Descrição:
    ## 🚨 Emergency Hotfix
    **Descrição**: {{description}}
    ## 📋 Checklist
    - [ ] Diagnóstico
    - [ ] Implementação
    - [ ] Testes
    - [ ] Deploy
Retorne o ID e URL da task criada."
```

Se `.env` ausente ou `TASK_MANAGER_PROVIDER=none`: registrar ID como `local` no context.md da sessão.

### Passo 3: Criar Branch Hotfix

```bash
# Garantir main atualizada
git checkout main
git pull origin main

# Criar hotfix branch
VERSION=$(cat package.json | grep version | head -1 | awk -F'"' '{print $4}')
PATCH=$(echo $VERSION | awk -F. '{print $1"."$2"."$3+1}')
BRANCH="hotfix/$PATCH-$(echo '{{description}}' | tr ' ' '-' | tr '[:upper:]' '[:lower:]' | head -c 30)"

git checkout -b $BRANCH
```

### Passo 4: Setup Session

```bash
# Criar sessão de desenvolvimento
mkdir -p .agents/onion/sessions/hotfix-$(date +%Y%m%d)/

# Criar context.md
cat > .agents/onion/sessions/hotfix-$(date +%Y%m%d)/context.md << EOF
# Hotfix Context

## Task
- Provider: [provider do .env]
- ID: [task_id criado]
- URL: [url da task]

## Branch
- Nome: $BRANCH
- Base: main

## Descrição
{{description}}
EOF
```

### Passo 5: Iniciar Desenvolvimento

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔥 HOTFIX INICIADO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Task: [URL da task no Task Manager]
🌿 Branch: hotfix/X.X.X-description

⚡ Próximos Passos:
1. Implementar correção
2. Testar localmente
3. /onion-engineer-pre-pr
4. /onion-git-hotfix-finish
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 📤 Output Esperado

### Sucesso

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ HOTFIX SETUP COMPLETO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Task Manager ([provider]):
∟ Task: 🔥 HOTFIX: {{description}}
∟ ID: [task_id]
∟ Status: In Progress
∟ Priority: Urgent

🌿 Git:
∟ Branch: hotfix/1.2.3-fix-description
∟ Base: main
∟ Remote: origin

📁 Session:
∟ Path: .agents/onion/sessions/hotfix-20251124/

🚀 Comandos:
∟ Desenvolver: /onion-engineer-work
∟ Pre-PR: /onion-engineer-pre-pr
∟ Finalizar: /onion-git-hotfix-finish
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Padrões: `.agents/onion/prompts/git-workflow-patterns.md`
- Agente: delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/gitflow-specialist.md` e atue como esse especialista..."

## ⚠️ Notas

- Sempre parte de `main` ou `master`
- Task criada com prioridade máxima
- Merge automático para main E develop no finish
