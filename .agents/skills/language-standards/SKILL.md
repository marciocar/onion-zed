---
name: language-standards
description: >
  Aplica padrões de idioma e documentação do projeto. Use ao escrever código,
  commits, comentários, READMEs, documentação técnica, mensagens de erro ou
  qualquer artefato textual. Garante código em inglês (variáveis, funções,
  classes, arquivos, branches) e comentários/docs em português brasileiro
  (comments, JSDoc, READMEs, mensagens ao usuário, respostas do assistente).
  Ative mesmo sem o usuário mencionar "idioma" ou "padrão".
---

## Regras Fundamentais

### Inglês (en-US) — SEMPRE
- Nomes de **variáveis, funções, classes, interfaces, types**
- Nomes de **arquivos e diretórios** (kebab-case: `user-profile.tsx`)
- Nomes de **skills e specialists** (`onion-engineer-start`, `jira-specialist`)
- **Nomes de branches** Git (`feature/user-dashboard`, `fix/auth-bug`)
- **Documentação técnica de API** (schemas, endpoints, response shapes)

### Português brasileiro (pt-BR) — SEMPRE
- **Comentários no código** (inline e JSDoc)
- **Respostas e explicações** do assistente IA
- **Documentação de processos e workflows**, READMEs e guias de uso
- **Mensagens de erro** para usuário final
- **Commits** (mensagens de commit em pt-BR — ver Gotcha abaixo)

## Quick Reference

| Contexto | Idioma | Exemplo |
|----------|--------|---------|
| Variáveis, funções, classes | EN | `getUserProfile()` |
| Comentários no código | PT-BR | `// Busca perfil do usuário` |
| Commits | PT-BR | `fix: corrige bug de autenticação` |
| Branches | EN | `feature/payment-flow` |
| Documentação técnica | PT-BR | `## Instalação` |
| Respostas do assistente | PT-BR | `Vou criar o componente...` |
| Nomes de arquivos | EN | `user-profile.tsx` |
| Mensagens de erro UI | PT-BR | `throw new Error('Usuário não encontrado')` |

## Exemplo correto

```typescript
/**
 * Componente de perfil de usuário com informações básicas
 */
export const UserProfileCard: React.FC<UserProfileCardProps> = ({ userId }) => {
  // Busca os dados do usuário usando o hook do ZenStack
  const { data: user, isLoading } = useFindUniqueUser({ where: { id: userId } });

  if (isLoading) return <ProfileSkeleton />;
  return <Card>{/* ... */}</Card>;
};
```

## Workflow obrigatório

### Antes de finalizar uma tarefa
1. **Consultar documentação existente** em `docs/`, `.agents/skills/`, `.agents/onion/specialists/`
2. **Validar conformidade de idioma** (código EN, comentários/docs PT-BR)
3. **Propor próximo passo lógico** com 1-2 opções recomendadas
4. **Sugerir skill de continuação** (`/onion-...`) quando aplicável

### Sintaxes oficiais — NUNCA inventar
- Consultar documentação oficial da versão em uso
- Delegar a specialists via `spawn_agent`: `specialists/nodejs-specialist.md`, `specialists/react-developer.md`, `specialists/zen-engine-specialist.md`
- Documentar desvios necessários com justificativa

## Gestão de Configurações (.env)

- **NUNCA** commitar `.env` com valores sensíveis (já no `.gitignore`)
- **SEMPRE** manter `.env.example` atualizado com placeholders
- Prefixos por integração: `CLICKUP_`, `JIRA_`, `GITHUB_`, `GAMMA_`, `POSTGRES_`
- Skills devem **funcionar sem integrações** quando possível
- Se integração não configurada: avisar e sugerir `/onion-meta-setup-integration`

## Exceções

- **Inglês em comentários**: referências diretas a código (`// O método getUserById()...`), termos técnicos sem tradução, links para docs EN.
- **Português em strings de código**: APENAS em strings de UI para usuário final (labels, mensagens visíveis). NUNCA em identificadores.

## Gotchas

- **Commits em idioma errado** — padrão do projeto é pt-BR; rever antes do `git commit`.
- **Nomes de arquivos em PT-BR** quebram convenções de framework (ex: Next.js routing) — sempre EN.
- **Mensagens técnicas vs UX** — `Error: invalid user ID` (técnico, EN) vs `Usuário não encontrado` (UX, PT-BR).

## Referências

- Rules do projeto: `AGENTS.md`
- KB: `docs/knowledge-base/concepts/configuration-management.md`
- Skills relacionadas: `onion-patterns`, `onion-validation`
