---
name: onion-docs-build-compliance-docs
description: Gerar arquitetura de compliance em `docs/compliance-context/`. Use quando precisar criar documentação de conformidade multi-framework (ISO 27001, ISO 22301, SOC2, PMBOK) para auditorias e certificações.
disable-model-invocation: true
---

Argumentos opcionais: `frameworks` (ex.: `iso27001,soc2` ou `all`) e `due_diligence` (caminho para checklist de DD). Detecte o modo a partir de quais argumentos foram fornecidos (ver Passo 1).

# 📋 Gerador de Documentação de Compliance

Criar documentação de conformidade para auditorias e certificações.

## 🎯 Objetivo

Gerar arquitetura completa de docs de compliance multi-framework.

## 🔧 Modos de Execução

```bash
# Modo 1: Seletivo
/onion-docs-build-compliance-docs frameworks="iso27001,soc2"

# Modo 2: Due Diligence
/onion-docs-build-compliance-docs due_diligence="path/to/checklist.md"

# Modo 3: Auto (analisa projeto)
/onion-docs-build-compliance-docs

# Modo 4: Completo
/onion-docs-build-compliance-docs frameworks="all"
```

## ⚡ Fluxo de Execução

### Passo 1: Detectar Modo

SE `{{frameworks}}` → Modo Seletivo
SE `{{due_diligence}}` → Modo DD (analisar checklist)
SENÃO → Modo Auto (analisar projeto)

### Passo 2: Selecionar Frameworks

| Framework | Foco | Quando Usar |
|-----------|------|-------------|
| ISO 27001 | Segurança da Info | Certificação, DD |
| ISO 22301 | Continuidade | DR, BCP |
| SOC2 | Trust Services | Clientes enterprise |
| PMBOK | Governança | Projetos |

### Passo 3: Delegar para Especialistas

Para cada framework selecionado:

```
SE "iso27001" → delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/iso-27001-specialist.md` e atue como esse especialista para: gerar a documentação ISO 27001"
SE "iso22301" → delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/iso-22301-specialist.md` e atue como esse especialista para: gerar a documentação ISO 22301"
SE "soc2" → delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/soc2-specialist.md` e atue como esse especialista para: gerar a documentação SOC2"
SE "pmbok" → delegue via tool `spawn_agent`: "Leia `.agents/onion/specialists/pmbok-specialist.md` e atue como esse especialista para: gerar a documentação PMBOK"
```

Coordenação via tool `spawn_agent`: "Leia `.agents/onion/specialists/security-information-master.md` e atue como esse especialista para: coordenar a geração multi-framework de compliance"

### Passo 4: Gerar Documentação

Estrutura de saída:
```
docs/compliance-context/
├── index.md
├── iso27001/
│   ├── policy.md
│   ├── risk-assessment.md
│   └── controls.md
├── soc2/
│   ├── trust-services.md
│   └── evidence.md
└── reports/
    └── summary.md
```

### Passo 5: Validar e Entregar

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ DOCS DE COMPLIANCE GERADOS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 Frameworks:
∟ ISO 27001: ✅ 12 documentos
∟ SOC2: ✅ 8 documentos

📁 Estrutura:
∟ docs/compliance-context/index.md
∟ docs/compliance-context/iso27001/ (12)
∟ docs/compliance-context/soc2/ (8)

📋 Cobertura:
∟ Políticas: 100%
∟ Controles: 85%
∟ Evidências: Template

🚀 Próximo: Revisar e customizar
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Orquestrador: `.agents/onion/specialists/security-information-master.md`
- ISO 27001: `.agents/onion/specialists/iso-27001-specialist.md`
- SOC2: `.agents/onion/specialists/soc2-specialist.md`

## ⚠️ Notas

- Docs gerados são templates base
- Customizar para contexto específico
- Revisar antes de auditorias
