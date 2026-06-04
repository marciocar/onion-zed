# `.agents/` — Núcleo nativo Zed do Sistema Onion

Estrutura nativa do Zed que substitui `.claude/` ([ADR 0001](../docs/meta-specs/adr/0001-zed-native-port.md)).

```
.agents/
├── skills/                  # Skills Zed (flat, 1 pasta por skill) — invocáveis por / e @
│   ├── onion/               # orquestrador (cérebro do sistema)
│   ├── onion-warmup/
│   ├── onion-patterns/
│   ├── onion-validation/
│   ├── language-standards/
│   └── onion-<categoria>-<comando>/   # ex-comandos (.claude/commands)
└── onion/
    ├── specialists/         # personas delegáveis via spawn_agent (ex-agentes)
    ├── utils/               # abstrações (task-manager, etc.) — lógica agnóstica
    ├── templates/           # ex-common/templates
    └── prompts/             # ex-common/prompts
```

## Regras (ver ADR 0001)

- **Skills**: catálogo **flat** — sem subpastas. Naming `onion-<categoria>-<comando>` (lowercase+hífen, ≤64 chars).
- **Frontmatter de SKILL.md**: apenas `name`, `description`, `disable-model-invocation`.
- **Delegação**: `spawn_agent` com prompt *"Leia `.agents/onion/specialists/<x>.md` e atue como esse especialista para: …"*.
- **Permissões de tools**: globais em `.zed/settings.json` (`agent.tool_permissions`), não por skill.
- **Rules do projeto**: `AGENTS.md` na raiz (lido nativamente pelo Zed).
