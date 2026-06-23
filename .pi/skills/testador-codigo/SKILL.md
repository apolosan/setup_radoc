---
name: testador-codigo
description: Projeta e executa testes de software — unitários, integração, contrato, E2E, regressão, segurança e performance. Use antes de implementar (TDD), para validar features, aumentar cobertura ou reproduzir bugs.
compatibility: Acesso a codebase, framework de testes do projeto e shell. Stack detectada em runtime, não presumida.
---

# Testador de Código-Fonte

## Visão Geral

Skill para **definir, escrever e executar** testes que provem comportamento esperado. Alinha-se ao TDD: testes antes de código de produção quando aplicável. Foco em testes úteis que capturam comportamento real, não em cobertura cosmética.

## Audiência e Contexto de Uso

- **Quem**: desenvolvedores, QA engineers, agentes de validação.
- **Quando acionar**:
  - "Escreva testes para..."
  - "Aumente cobertura de..."
  - início de feature (TDD)
  - bug reportado (teste de regressão)
  - antes de merge/release
  - validar contrato de API

## Ambiente e Dependências

- Descobrir framework de testes do projeto (não assumir Jest, pytest, etc.).
- Consultar ciclo padrão do agente: TDD obrigatório antes de código de produção.
- Ferramentas: runner de testes, linter, coverage quando existir.

## Comportamento Passo a Passo

### 1. Contexto e escopo (5W2H)

| Dimensão | Perguntas |
|----------|-----------|
| What | Qual comportamento deve ser provado? |
| Who | Quem consome o resultado (dev, CI, QA)? |
| When | Pré ou pós-implementação? Bloqueante para merge? |
| Where | Camada (unit, integration, E2E)? |
| Why | Risco se não testar? |
| How | Qual tipo de teste adequado? |
| How much | Quantos casos mínimos para confiança? |

### 2. Descobrir convenções do projeto

1. Localizar testes existentes (`**/test/**`, `**/*.test.*`, `**/*.spec.*`).
2. Identificar runner, assertions, fixtures, mocks.
3. Ler um teste representativo como template.
4. Verificar como CI executa testes.

### 3. Selecionar tipo de teste

| Tipo | Quando usar |
|------|-------------|
| **Unitário** | Regras isoladas, funções puras, value objects |
| **Integração** | DB, filas, APIs, adaptadores reais |
| **Contrato** | Fronteiras entre serviços/módulos |
| **E2E/Smoke** | Fluxos críticos do usuário |
| **Regressão** | Bug corrigido não volta |
| **Segurança** | Auth, injeção, secrets |
| **Performance** | SLAs, latência, throughput |
| **Acessibilidade** | UI com requisitos WCAG |

### 4. Ciclo TDD (quando aplicável)

```
RED   → escrever teste que falha pelo comportamento ausente
GREEN → implementar mínimo para passar (delegar a desenvolvedor-codigo se necessário)
REFACTOR → melhorar sem alterar comportamento
REPEAT
```

### 5. Estrutura de cada teste

```text
Arrange — setup de dados e dependências
Act     — executar unidade sob teste
Assert  — verificar resultado esperado
Cleanup — quando necessário (DB, arquivos)
```

Nomear testes pelo **comportamento**, não pelo método:
- Ruim: `testCreateUser`
- Bom: `rejects duplicate email with validation error`

### 6. Casos a cobrir

Para cada comportamento:
- **Happy path** — fluxo principal
- **Edge cases** — limites, vazios, null, unicode
- **Error paths** — exceções, códigos HTTP, rollback
- **Idempotência** — quando relevante

### 7. Executar e reportar

1. Rodar testes novos isoladamente
2. Rodar suite relacionada
3. Reportar: passou/falhou, cobertura se disponível, lacunas

### Formato de saída

```markdown
## Plano de Testes

### Comportamento sob teste
...

### Casos
| ID | Tipo | Cenário | Resultado esperado |
|----|------|---------|-------------------|

### Arquivos
- ...

### Execução
```
comando → resultado
```

### Lacunas / riscos
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"Teste a função de cálculo de desconto."

### Saída esperada
Casos: sem desconto, desconto válido, desconto > 100%, valor negativo; testes unitários seguindo convenção do projeto.

### Entrada
"Garanta que o fluxo de login funciona."

### Saída esperada
Smoke/E2E com credenciais de teste, casos de senha errada e usuário inexistente.

## Casos Limite e Armadilhas

- **Testes triviais**: assertar que getter retorna valor setado.
- **Testes frágeis**: acoplados a implementação interna.
- **Mock excessivo**: integração real é mais valiosa quando viável.
- **Flaky tests**: timing, ordem, estado global — isolar e estabilizar.
- **Cobertura 100% sem significado**: priorizar caminhos críticos.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Bug encontrado | `depurador-codigo` |
| Implementar para passar teste | `desenvolvedor-codigo` |
| Plano de release | `analista-qa` |
| Orquestrar testes paralelos | `orquestrador-subagentes` |

## Referências Consultadas

- Test Planning Guide: https://testpad.com/test-planning-guide/
- ISTQB Experience-Based Testing (exploratory complement)
- Kent Beck, Test-Driven Development (princípios TDD)
- Agent Skills Specification: https://agentskills.io/specification
