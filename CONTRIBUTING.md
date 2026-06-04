# Contribuindo com o Sistema Onion 🧅

Obrigado por considerar contribuir com o Onion!

O Onion é um **framework nativo do Zed** (`.agents/` + `.zed/`) — instalável em qualquer
projeto (novo, legado ou regulado) para orquestrar produto, engenharia e
compliance. **Não é produto npm, não é distribuído publicamente
e não tem CLI standalone.** Plataforma única: **Zed**.

Por isso, contribuir aqui é **escrever Markdown** (skills, specialists,
knowledge bases e documentação) — não código JavaScript/Node.

---

## 📋 Índice

- [Código de Conduta](#-código-de-conduta)
- [Pré-requisitos](#-pré-requisitos)
- [Estrutura do projeto](#-estrutura-do-projeto)
- [Tipos de contribuição](#-tipos-de-contribuição)
- [Padrões (meta-specs)](#-padrões-meta-specs)
- [Fluxo de Pull Request](#-fluxo-de-pull-request)
- [Idioma e commits](#-idioma-e-commits)
- [Validação](#-validação)

---

## 📜 Código de Conduta

Seja respeitoso, colaborativo, inclusivo e profissional em todas as interações.

---

## 🚀 Pré-requisitos

- **Git**
- **Zed** (plataforma única do framework)

Não há toolchain de build: o Onion é interpretado em runtime pelo Zed a
partir de `.agents/` (Markdown). Não há `package.json`, Node ou pnpm.

```bash
# 1. Fork e clone
git clone https://github.com/your-username/onion-zed.git
cd onion-zed

# 2. Abra no Zed e confie no worktree (worktree trust) — skills e specialists
#    carregam automaticamente a partir de .agents/skills/.
#    Para começar: /onion-warmup e depois /onion
```

---

## 🛠️ Estrutura do projeto

```
onion-zed/
├── .agents/                    # Sistema Onion operacional (nativo Zed)
│   ├── skills/                 # Skills (catálogo flat, onion-<cat>-<cmd>/)
│   │   └── onion-<cat>-<cmd>/
│   │       └── SKILL.md
│   └── onion/
│       ├── specialists/        # Specialists delegáveis via spawn_agent
│       ├── utils/              # Utilitários (incl. task-manager abstraction)
│       ├── templates/          # Fragmentos de template reutilizáveis
│       ├── prompts/            # Fragmentos de prompt compartilhados
│       └── sessions/           # Estado runtime de workflows faseados
├── .zed/
│   └── settings.json           # Modelos, permissões, context_servers (MCP)
├── docs/                       # Documentação (Spec as Code)
│   ├── meta-specs/             # L0 — "constituição" do framework
│   ├── knowledge-base/         # Knowledge bases estruturadas
│   ├── business-context/       # Gerado por /onion-docs-build-business-docs
│   ├── technical-context/      # Gerado por /onion-docs-build-tech-docs
│   └── onion/                  # Guias e referências
└── AGENTS.md                   # Rules nativas do Zed (lidas automaticamente)
```

---

## 🤝 Tipos de contribuição

- **🐛 Bugs** — abra uma issue com: skill/specialist envolvido, o que aconteceu,
  comportamento esperado, passos de reprodução.
- **✨ Novas skills/specialists** — use os criadores do próprio framework:
  `/onion-meta-create-skill`, `/onion-meta-create-agent`. Eles já
  aplicam os padrões das meta-specs.
- **📚 Documentação e knowledge bases** — correções, clareza, exemplos,
  `/onion-meta-create-knowledge-base`.
- **🔌 Integrações (Task Manager)** — novos adapters seguindo o padrão SDAAL em
  `.agents/onion/utils/task-manager/` (ver `docs/meta-specs/integrations.md`).

---

## 📏 Padrões (meta-specs)

As meta-specs L0 em `docs/meta-specs/` são a **fonte canônica** de padrões.
Consulte antes de criar/alterar artefatos:

| Você vai mexer em… | Consulte |
|---|---|
| Specialist | [`agents.md`](docs/meta-specs/agents.md) — frontmatter mínimo, naming kebab-case, limite ≤300 linhas |
| Skill | [`commands.md`](docs/meta-specs/commands.md) — frontmatter Zed, catálogo flat, workflows faseados, limite ≤500 linhas |
| Arquitetura/estrutura | [`architecture.md`](docs/meta-specs/architecture.md) — framework instalável, dependências permitidas |
| Idioma/estilo/naming | [`code-standards.md`](docs/meta-specs/code-standards.md) |
| Integração externa | [`integrations.md`](docs/meta-specs/integrations.md) — adapters, `.env`, `context_servers` |

Pontos-chave:

- **Tamanho skills**: ≤500 linhas (hard limit). Excedeu? Extraia para `docs/knowledge-base/`
  ou `.agents/onion/templates/`.
- **Tamanho specialists**: ≤300 linhas (hard limit). Excedeu? Extraia para KB e mantenha
  como persona enxuta.
- **Frontmatter de skills**: apenas `name`, `description`, `disable-model-invocation`
  (sem `allowed-tools`, `model`, `category` — campos inexistentes no Zed).
- **Frontmatter de specialists**: apenas `name` e `description`.
- **Tool names** em snake_case Zed: `read_file`, `write_file`, `edit_file`, `terminal`,
  `grep`, `find_path`, `list_directory`, `fetch`, `spawn_agent`, `search_web`.
- **Permissões globais** em `.zed/settings.json` (`agent.tool_permissions`) — nunca por
  skill ou specialist.
- **Sem assunções sobre o projeto-alvo**: nada de path absoluto; o framework é
  instalável em qualquer repo.

---

## 🔀 Fluxo de Pull Request

1. **Branch** a partir de `main` (GitFlow): `feature/...` ou `fix/...`
   (ou use `/onion-git-feature-start`).
2. **Mude** seguindo as meta-specs; atualize docs/índices afetados.
3. **Valide** localmente (ver abaixo).
4. **Commit** com Conventional Commits **em pt-BR** (ver próxima seção).
5. **Abra o PR** com título claro, descrição do quê/porquê, issues relacionadas
   e breaking changes (se houver).

---

## 🌍 Idioma e commits

Convenção do Onion (ver `code-standards.md`):

- **Código, nomes de arquivo, slugs, branches, variáveis**: inglês.
- **Comentários, documentação, mensagens ao usuário**: português brasileiro.
- **Mensagens de commit**: português brasileiro, seguindo
  [Conventional Commits](https://www.conventionalcommits.org/).

```bash
git commit -m "feat(product): adiciona skill de priorização de backlog"
git commit -m "fix(task-manager): corrige detecção de provider ausente no .env"
git commit -m "docs(meta-specs): esclarece convenção de frontmatter Zed"
```

Tipos: `feat`, `fix`, `docs`, `refactor`, `chore`, `test`, `style`, `perf`.

---

## 🧪 Validação

Antes de abrir o PR:

- `/onion-validate-workflow` — completude de workflows.
- Skills `onion-validation` e `onion-patterns` — conformidade de artefatos
  (frontmatter, categorias, limites de tamanho, naming).
- `/onion-meta-metaspec-validate` ou `spawn_agent` → `specialists/metaspec-gate-keeper.md`
  — validação de conformidade arquitetural contra as 5 meta-specs L0.

Checklist:

- [ ] Segue as meta-specs aplicáveis.
- [ ] Frontmatter correto (apenas campos válidos no Zed).
- [ ] Dentro dos limites de tamanho (ou refatorado com extração para KB).
- [ ] Documentação/índices atualizados (`/onion-docs-build-index` se necessário).
- [ ] Commits em pt-BR, Conventional Commits.

---

## 🔗 Links úteis

- [Identidade e visão geral (README)](README.md)
- [Guia de uso no Zed](ONION-ZED-GUIA.md)
- [Índice da documentação](docs/INDEX.md)
- [Meta-specs (constituição)](docs/meta-specs/index.md)
- [ADR 0001 — Port nativo Zed](docs/meta-specs/adr/0001-zed-native-port.md)
- [Guias de aplicação](docs/applying/) — greenfield, legado, regulado

---

**Obrigado por contribuir com o Onion! 🧅**
