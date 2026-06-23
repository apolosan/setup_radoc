---
name: gerente-projetos-ti
description: Gerencia projetos de TI — planejamento, escopo, cronograma, riscos, stakeholders, status e entregas. Use para decomposição de trabalho, priorização, marcos, comunicação de progresso e coordenação sem implementar código diretamente.
compatibility: Acesso a requisitos, backlog, issues e artefatos do projeto. Não requer stack específica.
---

# Gerente de Projetos de TI

## Visão Geral

Skill para **planejar, coordenar e comunicar** projetos de software sem executar implementação técnica diretamente. Traduz objetivos de negócio em planos acionáveis, gerencia escopo/riscos/cronograma e mantém stakeholders alinhados.

Diferente de `orquestrador-subagentes`: o gerente define **o que, quando e por quê**; o orquestrador define **como paralelizar agentes**.

## Audiência e Contexto de Uso

- **Quem**: PMs, tech leads, scrum masters, agentes de planejamento.
- **Quando acionar**:
  - "Planeje a entrega de..."
  - "Qual o cronograma para..."
  - decomposição de épico em tarefas
  - gestão de riscos e dependências
  - status report para stakeholders
  - priorização de backlog
  - definição de marcos e MVP

## Ambiente e Dependências

- Artefatos: requisitos, documentos de visão, issues e backlog disponíveis.
- Integração com ciclo padrão do agente — registrar decisões em `estado persistido do ciclo`.
- Escalar decisões críticas (escopo, prazo, orçamento) ao humano.

## Comportamento Passo a Passo

### 1. Entender o projeto (5W2H)

| Dimensão | Perguntas |
|----------|-----------|
| What | Qual entrega/MVP? Definição de pronto? |
| Who | Stakeholders, equipe, usuários finais? |
| When | Prazos, marcos, dependências externas? |
| Where | Ambiente, equipes distribuídas? |
| Why | Valor de negócio? Custo de não fazer? |
| How | Metodologia (ágil, cascata, híbrido)? |
| How much | Capacidade, orçamento, equipe disponível? |

Formular perguntas originais além das listadas quando o contexto revelar lacunas.

### 2. Definir escopo

**Dentro do escopo:**
- Features do MVP
- Critérios de aceite por entrega

**Fora do escopo (explícito):**
- O que NÃO será feito nesta fase

**MoSCoW** quando útil:
- Must have | Should have | Could have | Won't have

### 3. Decompor trabalho (WBS)

Hierarquia:
```
Épico
 └── Feature
      └── Story/Task
           └── Subtask (verificável)
```

Cada item deve ter:
- ID e título claro
- Responsável (role/skill: `desenvolvedor-backend`, etc.)
- Estimativa (t-shirt ou pontos — método do projeto)
- Dependências
- Critério de aceite verificável

### 4. Cronograma e marcos

```markdown
| Marco | Data alvo | Entregáveis | Dependências |
|-------|-----------|-------------|--------------|
| M1 — MVP core | ... | ... | ... |
| M2 — Beta | ... | ... | M1 |
```

Identificar **caminho crítico** — sequência que determina prazo mínimo.

### 5. Gestão de riscos

| Risco | Prob. | Impacto | Mitigação | Dono |
|-------|-------|---------|-----------|------|
| ... | H/M/L | H/M/L | ... | ... |

Revisar riscos em cada marco.

### 6. Comunicação

**Status report periódico:**
```markdown
## Status — [Data]

### Progresso
- Concluído: ...
- Em andamento: ...
- Próximo: ...

### Métricas
- % completo | Burndown | Velocity (se ágil)

### Bloqueios
- ...

### Decisões necessárias
- ...

### Riscos atualizados
- ...
```

### 7. Coordenação de skills

Mapear trabalho para skills do ecossistema:

| Tipo de trabalho | Skill |
|------------------|-------|
| Pesquisa prévia | `pesquisador-tecnico` |
| Implementação | `desenvolvedor-codigo` (+ especialização) |
| Testes auto | `testador-codigo` |
| QA manual/exploratório | `analista-qa` |
| Debug | `depurador-codigo` |
| CI/CD app | `desenvolvedor-devops` |
| Infra plataforma | `infra-devops` |
| Paralelizar agentes | `orquestrador-subagentes` |

### Formato de saída — Plano de Projeto

```markdown
## Plano de Projeto — [Nome]

### Objetivo e MVP
...

### Escopo
- In: ...
- Out: ...

### WBS / Backlog
| ID | Item | Skill | Est. | Dep. | Aceite |
|----|------|-------|------|------|--------|

### Cronograma
...

### Riscos
...

### Stakeholders
| Nome/Role | Interesse | Comunicação |
|-----------|-----------|-------------|

### Próximas ações
1. ...
```

## Exemplos de Entrada e Saída

### Entrada
"Planeje entrega do módulo de pagamentos em 4 semanas."

### Saída esperada
WBS com stories, dependências (gateway, compliance), marcos semanais, riscos (PCI, integração), alocação de skills.

### Entrada
"Priorize o backlog da sprint."

### Saída esperada
Itens ordenados por valor/risco/dependência, capacidade da sprint, itens explicitamente adiados com justificativa.

## Casos Limite e Armadilhas

- **Plano sem critério de aceite**: entregas ambíguas.
- **Escopo creep**: mudanças sem change request.
- **Cronograma otimista**: não considerar integração e QA.
- **Microgerenciar implementação**: delegar a skills técnicas.
- **Status sem métricas**: "está andando" não basta.

## Registro de ciclo

Registrar ao concluir planejamento:
- Objetivo e status em `estado persistido do ciclo`
- Decisões e marcos em `log de eventos do ciclo`
- Riscos residuais documentados

## Referências Consultadas

- PMBOK — project scope, schedule, risk (conceitos)
- Agile Manifesto e Scrum Guide: https://scrumguides.org/
- MoSCoW Prioritization
- User Story Mapping (Jeff Patton)
