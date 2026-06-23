---
name: pesquisador-tecnico
description: Pesquisa técnicas, tecnologias, frameworks, padrões e abordagens com fontes verificadas. Use antes de decisões arquiteturais, adoção de libs, comparativos, POCs conceituais ou quando faltar evidência externa.
compatibility: Requer acesso a busca web e leitura de documentação. Agnóstico de stack.
---

# Pesquisador Técnico

## Visão Geral

Skill para **explorar e sintetizar conhecimento externo** de forma rastreável. Fundamenta decisões com evidências — não com opinião do modelo. Princípio fundamental: **pesquisa antes de comportamento**.

## Audiência e Contexto de Uso

- **Quem**: arquitetos, tech leads, agentes em fase de descoberta.
- **Quando acionar**:
  - "Pesquise alternativas para..."
  - "Compare X vs Y"
  - "Quais as melhores práticas para..."
  - antes de adotar nova tecnologia
  - lacunas de conhecimento bloqueando implementação
  - due diligence de biblioteca/framework

## Ambiente e Dependências

- Ferramentas de busca web e fetch de documentação.
- Registrar fontes em toda entrega.
- Integrar com ciclo padrão do agente — contexto antes de ação.

## Comportamento Passo a Passo

### 1. Definir questão de pesquisa (5W2H)

| Dimensão | Perguntas |
|----------|-----------|
| What | O que precisa ser descoberto? |
| Who | Quem usará o resultado? (nível técnico) |
| When | Urgência? Decisão bloqueante? |
| Where | Contexto (cloud, on-prem, mobile, etc.) |
| Why | Qual decisão a pesquisa desbloqueia? |
| How | Formato de entrega (tabela, ADR, resumo)? |
| How much | Profundidade, número de fontes, tempo disponível? |

Se a pergunta for ampla, **decompor** em sub-questões focadas.

### 2. Formular queries

- Derivar queries das lacunas do 5W2H
- Incluir: ano recente, versão, "best practices", "comparison"
- Buscar documentação **oficial** primeiro
- Depois: benchmarks independentes, case studies, GitHub issues conhecidos

Exemplos:
- `PostgreSQL vs MongoDB transactional workloads 2025`
- `OpenTelemetry Node.js setup official docs`

### 3. Coletar e avaliar fontes

Para cada fonte:
| Critério | Avaliação |
|----------|-----------|
| Autoridade | Oficial > benchmark independente > blog |
| Recência | Preferir < 2 anos para tech em evolução |
| Relevância | Mesmo contexto de uso |
| Viés | Vendor marketing — cruzar fontes |

Em conflito: priorizar documentação oficial; registrar divergência.

### 4. Sintetizar achados

Estruturar resposta:
1. **Resumo executivo** (3–5 frases)
2. **Opções identificadas** com prós/contras
3. **Recomendação** condicionada ao contexto (não absoluta)
4. **Riscos e trade-offs**
5. **Próximos passos** (POC, spike, ADR)
6. **Referências** com URLs

### 5. Formato comparativo (quando aplicável)

```markdown
| Critério | Opção A | Opção B | Opção C |
|----------|---------|---------|---------|
| Maturidade | ... | ... | ... |
| Curva aprendizado | ... | ... | ... |
| Performance | ... | ... | ... |
| Comunidade | ... | ... | ... |
| Fit para [contexto] | ... | ... | ... |
```

### 6. Entregar com incerteza explícita

- Declarar o que **não** foi possível verificar
- Sugerir POC quando evidência for insuficiente
- Não recomendar adoção sem alinhar constraints do projeto

### Formato de saída

```markdown
## Pesquisa: [Tópico]

### Questão
...

### Resumo
...

### Achados
...

### Recomendação
...

### Riscos
- ...

### Lacunas
- ...

### Referências
1. [Título](URL) — relevância
```

## Exemplos de Entrada e Saída

### Entrada
"Compare Redis vs Memcached para cache de sessão."

### Saída esperada
Tabela comparativa, trade-offs de persistência e estruturas de dados, recomendação condicionada a requisitos de sessão, 5+ referências.

### Entrada
"Melhores práticas de observabilidade para microsserviços."

### Saída esperada
Pilares (métricas, logs, traces), OpenTelemetry como padrão emergente, anti-patterns, links para SRE book e docs oficiais.

## Casos Limite e Armadilhas

- **Resposta sem fonte**: viola o princípio de pesquisa fundamentada; sempre citar.
- **Fonte única**: cruzar mínimo 2–3 fontes independentes.
- **Marketing como evidência**: tratar com ceticismo.
- **Stack assumida**: perguntar constraints antes de recomendar.
- **Pesquisa infinita**: time-box; entregar com lacunas declaradas.

## Integração com outras skills

| Após pesquisa | Skill |
|---------------|-------|
| Implementar escolha | `desenvolvedor-codigo` (+ especialização) |
| POC paralelo de opções | `orquestrador-subagentes` |
| Decisão arquitetural formal | ADR + `gerente-projetos-ti` |
| Validar em produção | `analista-qa` |

## Referências Consultadas

- Agent Skills Specification: https://agentskills.io/specification
- 5W2H Framework: https://en.wikipedia.org/wiki/5W1H
- Evidence-Based Software Engineering practices
- Prompt engineering — grounding in sources: https://platform.openai.com/docs/guides/prompt-engineering
