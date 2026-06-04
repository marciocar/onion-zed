# 🧪 Sistema Integrado de Testes e Validação

> **Última atualização**: 2026-06-03 | **Plataforma**: Zed (ver [ADR 0001](../meta-specs/adr/0001-zed-native-port.md))

Documentação completa do conjunto integrado de skills, specialists e knowledge bases para testes e validação no Sistema Onion (nativo Zed).

---

## 📊 Visão Geral do Sistema

O Sistema de Testes e Validação é composto por **4 camadas integradas** que trabalham em conjunto:

```
┌─────────────────────────────────────────────────────────────┐
│  📚 KNOWLEDGE BASE (Framework de Testes)                    │
│  └─ framework_testes.md - Fonte de verdade                 │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│  🤖 SPECIALISTS (3 personas, via spawn_agent)              │
│  ├─ test-agent - Estratégias completas                     │
│  ├─ test-engineer - Implementação prática                  │
│  └─ test-planner - Planejamento e cobertura                │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│  🔧 SKILLS DE TESTE (3 skills)                              │
│  ├─ /onion-test-unit - Testes unitários                    │
│  ├─ /onion-test-integration - Testes de integração         │
│  └─ /onion-test-e2e - Testes end-to-end                    │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│  ✅ SKILLS DE VALIDAÇÃO (6 skills)                           │
│  ├─ /onion-validate-workflow - Validação de workflows      │
│  ├─ /onion-validate-test-strategy-create - Criar estratégias│
│  ├─ /onion-validate-test-strategy-analyze - Analisar       │
│  ├─ /onion-validate-qa-points-estimate - Estimar QA points │
│  ├─ /onion-validate-collab-three-amigos - Sessões colab.   │
│  └─ /onion-validate-collab-pair-testing - Teste em par     │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 Camada 1: Knowledge Base - Framework de Testes

### 📄 `docs/knowledge-base/frameworks/framework_testes.md`

**Propósito**: Fonte de verdade única para todas as práticas de teste.

**Conteúdo Principal**:

#### 1. **Modelo V de Testes**
```
DESENVOLVIMENTO          ←→          TESTE                    QA POINTS
├── Requisitos           ←→          Acceptance Testing       8-13 pts
├── Análise/Design       ←→          System Testing          5-8 pts
├── Arquitetura          ←→          Integration Testing     3-5 pts
└── Implementação        ←→          Unit Testing           1-3 pts
```

#### 2. **Três Perspectivas de Teste**

**White-box (Developer)**:
- Foco: Código interno, cobertura, caminhos de execução
- Ferramentas: Jest, PyTest, JUnit, Coverage.py
- Métricas: Coverage >80%, Mutation Score >70%

**Black-box (QA)**:
- Foco: Requisitos, casos de uso, jornada do usuário
- Ferramentas: Cypress, Selenium, Manual testing
- Métricas: QA Velocity, Estimation Accuracy >80%

**Grey-box (Cross-Dev)**:
- Foco: Integração, contratos de API, tratamento de erros
- Ferramentas: Postman, API testing, Integration suites
- Métricas: API Contract Coverage 100%, Integration Pass Rate >95%

#### 3. **Sistema de QA Story Points**

**Fórmula**:
```
QA Points = Complexidade Base + Risco + Tipo de Teste

Escala:
1 ponto  = 1-2 horas   (micro-teste)
2 pontos = 2-4 horas   (formulário simples)
3 pontos = 4-6 horas   (workflow básico)
5 pontos = 6-10 horas  (feature completa)
8 pontos = 10-16 horas (sistema crítico)
13 pontos = 16-24 horas (épico de teste)
```

#### 4. **Técnicas por Perspectiva**

- **White-box**: Code Coverage, Mutation Testing, TDD, BDD
- **Black-box**: Partição de Equivalência, Valor Limite, Tabela de Decisão, Exploratório
- **Grey-box**: Contract Testing, Fuzzing, Load/Stress Testing

**Como é usado**:
- Todas as skills e specialists consultam este framework antes de executar
- Garante consistência em todas as estimativas e estratégias
- Base para templates e checklists

---

## 🤖 Camada 2: Specialists

### 1. **test-agent** (via `spawn_agent`) - Estrategista Completo

**Responsabilidades**:
- ✅ Domínio total do Framework de Testes
- ✅ Criação de estratégias multi-perspectiva
- ✅ Desenvolvimento de pipelines automatizados
- ✅ Implementação de boas práticas
- ✅ Resolução de problemas de qualidade

**Quando usar**:
- Criar estratégias completas de teste
- Otimizar pipelines de CI/CD
- Resolver problemas complexos de qualidade
- Implementar métricas avançadas

**Exemplo de uso**:
```bash
spawn_agent → test-agent: "Criar estratégia completa de testes para feature de checkout"
```

### 2. **test-engineer** (via `spawn_agent`) - Implementador Prático

**Responsabilidades**:
- ✅ Escrever testes unitários práticos
- ✅ Verificar comportamento real do código
- ✅ Testar código como está (não modificar para testes)
- ✅ Sinalizar lacunas sem corrigir

**Quando usar**:
- Implementar testes unitários específicos
- Verificar qualidade de código existente
- Encontrar problemas reais no código

**Exemplo de uso**:
```bash
spawn_agent → test-engineer: "Criar testes unitários para UserService.validateEmail()"
```

### 3. **test-planner** (via `spawn_agent`) - Planejador e Analista

**Responsabilidades**:
- ✅ Análise sistemática da base de código
- ✅ Identificação de testes ausentes
- ✅ Recomendação de estratégia de testing
- ✅ Geração de relatórios de cobertura

**Quando usar**:
- Analisar cobertura de testes do projeto
- Planejar estratégia de testes para nova feature
- Identificar gaps de teste

**Exemplo de uso**:
```bash
spawn_agent → test-planner: "Analisar cobertura de testes e recomendar melhorias"
```

---

## 🔧 Camada 3: Skills de Teste

### 1. **`/onion-test-unit`** - Testes Unitários (White-box)

**Funcionalidades**:
- Auto-detecção de framework (Jest, Vitest, PyTest, JUnit)
- Análise de código para identificar funções testáveis
- Geração automática de arquivos de teste
- Execução com coverage e watch mode

**Parâmetros**:
```bash
/onion-test-unit <file-path> [--generate] [--run] [--coverage] [--watch] [--framework]
```

**Exemplo**:
```bash
/onion-test-unit src/services/UserService.ts --generate --run --coverage
```

**Integração**:
- Usa `framework_testes.md` para padrões White-box
- Integra com `test-engineer` para validação
- Gera testes seguindo padrão AAA (Arrange, Act, Assert)

### 2. **`/onion-test-integration`** - Testes de Integração (Grey-box)

**Funcionalidades**:
- Auto-detecção de framework (Supertest, Pact, Postman)
- Geração de testes de contrato de API
- Suporte a boundary testing (timeouts, erros)
- Suporte a fuzzing (dados malformados)

**Parâmetros**:
```bash
/onion-test-integration <api-endpoint> [--generate] [--run] [--contract] [--boundary] [--fuzz] [--framework]
```

**Exemplo**:
```bash
/onion-test-integration /api/users --generate --run --contract --boundary
```

**Integração**:
- Usa `framework_testes.md` para técnicas Grey-box
- Foca em contratos de API e integrações
- Valida tratamento de erros e limites

### 3. **`/onion-test-e2e`** - Testes End-to-End (Black-box)

**Funcionalidades**:
- Auto-detecção de framework (Cypress, Playwright, Selenium)
- Geração de cenários baseados em features
- Selectors inteligentes (data-attributes, semantic)
- Gravação de vídeo/screenshots

**Parâmetros**:
```bash
/onion-test-e2e <feature-name> [--generate] [--run] [--headless] [--record] [--framework]
```

**Exemplo**:
```bash
/onion-test-e2e login --generate --run --record
```

**Integração**:
- Usa `framework_testes.md` para técnicas Black-box
- Foca em jornada do usuário
- Valida experiência completa

---

## ✅ Camada 4: Skills de Validação

### 1. **`/onion-validate-workflow`** - Validação de Workflows

**Funcionalidades**:
- Validação de sincronização Git (local vs remoto)
- Validação de working directory
- Validação de gestão de sessões
- Validação de limpeza de branches
- Validação de compliance e segurança
- Validação de integração ClickUp

**Parâmetros**:
```bash
/onion-validate-workflow [pr-merge|cleanup|development|complete]
```

**Exemplo**:
```bash
/onion-validate-workflow pr-merge
```

**Output**:
- Score de qualidade (0-100%)
- Estatísticas detalhadas
- Ações recomendadas/obrigatórias

### 2. **`/onion-validate-test-strategy-create`** - Criar Estratégia de Teste

**Funcionalidades**:
- Cria estratégias completas baseadas no Framework
- Cálculo automático de QA Story Points
- Estratégias multi-perspectiva (White/Grey/Black-box)
- Integração com task managers (ClickUp/Asana)
- Geração de épicos e subtasks estruturadas

**Parâmetros**:
```bash
/onion-validate-test-strategy-create <feature-name> [--risk-level] [--complexity] [--task-manager] [--project-id] [--dry-run]
```

**Exemplo**:
```bash
/onion-validate-test-strategy-create checkout --risk-level alto --complexity complexo
```

**Integração**:
- **OBRIGATORIAMENTE** lê `framework_testes.md` antes de executar
- Usa `test-agent` para validação de estratégia
- Cria tasks no task manager com QA points calculados

### 3. **`/onion-validate-test-strategy-analyze`** - Analisar Estratégia

**Funcionalidades**:
- Análise de estratégias existentes
- Identificação de gaps
- Recomendações de melhoria
- Validação de conformidade com framework

**Parâmetros**:
```bash
/onion-validate-test-strategy-analyze <feature-name> [--output-format]
```

### 4. **`/onion-validate-qa-points-estimate`** - Estimar QA Story Points

**Funcionalidades**:
- Cálculo preciso usando fórmula do Framework
- Análise contextual inteligente
- Breakdown por perspectiva (White/Grey/Black-box)
- Sugestões de técnicas apropriadas
- Integração com task managers

**Parâmetros**:
```bash
/onion-validate-qa-points-estimate "<task-description>" [--complexity] [--risk] [--type] [--task-id] [--update] [--breakdown] [--suggest-techniques]
```

**Exemplo**:
```bash
/onion-validate-qa-points-estimate "Testar fluxo completo de checkout com múltiplos métodos de pagamento" --breakdown --suggest-techniques
```

**Integração**:
- **OBRIGATORIAMENTE** lê `framework_testes.md` para fórmula exata
- Usa análise contextual para auto-detectar complexidade/risco
- Pode atualizar task manager automaticamente

### 5. **`/onion-validate-collab-three-amigos`** - Sessão Three Amigos

**Funcionalidades**:
- Facilita sessões PO + Developer + QA
- Gera agenda estruturada
- Template de ata
- Checklist de outputs
- Estimativa conjunta de Dev + QA points

**Parâmetros**:
```bash
/onion-validate-collab-three-amigos <story_id> [--task-manager] [--generate-agenda]
```

**Exemplo**:
```bash
/onion-validate-collab-three-amigos CU-123 --generate-agenda
```

**Outputs**:
- User story refinada
- Dev story points estimados
- QA story points estimados
- Cross-testing points identificados
- Test strategy definida
- Definition of Done acordada

### 6. **`/onion-validate-collab-pair-testing`** - Teste em Par

**Funcionalidades**:
- Facilita sessões de teste em par
- Suporte a múltiplas combinações (Dev+Dev, Dev+QA, QA+QA)
- Estrutura de sessão com driver/navigator
- Documentação de findings em tempo real

**Parâmetros**:
```bash
/onion-validate-collab-pair-testing <feature-name> [--pair-type] [--duration]
```

---

## 🔄 Fluxos Integrados

### Fluxo 1: Desenvolvimento Completo com Testes

```
1. /onion-product-task "Criar feature de checkout"
   ↓
2. /onion-validate-collab-three-amigos CU-123
   → Estima Dev points + QA points
   → Define test strategy
   ↓
3. /onion-engineer-start checkout
   ↓
4. /onion-test-unit src/services/CheckoutService.ts --generate --run
   → Testes White-box (desenvolvedor)
   ↓
5. /onion-test-integration /api/checkout --generate --run --contract
   → Testes Grey-box (cross-dev)
   ↓
6. /onion-test-e2e checkout --generate --run
   → Testes Black-box (QA)
   ↓
7. /onion-validate-workflow pr-merge
   → Validação final antes de PR
```

### Fluxo 2: Criação de Estratégia Completa

```
1. /onion-validate-test-strategy-create checkout --risk-level alto
   → Lê framework_testes.md
   → Calcula QA points automaticamente
   → Cria épico no ClickUp com subtasks
   ↓
2. spawn_agent → test-agent: "Validar estratégia criada"
   → Revisa conformidade com framework
   → Sugere melhorias
   ↓
3. /onion-validate-test-strategy-analyze checkout
   → Analisa gaps e oportunidades
```

### Fluxo 3: Estimativa Precisa de QA

```
1. /onion-validate-qa-points-estimate "Testar sistema de pagamentos" --breakdown
   → Lê framework_testes.md
   → Analisa contexto
   → Calcula: Complexidade (8) + Risco (5) + Tipo (4) = 17 pontos
   → Breakdown:
     - White-box: 3 pontos (unit tests)
     - Grey-box: 5 pontos (integration)
     - Black-box: 9 pontos (E2E + exploratório)
   ↓
2. /onion-validate-qa-points-estimate --task-id CU-456 --update
   → Atualiza task no ClickUp com 17 QA points
```

---

## 📊 Métricas e Dashboards

### Métricas por Perspectiva

**White-box (Dev)**:
- Code Coverage: >80%
- Branch Coverage: >70%
- Mutation Score: >70%
- Unit Test Execution: <30s

**Grey-box (Cross-Dev)**:
- API Contract Coverage: 100%
- Integration Test Pass Rate: >95%
- Cross-team Review Time: <2h average

**Black-box (QA)**:
- QA Velocity: 25 pontos/sprint
- Estimation Accuracy: >80%
- Bug Detection Rate: >85%
- User Story Coverage: 100%

### Dashboard Integrado

```
┌─── WHITE-BOX (DEV) ────┐  ┌─── BLACK-BOX (QA) ────┐  ┌─── GREY-BOX (CROSS) ───┐
│ Coverage: 85% ✅       │  │ QA Velocity: 24pts ✅  │  │ API Tests: 47 ✅       │
│ Unit Tests: 247 ✅     │  │ Stories Tested: 8/10⚠️ │  │ Integration: 23 ✅     │
│ Mutation: 74% ✅       │  │ Bugs Found: 12 📊      │  │ Contract Tests: 15 ✅  │
│ Execution: 25s ✅      │  │ Estimation Acc: 87% ✅ │  │ Peer Reviews: 5 ✅     │
└────────────────────────┘  └────────────────────────┘  └─────────────────────────┘

┌───────────────── SPRINT OVERVIEW ─────────────────────┐
│ Combined Velocity: 47pts (Dev: 20, QA: 24, Cross: 3) │
│ Sprint Progress: ▓▓▓▓▓▓▓░░░ 75%                       │
│ Quality Gate: ✅ ALL PERSPECTIVES PASSING              │
│ Risk Score: 🟨 MEDIUM (cross-browser pending)         │
│ Deployment Ready: ⏳ 2 stories pending final QA      │
└────────────────────────────────────────────────────────┘
```

---

## 🎯 Quando Usar Cada Componente

### Para Desenvolvedores

**Durante desenvolvimento**:
- `/onion-test-unit` - Criar testes unitários
- `test-engineer` - Validar qualidade dos testes

**Antes de PR**:
- `/onion-test-integration` - Testar integrações
- `/onion-validate-workflow` - Validar workflow completo

### Para QA

**Planejamento**:
- `/onion-validate-qa-points-estimate` - Estimar esforço
- `/onion-validate-test-strategy-create` - Criar estratégia completa
- `test-planner` - Analisar cobertura

**Execução**:
- `/onion-test-e2e` - Testes end-to-end
- `/onion-validate-collab-pair-testing` - Sessões colaborativas

### Para Product Owners

**Refinement**:
- `/onion-validate-collab-three-amigos` - Sessões de refinement
- `/onion-validate-qa-points-estimate` - Entender esforço de teste

### Para Times Cross-funcionais

**Estratégia**:
- `test-agent` - Criar estratégias completas
- `/onion-validate-test-strategy-create` - Estratégias automatizadas

**Colaboração**:
- `/onion-validate-collab-three-amigos` - Alinhamento de times
- `/onion-validate-collab-pair-testing` - Transferência de conhecimento

---

## 🔗 Integrações

### Task Managers

Todas as skills de validação integram com (conforme `TASK_MANAGER_PROVIDER`):
- **ClickUp** (via MCP standalone declarado em `.zed/settings.json` → `context_servers`)
- **Jira** (via REST direto com `fetch`/`terminal`)
- **Asana / Linear** (via adapter)

**Funcionalidades**:
- Criar tasks/epics automaticamente
- Atualizar story points
- Sincronizar status
- Adicionar comentários estruturados

### CI/CD

Skills de teste geram:
- Configurações de pipeline
- Quality gates
- Relatórios de coverage
- Artefatos de teste

---

## 📚 Referências

### Knowledge Bases
- [`framework_testes.md`](../knowledge-base/frameworks/framework_testes.md) - Framework completo
- [`framework_story_points.md`](../knowledge-base/frameworks/framework_story_points.md) - Sistema de estimativas

### Skills Relacionadas
- `/onion-product-task` - Criar tasks
- `/onion-product-estimate` - Estimar story points de desenvolvimento
- `/onion-engineer-start` - Iniciar desenvolvimento
- `/onion-engineer-work` - Trabalhar em task
- `/onion-engineer-pr` - Criar Pull Request

### Specialists Relacionados (via `spawn_agent`)
- `product-agent` - Orquestração de produto
- `code-reviewer` - Review de código
- `onion` - Orquestrador master (também exposto como skill `onion`)

---

## 🚀 Quick Start

### Primeira Vez

1. **Entender o Framework**:
   ```bash
   # Ler framework completo
   cat docs/knowledge-base/frameworks/framework_testes.md
   ```

2. **Criar Primeira Estratégia**:
   ```bash
   /onion-validate-test-strategy-create minha-feature --risk-level médio
   ```

3. **Estimar QA Points**:
   ```bash
   /onion-validate-qa-points-estimate "Testar feature X" --breakdown
   ```

### Uso Diário

**Desenvolvedor**:
```bash
/onion-test-unit src/services/MyService.ts --generate --run --coverage
```

**QA**:
```bash
/onion-test-e2e login --generate --run --record
```

**Time**:
```bash
/onion-validate-collab-three-amigos CU-123 --generate-agenda
```

---

**Responsável**: Sistema Onion (nativo Zed)  
**Última Atualização**: 2026-06-03  
**Mantido por**: Skill `/onion-docs-build-index`

