<div align="center">

# 🧅 Onion

### Organize produto, engenharia e governança no mesmo ritmo — nativo do **Zed**.

[![Licença: MIT](https://img.shields.io/github/license/marciocar/onion-zed?color=success)](LICENSE)
![Plataforma](https://img.shields.io/badge/plataforma-Zed-194BFB)
![Metodologia](https://img.shields.io/badge/metodologia-Spec--as--Code_%2B_SDD-blue)
[![Família Onion](https://img.shields.io/badge/família-Onion-8A2BE2)](https://github.com/marciocar/onion)

**[O que é](#-o-que-é) · [Início rápido](#-início-rápido) · [Família Onion](#-família-onion) · [Documentação](#-documentação) · [Contribuir](#-contribuir)**

</div>

---

> [!NOTE]
> Esta é a porta **Zed** da família Onion — a mesma metodologia, expressa no primitivo nativo da plataforma. Conheça as outras 5 portas no hub: **[github.com/marciocar/onion](https://github.com/marciocar/onion)**.

## 🎯 O que é

O Onion é um **framework nativo do Zed** (em `.agents/` + `.zed/`) que se instala em qualquer projeto — novo, legado ou regulado — para orquestrar o ciclo completo de desenvolvimento. Separa **decisão de negócio**, **execução técnica** e **governança/compliance** em contextos distintos, conectados por fluxos e padrões repetíveis. Skills, specialists e documentação passam a conversar entre si em vez de competir por atenção no chat ou em arquivos soltos.

O Onion **não é produto npm**, **não é distribuído publicamente** e **não tem CLI standalone**. Plataforma única: Zed.

## ⚡ Início rápido

1. Abra um projeto que já tenha o Onion instalado (`.agents/` + `.zed/` + `AGENTS.md` na raiz) no **Zed** (confie no worktree quando solicitado).
2. No Agent Panel (`Ctrl+Alt+J`), rode **`/onion`** para orientação, ou **`/onion-warm-up`** para carregar contexto.

**Como invocar:**

| Primitivo | Sintaxe | Exemplo |
|---|---|---|
| Skill (comando) | `/onion-categoria-comando` | `/onion-engineer-start`, `/onion-product-task` |
| Specialist | via `spawn_agent` | `jira-specialist`, `code-reviewer` |

## 🧩 O que muda no dia a dia

- Menos retrabalho por decisões perdidas ou mal comunicadas.
- **Contexto explícito** antes de cada ação: todo mundo sabe em qual dimensão está trabalhando (produto, engenharia ou compliance).
- **Workflows faseados retomáveis** com sessões persistentes (`.agents/onion/sessions/`) que permitem pausar e continuar.
- Documentação e fluxo de trabalho **mais próximos do que o time realmente faz**.
- Práticas de **configuração e segurança** integradas (credenciais fora do repositório, templates seguros).

## 🌐 Família Onion

| Porta | Plataforma | Repositório |
|---|---|---|
| 🌐 onion (hub) | a história de todas | [onion](https://github.com/marciocar/onion) |
| 🟠 claude | Claude Code | [onion-claude](https://github.com/marciocar/onion-claude) |
| 🔵 cursor | Cursor | [onion-cursor](https://github.com/marciocar/onion-cursor) |
| 🟣 antigravity | Google Antigravity | [onion-antigravity](https://github.com/marciocar/onion-antigravity) |
| ⚡ **zed** | **Zed** | **◀ você está aqui** |
| 🟢 codex | OpenAI Codex | [onion-codex](https://github.com/marciocar/onion-codex) |
| 🐙 copilot | GitHub Copilot + VS Code | [onion-copilot](https://github.com/marciocar/onion-copilot) |

## 📚 Documentação

- **[Índice geral de docs](docs/INDEX.md)** · **[Guia Onion](docs/onion/)** — conceitos e referências operacionais.
- **[Guia Onion no Zed](ONION-ZED-GUIA.md)** — execução nativa, atalhos e catálogo de skills.

<details>
<summary><b>Mais: para quem é e como funciona em três passos</b></summary>

<br>

**Para quem é** — squads de produto, engenharia e compliance que precisam alinhar prioridade, implementação, qualidade e conformidade; times que sentem falta de contexto compartilhado; organizações que querem padrão sem burocracia; projetos novos, legados ou regulados (ISO 27001, ISO 22301, SOC2, PMBOK).

**Como funciona em três passos**

1. **Definir a intenção** no contexto certo — produto (descoberta e spec), engenharia (implementação e entrega) ou compliance (governança e conformidade).
2. **Executar com apoio** de skills padronizadas e specialists, em ciclos faseados retomáveis (`product-collect→feature` e `engineer-plan→pr-update`).
3. **Validar e registrar** — qualidade, segurança e conhecimento ficam sincronizados para o próximo ciclo.

</details>

## 🤝 Contribuir

Veja **[CONTRIBUTING.md](CONTRIBUTING.md)**.

## 📄 Licença

Distribuído sob a licença **MIT** — veja **[LICENSE](LICENSE)**.

O Onion se inspira em ideias de **interface unificada** para orquestração com IA; referência conceitual: [Esperanto, de Luis Novo](https://github.com/lfnovo/esperanto).

<div align="center"><sub>🧅 Onion — contexto certo, decisão melhor, entrega contínua.</sub></div>
