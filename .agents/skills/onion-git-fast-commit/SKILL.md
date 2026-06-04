---
name: onion-git-fast-commit
description: Adiciona todas as mudanças e faz commit rápido. Use quando precisar fazer commits típicos no fluxo do Sistema Onion, adicionando todas as alterações e commitando com uma mensagem em Conventional Commits.
disable-model-invocation: true
---

# Fast Commit

Adiciona todas as mudanças e faz commit com mensagem especificada.

> **Parâmetro**: a mensagem de commit é passada como argumento entre aspas. Faça o parsing do texto invocado para extraí-la. Use apenas operações `git` via tool `terminal`.

## 🎯 Uso

```bash
/onion-git-fast-commit "feat: implement admin dashboard basic flow"
```

## ⚡ Fluxo de Execução

1. `git add .` — adiciona todas as mudanças
2. `git commit -m "<mensagem>"` — commit com a mensagem

## 📋 Convenção de Mensagens

Use [Conventional Commits](https://conventionalcommits.org):

| Tipo | Descrição |
|------|-----------|
| `feat:` | Nova funcionalidade |
| `fix:` | Correção de bug |
| `docs:` | Documentação |
| `refactor:` | Refatoração |
| `chore:` | Manutenção |

## ⚠️ Notas

- SEMPRE revise `git status` antes
- Prefira commits atômicos e descritivos
