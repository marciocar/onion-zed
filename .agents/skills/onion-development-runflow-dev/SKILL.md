---
name: onion-development-runflow-dev
description: Desenvolvimento com Runflow SDK. Use quando precisar criar projetos, agentes, sistemas multi-agente, RAG, workflows ou integrações com o Runflow SDK, orientando próximos passos e fechamento de tarefas via o especialista Runflow.
disable-model-invocation: true
---

# Desenvolvimento Runflow

Skill especializada para desenvolvimento completo com Runflow SDK usando o especialista Runflow. Facilita criação de projetos, agentes, workflows, RAG e integrações, sempre orientando próximos passos e fechamento de tarefas.

## Processo

### 1. Invocar Agente Especialista

**SEMPRE** invoque o especialista Runflow via tool `spawn_agent`:

```
spawn_agent → "Leia .agents/onion/specialists/runflow-specialist.md e atue como
esse especialista para: [sua solicitação detalhada].
Contexto do projeto: [descreva o projeto/agente/workflow envolvido].
Retorne código completo, validação e próximos passos."
```

O especialista possui conhecimento completo da base de conhecimento em `docs/knowledge-base/platforms/runflow.md` e padrões do projeto.

### 2. Operações Disponíveis

#### 2.1. Criar Novo Projeto Runflow

**Quando usar**: Iniciar um novo projeto do zero com Runflow SDK

**Delegação via `spawn_agent`**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
criar novo projeto Runflow com nome [nome-do-projeto]. Deve incluir: estrutura de
diretórios, package.json, tsconfig.json, arquivo main.ts com agente básico,
configuração .runflow/rf.json, e README.md"
```

**O que o especialista fará**:
1. Criar estrutura de diretórios completa
2. Configurar `package.json` com dependências Runflow
3. Configurar `tsconfig.json` para TypeScript
4. Criar `main.ts` com agente básico seguindo padrões
5. Criar `.runflow/rf.json` com template de configuração
6. Gerar `README.md` com instruções
7. Validar código e sugerir próximos passos

**Exemplo de prompt para spawn_agent**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
criar novo projeto Runflow chamado 'assistente-juridico' para ajudar com processos
jurídicos. Retorne estrutura completa e instruções de setup."
```

#### 2.2. Criar Novo Agente Runflow

**Quando usar**: Adicionar novo agente a projeto existente

**Delegação via `spawn_agent`**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
criar novo agente Runflow chamado [nome] com as seguintes características:
[descrição detalhada das funcionalidades, tools necessárias, se precisa RAG, memory, etc.]"
```

**O que o especialista fará**:
1. Analisar requisitos e consultar base de conhecimento
2. Verificar padrões existentes no projeto (`main.ts`)
3. Criar agente seguindo estrutura do projeto
4. Implementar tools customizadas se necessário
5. Configurar memory e RAG conforme especificado
6. Validar código e criar testes básicos
7. Documentar uso e próximos passos

**Exemplo de prompt para spawn_agent**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
criar agente chamado 'ProcessAnalyzer' que analisa processos jurídicos. Precisa de:
tool para buscar processos por número, RAG com base 'processos', memory para lembrar
análises anteriores, e responder em português brasileiro. Retorne código completo."
```

#### 2.3. Conectar Agentes (Multi-Agent System)

**Quando usar**: Criar sistema com múltiplos agentes especializados e supervisor

**Delegação via `spawn_agent`**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
criar sistema multi-agente com supervisor que roteia para: [lista de agentes].
Supervisor deve: [critérios de roteamento]. Cada agente especializado: [descrição]"
```

**O que o especialista fará**:
1. Criar agentes especializados individuais
2. Criar agente supervisor com lógica de roteamento
3. Implementar função de roteamento inteligente
4. Configurar comunicação entre agentes
5. Criar exemplo de uso completo
6. Documentar arquitetura e fluxo

**Exemplo de prompt para spawn_agent**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
criar sistema multi-agente com supervisor que roteia para: SalesAgent (vendas),
SupportAgent (suporte técnico), BillingAgent (cobrança). Supervisor analisa intenção e
roteia automaticamente. Cada agente tem suas próprias tools e RAG específicos."
```

#### 2.4. Criar Projeto RAG (Base de Conhecimento)

**Quando usar**: Configurar RAG para busca semântica em base de conhecimento

**Delegação via `spawn_agent`**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
configurar RAG para base '[nome-da-base]' com: [tipo de conteúdo, threshold, k resultados,
quando usar busca]. Integrar com agente [nome-do-agente]."
```

**O que o especialista fará**:
1. Verificar se base de conhecimento existe na plataforma Runflow
2. Configurar RAG no agente com parâmetros otimizados
3. Criar searchPrompt apropriado para o contexto
4. Implementar exemplo de uso
5. Documentar configuração e ajustes recomendados

**Exemplo de prompt para spawn_agent**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
configurar RAG para base 'processos-juridicos' no agente ProcessAnalyzer. Threshold 0.2,
k=3 resultados. Buscar quando usuário perguntar sobre processos, previdência, intimações.
Criar searchPrompt apropriado."
```

#### 2.5. Criar Workflow Completo

**Quando usar**: Orquestrar múltiplos passos com agentes e conectores

**Delegação via `spawn_agent`**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
criar workflow '[nome]' que: [descrição passo a passo]. Incluir:
[agentes envolvidos, conectores necessários, passos condicionais se houver]."
```

**O que o especialista fará**:
1. Definir schema de entrada e saída com Zod
2. Criar agentes necessários para cada passo
3. Configurar conectores (HubSpot, Twilio, etc.)
4. Implementar workflow com passos sequenciais/paralelos
5. Adicionar passos condicionais se necessário
6. Criar exemplo de execução e documentar fluxo

**Exemplo de prompt para spawn_agent**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
criar workflow 'lead-qualification' que: 1) qualifica lead com agente, 2) se nota >= 7
cria contato no HubSpot, 3) cria deal, 4) notifica equipe no Slack. Se nota < 7, apenas
registra. Retorne código completo e exemplo de execução."
```

#### 2.6. Orientar Próximos Passos

**Quando usar**: Após qualquer operação, pedir orientação sobre o que fazer a seguir

**Delegação via `spawn_agent`**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
orientar os próximos passos após [o que foi feito]. Sugerir: comandos para testar,
melhorias possíveis, integrações recomendadas, e próximas tarefas."
```

**O que o especialista fará**:
1. Analisar o que foi criado/modificado
2. Sugerir comandos de teste apropriados
3. Identificar melhorias e otimizações
4. Recomendar integrações úteis
5. Fornecer comandos específicos para próximas ações

**Exemplo de prompt para spawn_agent**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
orientar os próximos passos após criar o agente ProcessAnalyzer. Sugerir comandos
para testar, melhorias e próximas tarefas."
```

#### 2.7. Fechar Tarefa / Finalizar Desenvolvimento

**Quando usar**: Quando uma feature está completa e precisa ser finalizada

**Delegação via `spawn_agent`**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
finalizar o desenvolvimento de [feature/tarefa]. Verificar: código completo, testes,
documentação, validação, e criar resumo do que foi implementado."
```

**O que o especialista fará**:
1. Revisar código completo criado
2. Validar com linter
3. Verificar se testes estão implementados
4. Confirmar documentação atualizada
5. Criar resumo executivo do que foi feito
6. Sugerir comandos para deploy/teste final

**Exemplo de prompt para spawn_agent**:
```
"Leia .agents/onion/specialists/runflow-specialist.md e atue como esse especialista para:
finalizar o desenvolvimento do sistema multi-agente de suporte. Verificar tudo e
criar resumo completo."
```

### 3. Fluxo de Trabalho Recomendado

#### Para Novo Projeto Completo:
1. **Criar projeto**: `spawn_agent` → `"...runflow-specialist.md...criar novo projeto Runflow..."`
2. **Criar agente inicial**: `spawn_agent` → `"...runflow-specialist.md...criar agente..."`
3. **Configurar RAG**: `spawn_agent` → `"...runflow-specialist.md...configurar RAG..."`
4. **Testar**: `spawn_agent` → `"...runflow-specialist.md...orientar próximos passos..."`
5. **Finalizar**: `spawn_agent` → `"...runflow-specialist.md...finalizar desenvolvimento..."`

#### Para Adicionar Feature:
1. **Criar agente/feature**: `spawn_agent` → `"...runflow-specialist.md...criar [tipo]..."`
2. **Integrar**: `spawn_agent` → `"...runflow-specialist.md...conectar [agentes/workflows]..."`
3. **Orientar**: `spawn_agent` → `"...runflow-specialist.md...orientar próximos passos..."`
4. **Finalizar**: `spawn_agent` → `"...runflow-specialist.md...finalizar desenvolvimento..."`

## Guidelines

### ✅ Boas Práticas

**Sempre**:
- ✅ Delegue ao especialista Runflow via `spawn_agent` para todas as operações
- ✅ Seja específico e detalhado nos prompts de delegação
- ✅ Mencione requisitos técnicos (RAG, memory, tools, etc.)
- ✅ Peça orientação de próximos passos após criar algo
- ✅ Finalize tarefas explicitamente para documentação

**Estrutura de Prompts para spawn_agent**:
- ✅ Inclua nome do projeto/agente/feature
- ✅ Descreva funcionalidades desejadas
- ✅ Especifique tools necessárias
- ✅ Mencione se precisa RAG, memory, workflows
- ✅ Indique integrações (HubSpot, Twilio, etc.)

**Desenvolvimento**:
- ✅ Teste incrementalmente após cada criação
- ✅ Valide código antes de continuar
- ✅ Documente decisões importantes
- ✅ Siga padrões do projeto (`main.ts`)

### ⚠️ Atenções Especiais

**Configuração**:
- ⚠️ Verifique `.runflow/rf.json` ou variáveis de ambiente antes de executar
- ⚠️ Confirme versão do SDK (1.0.56) no `package.json`
- ⚠️ Use `observability: 'minimal'` para evitar erros no trace collector

**RAG**:
- ⚠️ Base de conhecimento deve existir na plataforma Runflow antes de configurar
- ⚠️ Ajuste threshold e k baseado no tipo de conteúdo
- ⚠️ Crie searchPrompt específico para o contexto

**Multi-Agent**:
- ⚠️ Defina claramente critérios de roteamento no supervisor
- ⚠️ Cada agente especializado deve ter tools e RAG apropriados
- ⚠️ Teste roteamento com diferentes tipos de input

**Workflows**:
- ⚠️ Defina schemas de entrada/saída claros com Zod
- ⚠️ Trate erros em cada passo do workflow
- ⚠️ Teste passos condicionais com diferentes cenários

### ❌ O Que Evitar

**Prompts de delegação**:
- ❌ Não seja vago: "criar agente" → "criar agente X que faz Y com tools Z"
- ❌ Não pule etapas: configure RAG antes de usar no agente
- ❌ Não ignore validação: sempre teste após criar código

**Código**:
- ❌ Não acesse Prisma diretamente (use Runflow SDK)
- ❌ Não use `observability: 'full'` (use 'minimal')
- ❌ Não ignore tratamento de erros em tools

**Integração**:
- ❌ Não configure conectores sem credenciais válidas
- ❌ Não use RAG sem base de conhecimento criada
- ❌ Não conecte agentes sem definir roteamento claro

## Exemplos

### Exemplo 1: Criar Projeto Completo do Zero

**Input**:
```
/onion-development-runflow-dev Criar projeto completo "assistente-juridico" para análise
de processos. Incluir: agente principal, RAG com base "processos-juridicos", tool para
buscar processos, memory para contexto
```

**Processo**:
1. Delega via `spawn_agent` para criar projeto
2. Especialista cria estrutura completa
3. Nova delegação via `spawn_agent` para criar agente principal
4. Configura RAG
5. Cria tool de busca
6. Orienta próximos passos
7. Finaliza com resumo

---

### Exemplo 2: Adicionar Sistema Multi-Agente

**Input**:
```
/onion-development-runflow-dev Criar sistema multi-agente com supervisor que roteia para
Sales, Support e Billing. Cada um com suas próprias tools e RAG
```

**Processo**:
1. Delega via `spawn_agent` para criar agentes especializados
2. Cria agente supervisor
3. Implementa roteamento
4. Configura RAG específico para cada agente
5. Cria exemplo de uso e orienta testes

---

### Exemplo 3: Configurar RAG e Orientar Próximos Passos

**Input**:
```
/onion-development-runflow-dev Configurar RAG no agente ProcessAnalyzer com base
"processos". Depois orientar próximos passos
```

**Processo**:
1. Delega via `spawn_agent` para configurar RAG
2. Especialista ajusta parâmetros (threshold, k, searchPrompt)
3. Nova delegação via `spawn_agent` para orientar próximos passos
4. Sugere comandos de teste e melhorias

---

### Exemplo 4: Finalizar Feature Completa

**Input**:
```
/onion-development-runflow-dev Finalizar desenvolvimento do sistema de suporte completo.
Verificar tudo e criar resumo
```

**Processo**:
1. Delega via `spawn_agent` para revisar e validar código
2. Especialista verifica testes e documentação
3. Cria resumo executivo e sugere deploy/teste final

## Checklist de Validação

### Antes de Criar
- [ ] Definiu claramente o que quer criar
- [ ] Especificou requisitos técnicos (RAG, memory, tools)
- [ ] Mencionou integrações necessárias
- [ ] Verificou se projeto Runflow existe (para novos agentes)

### Durante Criação
- [ ] O especialista Runflow foi invocado via `spawn_agent`
- [ ] Prompt de delegação foi específico e detalhado
- [ ] Código gerado segue padrões do projeto
- [ ] Validação foi executada

### Após Criação
- [ ] Código compila sem erros
- [ ] Linter não mostra problemas críticos
- [ ] Configuração `.runflow/rf.json` está correta
- [ ] Próximos passos foram orientados
- [ ] Documentação foi atualizada

### Para Finalizar
- [ ] Todo código foi revisado
- [ ] Testes foram implementados/verificados
- [ ] Documentação está completa
- [ ] Resumo executivo foi criado
- [ ] Comandos de teste/deploy foram fornecidos

## Comandos Relacionados

- `/onion-meta-create-agent` — criar agente especializado
- `/onion-meta-create-knowledge-base` — criar base de conhecimento

## Troubleshooting

### Agente não encontra base de conhecimento
Verifique se base existe na plataforma Runflow antes de configurar RAG.

### Erro no trace collector
Use `observability: 'minimal'` em todos os agentes.

### Agente não segue padrões do projeto
Mencione explicitamente "seguir padrões de main.ts" no prompt de delegação.

### Roteamento multi-agente não funciona
Verifique critérios de roteamento no supervisor e teste com diferentes inputs.

### Workflow falha em algum passo
Revise schemas de entrada/saída e tratamento de erros em cada passo.

## FAQ

**P: Posso criar múltiplos agentes de uma vez?**
R: Sim! Use um prompt de delegação detalhado listando todos os agentes e suas características.

**P: Como testar agentes criados?**
R: Use `rf test` após criar, ou delegue via `spawn_agent` pedindo orientação de próximos passos.

**P: Posso modificar código gerado pelo especialista?**
R: Sim! O especialista cria código seguindo padrões, mas você pode ajustar conforme necessário.

**P: Como conectar agentes existentes?**
R: Delegue via `spawn_agent`: `"...runflow-specialist.md...conectar agente [A] com agente [B] através de [método: supervisor/workflow]"`.

**P: RAG precisa ser configurado antes ou depois do agente?**
R: Pode ser configurado junto na criação do agente, ou adicionado depois modificando o código.

---

## Resumo de Uso

**Sintaxe básica**:
```
/onion-development-runflow-dev [descreva o que você quer criar/fazer com o Runflow SDK]
```

A skill delega todas as operações ao `runflow-specialist` via `spawn_agent`.

**Operações principais**:
1. Criar novo projeto Runflow
2. Criar novo agente
3. Conectar agentes (multi-agent)
4. Configurar RAG
5. Criar workflows
6. Orientar próximos passos
7. Fechar/finalizar desenvolvimento
