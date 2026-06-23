---
name: analista-qa
description: Analisa qualidade de software — planos de teste, testes exploratórios, critérios de aceite, relatórios de defeitos e readiness de release. Use para validação pré-release, charters exploratórios, revisão de requisitos e garantia de qualidade.
compatibility: Acesso ao sistema sob teste, requisitos, testes automatizados e ambiente de staging. Stack-agnóstico.
---

# Analista QA

## Visão Geral

Skill para **garantir qualidade** através de planejamento, execução exploratória estruturada, validação de critérios de aceite e comunicação clara de riscos. Complementa `testador-codigo` (automação) com foco em **julgamento, cobertura de risco e release readiness**.

| Papel | testador-codigo | analista-qa |
|-------|-----------------|-------------|
| Foco | Escrever/rodar testes automatizados | Estratégia, exploração, aceite, relatórios |
| Saída | Código de teste | Planos, charters, bug reports, go/no-go |

## Audiência e Contexto de Uso

- **Quem**: QA analysts, testers, agentes de validação pré-release.
- **Quando acionar**:
  - "Valide se está pronto para release"
  - "Crie plano de testes"
  - teste exploratório de feature nova
  - revisar critérios de aceite
  - reportar bugs com reprodução clara
  - regressão manual de áreas de risco

## Ambiente e Dependências

- Requisitos, user stories, critérios de aceite.
- Ambiente de teste (staging, preview deploy).
- Suite automatizada existente (consultar `testador-codigo` para gaps).
- Seguir ciclo padrão do agente para registro de evidências.

## Comportamento Passo a Passo

### 1. Entender o que validar (5W2H)

| Dimensão | Perguntas |
|----------|-----------|
| What | Qual feature/release? O que é "pronto"? |
| Who | Quais personas/roles afetados? |
| When | Deadline? Bloqueantes vs nice-to-have? |
| Where | Ambiente, browsers, dispositivos? |
| Why | Risco de negócio se falhar? |
| How | Automatizado, exploratório ou ambos? |
| How much | Profundidade vs prazo? |

### 2. Plano de testes

Responder três perguntas:
1. **O que** testar? (escopo e fora de escopo)
2. **Como** testar? (exploratório, scripted, regressão auto)
3. **Quem/quando**? (sessões, responsáveis, cronograma)

Incluir:
- **Exit criteria** — quando parar com confiança
- **Ambiente** — dados, contas, config
- **Riscos** priorizados (matriz impacto × probabilidade)

### 3. Testes exploratórios (SBTM)

**Session-Based Test Management:**

1. **Charter** antes da sessão (60–90 min):
   ```
   Explore [área] with [recursos] to discover [riscos]
   ```
2. **Time-box** a sessão
3. **Notas em tempo real** — bugs, perguntas, surpresas
4. **Debrief** após sessão:
   - O que foi coberto?
   - Bugs encontrados (severidade)
   - Follow-up charters necessários?

**Heurísticas úteis:**
- SFDPOT: Structure, Function, Data, Platform, Operations, Time
- Tours: Money Tour, Saboteur Tour, Landmark Tour

### 4. Execução e evidências

Para cada defeito:
```markdown
## Bug: [título]

**Severidade:** critical | major | minor | trivial
**Ambiente:** ...
**Reprodução:**
1. ...
**Esperado:** ...
**Atual:** ...
**Evidência:** screenshot, log, request ID
```

### 5. Release readiness

Checklist go/no-go:
- [ ] Critérios de aceite da story atendidos
- [ ] Regressão crítica passou (auto + manual de risco)
- [ ] Defeitos bloqueantes resolvidos ou aceitos explicitamente
- [ ] Performance/segurança revisados se aplicável
- [ ] Documentação/release notes atualizadas
- [ ] Rollback testado ou documentado

### Formato de saída — Relatório QA

```markdown
## Relatório de Qualidade — [Release/Feature]

### Escopo testado
- ...

### Cobertura
| Área | Método | Status | Notas |
|------|--------|--------|-------|

### Defeitos
| ID | Sev | Status | Resumo |
|----|-----|--------|--------|

### Riscos residuais
- ...

### Recomendação
GO | NO-GO | GO com ressalvas

### Ressalvas
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"Valide o checkout antes do Black Friday."

### Saída esperada
Plano focado em pagamento, estoque, concorrência; charters exploratórios; matriz de risco; recomendação go/no-go.

### Entrada
"Explore a nova tela de perfil."

### Saída esperada
Charter SBTM, sessão 90min, debrief com bugs e gaps de cobertura.

## Casos Limite e Armadilhas

- **Checkbox QA**: executar casos sem pensar em riscos.
- **Exploração sem charter**: perda de accountability.
- **Bug sem reprodução**: dev não consegue corrigir.
- **Go com bloqueantes**: escalar decisão, não assumir.
- **Duplicar automação**: QA foca no que auto não cobre.

## Integração com outras skills

| Necessidade | Skill |
|-------------|-------|
| Automatizar casos encontrados | `testador-codigo` |
| Investigar causa de bug | `depurador-codigo` |
| Corrigir defeito | `desenvolvedor-codigo` |
| Planejar sprint/release | `gerente-projetos-ti` |

## Referências Consultadas

- Exploratory Testing Guide: https://yrkan.com/blog/exploratory-testing-guide/
- Test Charter Writing: https://yrkan.com/blog/test-charter-writing/
- SSW Rules — Manage Exploratory Testing: https://www.ssw.com.au/rules/manage-report-exploratory-testing/
- Test Planning Guide: https://testpad.com/test-planning-guide/
- ISTQB — test planning and exit criteria
