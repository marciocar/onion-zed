---
name: onion-docs-reverse-consolidate
description: Engenharia reversa de projetos para gerar documentação consolidada. Use como pré-processador para `/onion-docs-build-tech-docs` quando precisar analisar qualquer codebase (JS, Python, Rust, Go) e produzir um documento consolidado de arquitetura, stack e estrutura.
disable-model-invocation: true
---

Argumentos: `project_path` (caminho do projeto a analisar — obrigatório) e `output_path` (opcional, default `docs/reverse/`). Faça o parsing desses valores a partir dos argumentos recebidos.

# 🔍 Engenharia Reversa de Projetos

Orquestrador de engenharia reversa para gerar documentação consolidada.

## 🎯 Objetivo

Analisar qualquer projeto e gerar documento consolidado para `/onion-docs-build-tech-docs`.

## ⚡ Fluxo de Execução

### Passo 1: Validar Input

```bash
# Verificar path existe
test -d "{{project_path}}" || { echo "❌ Path não existe"; exit 1; }

# Verificar é projeto de software
ls "{{project_path}}"/*.json "{{project_path}}"/*.yaml 2>/dev/null || \
  echo "⚠️ Arquivos de config não detectados"
```

### Passo 2: Detectar Stack

```bash
# Package managers
test -f package.json && STACK="javascript"
test -f requirements.txt && STACK="python"
test -f Cargo.toml && STACK="rust"
test -f go.mod && STACK="go"

# Frameworks
grep -q "react" package.json && FRAMEWORK="react"
grep -q "fastify" package.json && FRAMEWORK="fastify"
grep -q "nx" package.json && MONOREPO="nx"
```

### Passo 3: Analisar Estrutura

Delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/docs-reverse-engineer.md` e atue como esse especialista para: analisar a estrutura do projeto":

1. **Directory Scan**
   - Estrutura de pastas
   - Padrões de arquivos
   - Convenções de naming

2. **Dependency Analysis**
   - Dependências principais
   - DevDependencies
   - Peer dependencies

3. **Architecture Detection**
   - Padrões (MVC, DDD, Clean)
   - Camadas identificadas
   - Pontos de integração

### Passo 4: Gerar Documento

Criar `{{output_path}}/consolidated.md`:

```markdown
# Documentação Consolidada: [Nome do Projeto]

## 📊 Metadados
- Stack: [stack detectado]
- Framework: [framework]
- Monorepo: [sim/não]
- Tamanho: [arquivos/linhas]

## 🏗️ Arquitetura
[Descrição da arquitetura detectada]

## 📁 Estrutura
[Árvore de diretórios comentada]

## 🔧 Componentes Principais
[Lista de módulos/componentes]

## 🔗 Integrações
[APIs, databases, serviços externos]

## 📋 Recomendações
[Sugestões para documentação adicional]
```

### Passo 5: Finalizar

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ ANÁLISE CONCLUÍDA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 Projeto: {{project_path}}

🔍 Stack Detectado:
∟ Linguagem: TypeScript
∟ Framework: React + Fastify
∟ Monorepo: NX
∟ Arquitetura: Clean Architecture

📁 Arquivos Gerados:
∟ docs/reverse/consolidated.md
∟ docs/reverse/structure.json
∟ docs/reverse/dependencies.json

📈 Métricas:
∟ Arquivos analisados: 245
∟ Linhas de código: 12,450
∟ Módulos: 18

🚀 Próximo: /onion-docs-build-tech-docs
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Agente: `.agents/onion/specialists/docs-reverse-engineer.md`
- Próximo passo: /onion-docs-build-tech-docs

## ⚠️ Notas

- Tempo médio: 2-5 minutos dependendo do tamanho
- Funciona com JavaScript, Python, Rust, Go
- Output otimizado para consumo por IA
