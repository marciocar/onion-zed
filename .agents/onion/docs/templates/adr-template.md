---
# Claude Code - ADR Template Metadata
template:
  type: adr
  version: 2.0
  category: architecture-decision
  adr_number: "XXX"

decision_metadata:
  status: "[Proposed | Accepted | Rejected | Superseded]"
  date: "YYYY-MM-DD"
  deciders: []
  technical_story: "[Link para issue/ticket se aplicável]"

quality_attributes:
  performance: false
  security: false
  maintainability: false
  scalability: false
  usability: false
  reliability: false
  testability: false

alternatives:
  total_considered: 3
  selected: "[Nome da alternativa selecionada]"

decision_matrix:
  criteria:
    - name: "Performance"
      weight: 20
    - name: "Maintainability"
      weight: 25
    - name: "Development Speed"
      weight: 15
    - name: "Cost"
      weight: 20
    - name: "Risk"
      weight: 20

consequences:
  positive: []
  negative: []
  neutral: []

related_decisions:
  supersedes: []
  superseded_by: []
  relates_to: []
  impacts: []

implementation:
  timeline:
    short_term: "[1-3 months]"
    medium_term: "[3-6 months]"
    long_term: "[6+ months]"
  next_review: "[Data]"

ai_assistant:
  suggest_alternatives: true
  validate_consequences: true
  track_implementation: true
  monitor_metrics: true
  build_dependency_graph: true
---

# ADR-XXX: [Título da Decisão Arquitetural]

**Status:** [Proposed | Accepted | Rejected | Superseded]  
**Date:** YYYY-MM-DD  
**Deciders:** [Lista de pessoas envolvidas]  
**Technical Story:** [Link para issue/ticket se aplicável]

---

## 📋 **Context and Problem Statement**

[Descrever o contexto arquitetural, tecnológico ou de negócio que levou à necessidade desta decisão. Incluir:]

- **Problema específico** que precisamos resolver
- **Restrições** t,écnicas, de tempo ou de recursos
- **Drivers** de qualidade (performance, security, maintainability, etc.)
- **Stakeholders** afetados pela decisão

### **Quality Attributes Affected**
- [ ] Performance
- [ ] Security  
- [ ] Maintainability
- [ ] Scalability
- [ ] Usability
- [ ] Reliability
- [ ] Testability

---

## 🎯 **Decision**

[Descrever a decisão tomada de forma clara e concisa]

### **Key Decision Points**
1. **Tecnologia/Approach:** [Escolha principal]
2. **Implementation Strategy:** [Como será implementado]
3. **Migration Plan:** [Se aplicável]

---

## 🔍 **Alternatives Considered**

### **Alternative 1: [Nome da alternativa]**
- **Pros:** 
  - [Vantagem 1]
  - [Vantagem 2]
- **Cons:**
  - [Desvantagem 1]
  - [Desvantagem 2]
- **Cost/Effort:** [Alto/Médio/Baixo]

### **Alternative 2: [Nome da alternativa]**
- **Pros:** 
  - [Vantagem 1]
  - [Vantagem 2]
- **Cons:**
  - [Desvantagem 1]
  - [Desvantagem 2]
- **Cost/Effort:** [Alto/Médio/Baixo]

### **Alternative 3: [Nome da alternativa]**
- **Pros:** 
  - [Vantagem 1]
  - [Vantagem 2]
- **Cons:**
  - [Desvantagem 1]
  - [Desvantagem 2]
- **Cost/Effort:** [Alto/Médio/Baixo]

---

## ⚖️ **Decision Matrix**

| Criteria | Weight | Alt 1 | Alt 2 | Alt 3 | **Selected** |
|----------|--------|-------|-------|-------|--------------|
| Performance | 20% | 3 | 4 | 2 | **4** |
| Maintainability | 25% | 4 | 3 | 5 | **3** |
| Development Speed | 15% | 5 | 2 | 3 | **2** |
| Cost | 20% | 2 | 5 | 4 | **5** |
| Risk | 20% | 4 | 3 | 2 | **3** |
| **Total Score** | | **3.4** | **3.4** | **3.2** | **3.4** |

*Scale: 1 (Poor) to 5 (Excellent)*

---

## 📊 **Consequences**

### **✅ Positive Consequences**
- [Consequência positiva 1 - específica e mensurável]
- [Consequência positiva 2 - específica e mensurável]
- [Consequência positiva 3 - específica e mensurável]

### **⚠️ Negative Consequences**
- [Consequência negativa 1 - e como mitigar]
- [Consequência negativa 2 - e como mitigar]
- [Consequência negativa 3 - e como mitigar]

### **🔄 Neutral Consequences**
- [Mudanças que são neutras mas importantes de documentar]

---

## 📈 **Expected Outcomes and Metrics**

### **Success Metrics**
- [ ] **Metric 1:** [Específica - ex: Response time < 200ms]
- [ ] **Metric 2:** [Específica - ex: 95% test coverage]
- [ ] **Metric 3:** [Específica - ex: Zero breaking changes]

### **Timeline**
- **Short-term (1-3 months):** [Resultados esperados]
- **Medium-term (3-6 months):** [Resultados esperados]
- **Long-term (6+ months):** [Resultados esperados]

---

## 🔗 **Related Decisions**

- [Link para ADR relacionado]
- [Link para ADR que esta decisão substitui]
- [Link para ADR que pode ser impactado]

---

## 📚 **References**

### **Technical References**
- [Link para documentação técnica]
- [Link para benchmarks ou estudos]
- [Link para best practices]

### **Implementation References**
- [Link para código relevante]
- [Link para configurações]
- [Link para testes]

---

## 📝 **Implementation Notes**

### **Action Items**
- [ ] [Item específico com responsável e prazo]
- [ ] [Item específico com responsável e prazo]
- [ ] [Item específico com responsável e prazo]

### **Risk Mitigation**
- **Risk 1:** [Risco] → **Mitigation:** [Como mitigar]
- **Risk 2:** [Risco] → **Mitigation:** [Como mitigar]

### **Review Schedule**
- **Next Review:** [Data]
- **Review Criteria:** [O que avaliar na próxima revisão]

---

**📅 Created:** [Date]  
**👤 Author:** [Name and role]  
**🔄 Last Updated:** [Date]  
**📋 Status:** [Current status] 