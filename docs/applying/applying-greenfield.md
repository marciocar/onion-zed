# Onion em Projeto Novo (Greenfield)

> Guia passo a passo para aplicar o Sistema Onion em um projeto sem código nem documentação prévia.

---

## Pré-requisitos

- **Zed instalado** (baixe em https://zed.dev/download; em Linux/macOS também via `curl -f https://zed.dev/install.sh | sh`)
- **Provedor de LLM configurado no Zed** — abra o Agent Panel e configure ao menos um provider (Anthropic, OpenAI, ou via `claude-acp`/Zed AI). Sem isso as skills e o `spawn_agent` não executam
- Git instalado
- Acesso ao repositório do Onion (para copiar `.agents/`, `.zed/settings.json` e `AGENTS.md`)
- Decisão sobre Task Manager: Jira, ClickUp, Asana, Linear ou `none` (operação offline)
- Decisão sobre se o projeto exige compliance regulatório (se sim, ver também [applying-regulated.md](./applying-regulated.md))

---

## Quando este guia se aplica

- Projeto que **ainda não existe** ou **acabou de ser inicializado** (`git init` recente)
- Sem README, sem código fonte, sem arquitetura prévia
- Equipe quer começar com Onion desde o dia zero

Se o projeto **já tem código**, consultar [applying-legacy.md](./applying-legacy.md).

---

## Passo 1 — Estrutura inicial do projeto-alvo

```bash
# No diretório do projeto-alvo
mkdir meu-projeto && cd meu-projeto
git init
```

---

## Passo 2 — Copiar o Onion

Copiar do repositório do Onion para o projeto-alvo:

- `.agents/` integral (skills em `.agents/skills/`, specialists em `.agents/onion/specialists/`, utils, templates, prompts, sessions estrutura)
- `.zed/settings.json` (permissões `agent.tool_permissions` e `context_servers` — ajustar conforme passo 3)
- `docs/meta-specs/` (constituição do framework — pode ser referenciada via symlink ou cópia)
- `docs/sdaal/` (KB do padrão SDAAL)
- Templates de `docs/business-context/README.md`, `docs/technical-context/README.md`, `docs/compliance-context/README.md`
- `AGENTS.md` (rules nativas Zed — ajustar conforme passo 4)
- `.env.example` (renomear para `.env` e configurar)

Estrutura resultante mínima no projeto-alvo:

```
meu-projeto/
├── .agents/                # Operacional Onion (skills + specialists + utils)
├── .zed/
│   └── settings.json       # tool_permissions + context_servers (MCP)
├── docs/
│   ├── business-context/   # Vazio inicialmente (será populado)
│   ├── technical-context/  # Vazio inicialmente
│   ├── compliance-context/ # Vazio (criar apenas se regulado)
│   ├── meta-specs/         # Constituição (cópia ou referência)
│   ├── sdaal/              # Referência do padrão SDAAL
│   └── knowledge-base/     # Vazio inicialmente
├── AGENTS.md
└── .env (não commitado)
```

> **Worktree trust obrigatório:** ao abrir o projeto-alvo no Zed pela primeira vez, confirme o prompt de confiança da worktree. Sem isso o Zed **não descobre** as skills locais em `.agents/skills/` nem inicializa os `context_servers` declarados em `.zed/settings.json`.

---

## Passo 3 — Configurar integrações

```bash
/onion-meta-setup-integration
```

A skill guiará a configuração de:

- `TASK_MANAGER_PROVIDER` (jira | clickup | asana | linear | none)
- Variáveis específicas do provider escolhido
- MCPs aplicáveis declarados em `.zed/settings.json` (`context_servers`) — ex. ClickUp MCP standalone. (Jira usa REST direto via `fetch`/`terminal`, sem MCP.)

Se for operar offline (sem Task Manager): definir `TASK_MANAGER_PROVIDER=none`. As skills `/onion-product-*` continuarão funcionando mas não persistirão em Task Manager externo.

---

## Passo 4 — Adaptar AGENTS.md ao projeto-alvo

O `AGENTS.md` do Onion (rules lidas nativamente pelo Zed) é genérico. Para o projeto-alvo, ajustar:

1. Substituir descrição "Sistema Onion" pela descrição do projeto-alvo
2. Manter as seções de Task Manager Abstraction (são úteis)
3. Adicionar contexto específico do projeto (stack pretendida, equipe, restrições)
4. Manter referência às meta-specs do Onion como guia normativo

---

## Passo 5 — Gerar contexto de negócio inicial

```bash
/onion-docs-build-business-docs
```

A skill passará por:

1. **Descoberta** — analisa o que existe (README inicial, materiais externos se fornecidos)
2. **Discussão** — pergunta visão, personas, público-alvo, modelo de negócio
3. **Geração** — preenche `docs/business-context/` seguindo a estrutura em [docs/business-context/README.md](../business-context/README.md)

Em greenfield, este passo define a base estratégica antes de qualquer código.

---

## Passo 6 — Gerar contexto técnico inicial

```bash
/onion-docs-build-tech-docs
```

A skill passará por:

1. **Descoberta** — escaneia o pouco que existe (arquivos de config, decisões inicialmente declaradas)
2. **Discussão** — pergunta sobre stack pretendida, padrões arquiteturais, restrições, trade-offs
3. **Geração** — preenche `docs/technical-context/` com ADRs iniciais, charter, AI development guide

Em greenfield, este passo define a arquitetura intencional antes da implementação.

---

## Passo 7 — Iniciar primeiro ciclo de desenvolvimento

### Camada Produto

```bash
/onion-product-collect    # Coletar ideias iniciais de features
/onion-product-refine     # Refinar via perguntas
/onion-product-spec       # Criar spec da primeira feature
/onion-product-task       # Decompor em tasks
/onion-product-estimate   # Estimar story points
/onion-product-feature    # Criar no Task Manager (se configurado)
```

### Camada Engenharia

```bash
/onion-engineer-plan      # Planejar implementação
/onion-engineer-start     # Criar sessão de desenvolvimento
/onion-engineer-work      # Executar fase atual
# ... iterar work até pronto
/onion-engineer-pre-pr    # Validação pré-PR
/onion-engineer-pr        # Abrir Pull Request
```

---

## Passo 8 — Manter contextos atualizados

A cada mudança significativa de produto ou arquitetura:

```bash
/onion-docs-build-business-docs   # Atualizar business context
/onion-docs-build-tech-docs       # Atualizar technical context
/onion-docs-build-index           # Reconstruir INDEX
```

---

## Compliance opcional

Se durante a evolução do projeto surgir requisito regulatório:

```bash
mkdir -p docs/compliance-context
# Copiar README de docs/compliance-context/README.md do Onion
/onion-docs-build-compliance-docs
```

Detalhes em [applying-regulated.md](./applying-regulated.md).

---

## Troubleshooting

### Skill não é reconhecida

- Verificar que `.agents/` foi copiado integralmente (skills são filhas diretas de `.agents/skills/`, catálogo flat)
- Confirmar que o **worktree trust** foi concedido ao abrir o projeto no Zed
- Confirmar que o Zed está com o projeto-alvo aberto como worktree
- Recarregar a janela do Zed

### Task Manager não responde

- Verificar variáveis em `.env`
- Rodar `/onion-meta-setup-integration` novamente
- Confirmar que o `context_server` (MCP) correspondente está declarado em `.zed/settings.json` e que a worktree foi confiada
- Validar token/credenciais com o provider

### Specialist não encontrado

- Verificar que `.agents/onion/specialists/` foi copiado
- Confirmar que o arquivo do specialist existe (`.agents/onion/specialists/<slug>.md`) e que o `spawn_agent` aponta para o caminho correto

---

## Checklist de "primeira skill útil"

Considera-se Onion operacional no projeto-alvo quando:

- [ ] `.agents/` copiado integralmente
- [ ] `.zed/settings.json` presente (`agent.tool_permissions` + `context_servers`)
- [ ] `AGENTS.md` presente na raiz
- [ ] Worktree trust concedido no Zed (skills locais e MCPs descobertos)
- [ ] `.env` configurado com `TASK_MANAGER_PROVIDER`
- [ ] `/onion-meta-setup-integration` executado sem erros
- [ ] `/onion-docs-build-business-docs` gerou `business-context/` populado
- [ ] `/onion-docs-build-tech-docs` gerou `technical-context/` populado
- [ ] Primeiro `/onion-product-task` criou task com sucesso (ou foi processado offline)
- [ ] Primeiro `/onion-engineer-start` criou sessão em `.agents/onion/sessions/`

---

**Próximo guia**: [Onion em projeto legado](./applying-legacy.md) | [Onion em projeto regulado](./applying-regulated.md)
