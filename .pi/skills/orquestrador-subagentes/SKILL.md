---
name: orquestrador-subagentes
description: Orquestra subagentes em paralelo (tarefas independentes) ou sequencial (tarefas interdependentes). Use quando dividir trabalho complexo, coordenar múltiplos agentes, delegar domínios distintos, ou decidir entre execução paralela vs encadeada.
compatibility: Requer harness com suporte a subagentes ou tasks paralelas (pi, Cursor, Claude Code, etc.). Acesso a leitura/escrita de arquivos e shell.
---

# Orquestrador de Subagentes

## Visão Geral

Skill especializada em **decompor, rotear e coordenar** trabalho entre subagentes. Distingue explicitamente:

- **Paralelo**: subagentes independentes, sem dependência de saída, sem sobreposição de estado/arquivos.
- **Sequencial**: cada etapa depende da anterior; saída de A alimenta entrada de B.

Não executa o domínio final — **projeta o plano de orquestração**, dispara subagentes e consolida resultados.

## Audiência e Contexto de Uso

- **Quem**: agente líder, tech lead automatizado, coordenador de fluxos multi-etapa.
- **Quando acionar**:
  - "Divida isso em subagentes"
  - "Execute em paralelo"
  - "Faça X depois de Y"
  - trabalho com domínios independentes (frontend + backend + docs)
  - refatorações amplas com fronteiras de arquivo claras
  - pesquisa paralela em tópicos distintos

## Ambiente e Dependências

- Harness com capacidade de spawn de subagentes (`task`, sub-agents, sessões paralelas).
- Isolamento recomendado: branches, worktrees ou diretórios separados para tarefas paralelas que tocam código.
- Aplicar ciclo obrigatório do agente (contexto → reflexão → ação → registro).

## Comportamento Passo a Passo

### 1. Análise 5W2H da solicitação

| Dimensão | Perguntas |
|----------|-----------|
| What | Qual entrega final? Quais subentregas? |
| Who | Quais especialistas/subagentes? |
| When | O que pode rodar agora vs depois? |
| Where | Quais arquivos/módulos cada um toca? |
| Why | Paralelismo reduz tempo ou só adiciona risco? |
| How | Qual topologia (paralelo, sequencial, híbrido)? |
| How much | Quantos subagentes (ideal: 3–5 simultâneos)? |

### 2. Decisão paralelo vs sequencial

**Use paralelo** somente se TODAS forem verdadeiras:
- 2+ tarefas genuinamente independentes
- Sem estado compartilhado mutável
- Fronteiras de arquivo/domínio sem sobreposição
- Critérios de aceite verificáveis por subagente

**Use sequencial** se QUALQUER uma for verdadeira:
- B depende da saída completa de A
- Mesmos arquivos ou tipos compartilhados
- Escopo ambíguo (precisa entender antes de agir)
- Migração de schema → código que usa o schema

**Prefira agente único** quando: bug fix localizado, requisito ambíguo, ou acoplamento alto sem spec escrita.

### 3. Decomposição atômica

Para cada sub-tarefa, definir:
- **ID** e **descrição** em uma frase
- **Entrada** necessária (artefatos, contexto)
- **Saída** esperada (arquivos, decisões, evidências)
- **Verificação binária** (teste passa, lint limpo, critério manual)
- **Dependências** (IDs de tarefas predecessoras)
- **Modo**: `parallel` | `sequential` | `background`

### 4. Especificação mínima por subagente

Cada prompt de subagente deve conter:
1. Objetivo e escopo (incluindo o que **não** fazer)
2. Contratos de interface que pode ler mas não alterar
3. Arquivos permitidos vs proibidos
4. Critérios de aceite mensuráveis
5. Comandos de validação obrigatórios ao final

### 5. Execução

**Paralelo:**
```
Fase 1: [A, B, C] em paralelo
Fase 2: consolidar + resolver conflitos
Fase 3: [D] sequencial (integração)
```

- Limitar concorrência a 3–5 subagentes
- Tarefas de pesquisa/análise podem rodar em background
- Nunca paralelizar edições no mesmo arquivo

**Sequencial:**
```
A (contexto) → B (design) → C (implementação) → D (validação)
```

- Passar handoff estruturado: decisões, arquivos alterados, pendências, riscos

### 6. Consolidação e validação

1. Coletar saídas de todos os subagentes
2. Verificar critérios de aceite individualmente
3. Detectar conflitos (merge, contratos quebrados, duplicação)
4. Executar validação integrada (testes, build, smoke)
5. Registrar estado: `estado persistido do ciclo` e eventos

### 7. Formato de saída

```markdown
## Plano de Orquestração

### Topologia
[paralelo | sequencial | híbrido]

### Grafo de dependências
[mermaid ou lista]

### Tarefas
| ID | Modo | Subagente | Escopo | Depende de | Verificação |
|----|------|-----------|--------|------------|-------------|

### Riscos e mitigação
- ...

### Próximo passo
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"Implemente autenticação OAuth: backend API, frontend botão Google e testes."

### Saída esperada
Plano híbrido:
- **Paralelo**: backend OAuth route + frontend botão (arquivos distintos)
- **Sequencial depois**: testes E2E dependem de ambos
- Spec com contrato da API antes de disparar subagentes

### Entrada
"Pesquise 3 frameworks de ORM em paralelo."

### Saída esperada
3 subagentes background, domínios independentes, consolidação em tabela comparativa ao final.

## Casos Limite e Armadilhas

- **Over-parallelizing**: 10 micro-tarefas paralelas → overhead e conflitos. Agrupar.
- **Under-parallelizing**: análises independentes em série → desperdício de tempo.
- **Sem spec**: subagentes tomam decisões implícitas conflitantes. Escrever spec mínima primeiro.
- **Merge hell**: paralelizar sem isolamento de branch/worktree.
- **Subagente sem validação**: sempre exigir evidência objetiva antes de merge.

## Integração com outras skills

| Situação | Delegar para |
|----------|--------------|
| Implementação | `desenvolvedor-codigo` (+ especialização) |
| Debug após integração | `depurador-codigo` |
| Testes | `testador-codigo` |
| Pesquisa prévia | `pesquisador-tecnico` |
| Planejamento de release | `gerente-projetos-ti` |

## Referências Consultadas

- Agent Skills Specification: https://agentskills.io/specification
- Multi-Agent Pipelines: https://agentguides.dev/agentic-workflows/multi-agent/
- Sub-Agent Parallel vs Sequential: https://claudefa.st/blog/guide/agents/sub-agent-best-practices
- When Multi-Agent Is Overkill: https://www.augmentcode.com/guides/when-multi-agent-ai-is-overkill
- Task Decomposition for Agents: https://lab.asquaresolution.com/tracks/claude-code-operator/prompt-engineering/task-decomposition
- AI Agent Orchestration 2026: https://amux.io/guides/ai-agent-orchestration-2026/
