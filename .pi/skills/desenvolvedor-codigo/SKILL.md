---
name: desenvolvedor-codigo
description: Implementa novas funcionalidades e alterações de código-fonte com escopo mínimo, convenções do projeto e validação objetiva. Use para features, refactors localizados, correções não-debug e evolução de código. Delegue especializações via sub-skills.
compatibility: Acesso a leitura/escrita de código, shell, testes e linters. Stack detectada em runtime.
---

# Desenvolvedor / Implementador de Código

## Visão Geral

Skill **genérica de implementação** para escrever e modificar código-fonte. Opera de forma agnóstica de stack, derivando convenções do projeto existente. Para domínios específicos, **ativar sub-skill** correspondente antes de implementar.

## Audiência e Contexto de Uso

- **Quem**: agentes de codificação, pair programming automatizado.
- **Quando acionar**:
  - "Implemente..."
  - "Adicione feature..."
  - "Refatore X para Y"
  - código após testes escritos (TDD green)
  - evolução de API ou módulo

## Sub-skills especializadas

| Domínio | Skill | Acionar quando |
|---------|-------|----------------|
| UI Web/Mobile | `desenvolvedor-frontend` | componentes, páginas, responsividade, a11y |
| APIs/Serviços | `desenvolvedor-backend` | endpoints, domínio, persistência, auth |
| Smart contracts | `desenvolvedor-blockchain` | contratos, wallets, on-chain |
| CI/CD/Pipelines | `desenvolvedor-devops` | Docker, GitHub Actions, IaC de app |

Se o domínio for claro, carregar a sub-skill **antes** de codificar.

## Ambiente e Dependências

- Seguir ciclo padrão do agente: contexto → reflexão → **testes antes de produção** → ação → validação → registro.
- Descobrir MCP/ferramentas disponíveis antes de agir.
- Não introduzir dependências sem justificativa.

## Comportamento Passo a Passo

### 1. Obter contexto

1. Ler código adjacente e arquivos relacionados.
2. Identificar padrões: naming, estrutura, imports, error handling.
3. Verificar issues, ADRs, docs do módulo.
4. Consultar `estado persistido do ciclo` se existir.

### 2. Planejar mudança mínima

- Definir **escopo exato** — o que entra e o que fica fora.
- Listar arquivos a tocar.
- Identificar testes existentes ou necessários.
- Avaliar riscos (breaking changes, migração, segurança).

### 3. TDD quando aplicável

1. Confirmar que testes expressam o comportamento (ou escrevê-los via `testador-codigo`).
2. Implementar o **mínimo** para testes passarem.
3. Não expandir escopo durante green.

### 4. Implementar

Princípios:
- **Reutilizar** funções e abstrações existentes.
- **Match conventions** — código deve parecer do mesmo autor.
- **Mudança focada** — sem refactor não solicitado.
- **Comentários** só para lógica não óbvia.
- **Segurança** — sem secrets hardcoded, validar inputs.

### 5. Validar

Executar na ordem:
1. Testes novos e relacionados
2. Linter / type checker se existir
3. Build se aplicável
4. Smoke manual quando E2E não cobre

### 6. Registrar

Atualizar `estado persistido do ciclo` e `log de eventos do ciclo` com arquivos alterados, decisões e próximo passo.

### Formato de saída

```markdown
## Implementação

### Objetivo
...

### Arquivos alterados
- `path` — resumo da mudança

### Decisões
- ...

### Validação
- [ ] Testes: ...
- [ ] Lint: ...

### Riscos / pendências
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"Adicione validação de CPF no cadastro de usuário."

### Saída esperada
Teste de validação, função reutilizável ou integração em form existente, mensagens de erro consistentes com o projeto.

### Entrada
"Crie endpoint REST para listar pedidos paginados."

### Saída esperada
Ativar `desenvolvedor-backend`, contrato da API, testes de integração, paginação seguindo padrão do projeto.

## Casos Limite e Armadilhas

- **Over-engineering**: abstrações para uso único.
- **Escopo creep**: implementar além do pedido.
- **Ignorar convenções**: estilo diferente do restante do codebase.
- **Sem testes**: viola ciclo padrão do agente.
- **Assumir stack**: sempre descobrir no projeto.

## Integração com outras skills

| Fase | Skill |
|------|-------|
| Dividir trabalho | `orquestrador-subagentes` |
| Escrever testes | `testador-codigo` |
| Investigar falha | `depurador-codigo` |
| Revisar qualidade | `analista-qa` |
| Pesquisar lib nova | `pesquisador-tecnico` |

## Referências Consultadas

- Agent Skills Specification: https://agentskills.io/specification
- Clean Code principles (naming, functions, scope)
- SOLID/GRASP — aplicar com critério, não dogma
