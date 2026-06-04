---
name: onion-docs-build-index
description: Gerenciar e atualizar índices de documentação. Use quando precisar reconstruir o INDEX.md principal ou os index.md de seções específicas (business-context, technical-context, compliance, onion, guidelines), mantendo a estrutura navegável e estatísticas atualizadas.
disable-model-invocation: true
---

Argumento opcional: o nome de uma seção (ex.: `business-context`). Sem argumento, reconstrói o `INDEX.md` principal; com argumento, reconstrói o `index.md` da seção indicada. Os argumentos fornecidos: #$ARGUMENTS

Este comando gerencia os índices de documentação do projeto, mantendo a estrutura organizada e navegável.

**Estrutura de Documentação do projeto**:
```
docs/
├── INDEX.md                    # Índice principal (hub central)
├── business-context/           # Contexto de negócio (15 arquivos)
│   └── index.md
├── technical-context/          # Contexto técnico (15 arquivos)
│   └── index.md
├── compliance/                 # Compliance e Governança (18 arquivos) ✨ NOVO
│   ├── index.md
│   ├── security/               # ISO 27001 (3 arquivos)
│   ├── business-continuity/    # ISO 22301 (5 arquivos)
│   ├── soc2/                   # SOC2 Type II (4 arquivos)
│   ├── ai-governance/          # AI Governance (1 arquivo)
│   ├── privacy/                # LGPD (1 arquivo)
│   └── due-diligence/          # Due Diligence (4 arquivos)
├── onion/                      # Sistema Onion (22 arquivos)
├── guidelines/                 # Guidelines de desenvolvimento (4 arquivos)
└── files/                      # Recursos diversos

.agents/skills/                 # Skills organizadas por categoria
    ├── onion-docs-*            # Skills de documentação
    ├── onion-engineer-*        # Workflows de desenvolvimento
    ├── onion-product-*         # Coleta e gestão de produto
    └── ...
.agents/onion/specialists/      # Agentes especializados de IA
```

## Usage

### /onion-docs-build-index

**Sem argumentos**: Reconstrói o arquivo `INDEX.md` principal na pasta `docs/`.

Este índice central fornece:
- Visão geral do projeto
- Links para todas as seções de documentação
- Descrição de cada seção
- Estatísticas da documentação (80 arquivos, comandos e agentes)
- Guias de navegação por perfil (dev, PM, vendas, arquitetos, CISO/Compliance)
- Mapa de navegação rápida
- Referência completa aos agentes especializados em `.agents/onion/specialists/`
- Métricas de maturidade de compliance (ISO 27001, ISO 22301, SOC2, LGPD)

**Comportamento**:
1. Escaneia todas as pastas em `docs/`
2. Lê os arquivos `index.md` de cada seção
3. Escaneia `.agents/skills/` e `.agents/onion/specialists/` para contar recursos
4. Extrai informações relevantes (título, descrição, arquivos principais)
5. Gera/atualiza `docs/INDEX.md` com estrutura completa
6. Mantém estatísticas atualizadas:
   - arquivos markdown (+README.md landing pages)
   - skills/comandos
   - agentes IA
   - arquivos business-context
   - arquivos technical-context
   - arquivos compliance ✨ NOVO
   - arquivos onion
   - arquivos guidelines
7. Gera métricas de maturidade de compliance:
   - ISO 27001:2022 (84% implementado)
   - ISO 22301:2019 (100% implementado)
   - SOC2 Type II (93% readiness)
   - LGPD (95% compliant)
   - AI Governance (100% documentado)

### /onion-docs-build-index <section-name>

**Com argumento**: Reconstrói o índice de uma seção específica da documentação.

**Seções disponíveis**:
- `business-context` - Documentação de negócio
- `technical-context` - Documentação técnica
- `compliance` - Compliance e Governança (ISO 27001, ISO 22301, SOC2, LGPD) ✨ NOVO
- `onion` - Sistema Onion (comandos e agentes)
- `guidelines` - Guidelines de desenvolvimento

**Comportamento**:
1. Percorre a estrutura de arquivos da seção especificada
2. Identifica arquivos principais e subpastas
3. Gera/atualiza o `index.md` da seção
4. Mantém links relativos corretos
5. Preserva estrutura e organização

**Exemplo**:
```bash
/onion-docs-build-index business-context
# Reconstrói docs/business-context/index.md

/onion-docs-build-index technical-context
# Reconstrói docs/technical-context/index.md

/onion-docs-build-index compliance
# Reconstrói docs/compliance-context/index.md
```

Argumentos fornecidos: #$ARGUMENTS
