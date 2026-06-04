---
name: onion-meta-create-agent-express
description: Cria um SPECIALIST do Onion de forma rápida e simplificada em .agents/onion/specialists/<slug>.md. Use quando precisar gerar um novo especialista de IA nativo Zed com setup mínimo, sem o fluxo completo.
disable-model-invocation: true
---

# ⚡ Criar Specialist (Express)

Versão rápida do criador de specialists nativos Zed. Para o fluxo completo com
validações detalhadas, use `/onion-meta-create-agent`.

## Requisitos do Usuário
<requirements>
{{descrição livre do que o specialist deve fazer}}
</requirements>

## Processo

### 1. Entender o Propósito
- Qual é a responsabilidade principal do specialist?
- Que tarefas ele executará?
- O que o torna especializado (uma só responsabilidade)?

### 2. Definir Configuração
- **Slug**: identificador kebab-case (ex.: `payment-validator`).
- **Description**: clara e concisa, com o gatilho de delegação ("delegue
  quando…").

### 3. Tools Nativas do Zed

O specialist herda as permissões **globais** de `.zed/settings.json`. No corpo
do prompt, cite apenas as tools que ele usa, em snake_case Zed:

- **Arquivos**: `read_file`, `write_file`, `edit_file`
- **Busca/navegação**: `find_path`, `grep`, `list_directory`
- **Execução/delegação**: `terminal`, `spawn_agent`
- **Web**: `fetch`, `search_web`
- **Diagnóstico**: `diagnostics`
- **MCP**: ferramentas expostas via `context_servers` (ex.: ClickUp)

> Não existe campo `tools:` por specialist no Zed — escopo de tools é global.
> Cite no prompt apenas o subconjunto realmente necessário.

### 4. Projetar o Prompt do Sistema
Crie um prompt que:
- Define claramente papel e expertise.
- Dá instruções passo a passo.
- Lista restrições/diretrizes.
- Especifica o formato de saída.
- Inclui exemplos quando ajudar.

### 5. Criar o Arquivo

Frontmatter **mínimo Zed** (só `name` + `description`):

```markdown
---
name: {{slug}}
description: >
  [Especialização]. Delegue quando [gatilho].
---

[Prompt do sistema: papel, expertise, processo, regras, output]
```

> **Não** inclua `model:`, `tools:`, `category:`, `color:` — são ignorados pelo
> Zed. Extensão deve ser `.md`.

### 6. Gravar e Confirmar

```bash
write_file .agents/onion/specialists/{{slug}}.md
wc -l .agents/onion/specialists/{{slug}}.md   # deve ser < 300
```

## Melhores Práticas
- Uma única responsabilidade por specialist.
- Prompts claros e acionáveis; output explícito.
- Cite só as tools necessárias (snake_case Zed).
- < 300 linhas.
- Delegação a outros specialists via `spawn_agent` lendo o `.md` da persona —
  nunca `@agente` (não há registry nomeado no Zed).

## 🔗 Referências

- Fluxo completo: `/onion-meta-create-agent`
- Specialist gerador (opcional): `spawn_agent` → `.agents/onion/specialists/agent-creator-specialist.md`
- Padrões: skills `onion-patterns`, `onion-validation`
- ADR: `docs/meta-specs/adr/0001-zed-native-port.md`

Agora, analise os requisitos e crie o specialist seguindo este processo.
