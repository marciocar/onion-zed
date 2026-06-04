# Guias de Aplicação do Sistema Onion

> Como instalar e aplicar o Sistema Onion em projetos novos, legados ou regulados.

---

## Visão geral

O Sistema Onion é um **framework nativo do Zed** (em `.agents/` + `.zed/`) que se materializa em cada projeto-alvo. Ver [ADR 0001](../meta-specs/adr/0001-zed-native-port.md). Estes guias cobrem os três cenários principais de aplicação:

| Cenário | Guia | Quando usar |
|---|---|---|
| Greenfield | [applying-greenfield.md](./applying-greenfield.md) | Projeto novo, sem código ou documentação prévia |
| Legado | [applying-legacy.md](./applying-legacy.md) | Projeto existente, requer engenharia reversa antes da aplicação |
| Regulado | [applying-regulated.md](./applying-regulated.md) | Projeto sujeito a frameworks de compliance (ISO 27001, ISO 22301, SOC2, PMBOK) |

Cada guia documenta:

- Pré-requisitos
- Passo a passo desde clone/init até primeira skill útil
- Decisão sobre quais dos três contextos spec-as-code ativar (business, technical, compliance)
- Skills específicas do cenário
- Troubleshooting

---

## Pré-requisitos comuns a todos os cenários

1. **Zed instalado** — plataforma única do Onion (ver instalação em [applying-greenfield.md](./applying-greenfield.md))
2. **Git** instalado e funcional
3. **Acesso ao repositório do Onion** (este repositório) para copiar `.agents/`, `.zed/settings.json`, `AGENTS.md` e estrutura `docs/`
4. **Worktree trust** habilitado no Zed para o projeto-alvo (necessário para descobrir skills locais em `.agents/skills/` e os `context_servers`)
5. **Conta em pelo menos um Task Manager** (Jira, ClickUp, Asana ou Linear) se o projeto usar tasks

---

## Decisão inicial — Qual cenário se aplica?

Use esta árvore de decisão:

```
O projeto-alvo já tem código?
├── NÃO → applying-greenfield.md
└── SIM → O projeto-alvo está sujeito a frameworks regulatórios?
    ├── SIM → applying-regulated.md
    └── NÃO → applying-legacy.md
```

Projetos podem combinar cenários (ex: greenfield em setor regulado segue greenfield com camada compliance ativada).

---

**Próximo passo**: abrir o guia correspondente ao seu cenário. A instalação base (`.agents/` + `.zed/settings.json` + `AGENTS.md` + worktree trust) é detalhada em [applying-greenfield.md](./applying-greenfield.md).
