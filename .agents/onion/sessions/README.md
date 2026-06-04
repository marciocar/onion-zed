# Sessions — Workflows Faseados

Diretório de estado runtime dos workflows faseados do Sistema Onion.

Cada subpasta corresponde a uma sessão de trabalho ativa (feature, hotfix, etc.)
e contém arquivos de contexto criados pelas skills `onion-engineer-*` e `onion-product-*`.

## Estrutura típica de uma sessão

```
sessions/
└── <feature-slug>/
    ├── context.md       # Contexto geral (task IDs, branch, objetivo)
    ├── architecture.md  # Decisões arquiteturais da feature
    ├── plan.md          # Plano de implementação faseado
    └── notes.md         # Notas e observações durante o desenvolvimento
```

## Skills que operam sessões

| Skill | Ação |
|-------|------|
| `/onion-engineer-start` | Cria a sessão inicial |
| `/onion-engineer-work` | Retoma e atualiza o estado |
| `/onion-engineer-pre-pr` | Valida o estado antes do PR |
| `/onion-engineer-pr-update` | Atualiza sessão pós-review |
| `/onion-docs-sync-sessions` | Sincroniza histórico de sessões |

## Gitignore em projetos-alvo

Quando o Onion é instalado em projetos-alvo, considere adicionar ao `.gitignore`:

```
.agents/onion/sessions/
```

No repositório do Onion (este), sessões de exemplo podem ser preservadas para referência e testes.
