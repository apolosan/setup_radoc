---
name: desenvolvedor-backend
description: Desenvolve serviços backend — APIs, domínio, persistência, autenticação, filas e integrações. Use para endpoints REST/GraphQL/gRPC, regras de negócio server-side, ORM, migrations e microsserviços.
compatibility: Linguagem e framework backend detectados em runtime. Acesso a DB, shell e testes de integração.
---

# Desenvolvedor Backend

## Visão Geral

Sub-skill de `desenvolvedor-codigo` focada na **camada de servidor**: expor contratos, aplicar regras de negócio, persistir dados e integrar sistemas externos com confiabilidade e segurança.

## Audiência e Contexto de Uso

- **Quem**: desenvolvedores backend, agentes de API.
- **Quando acionar**:
  - endpoints, controllers, handlers
  - modelagem de domínio e serviços
  - banco de dados, migrations, queries
  - autenticação/autorização
  - filas, jobs, webhooks
  - integrações com terceiros

## Ambiente e Dependências

- Herda `desenvolvedor-codigo` e ciclo padrão do agente.
- Detectar: linguagem, framework web, ORM, padrão arquitetural (layered, hexagonal, etc.).
- Nunca commitar secrets; usar env vars.

## Comportamento Passo a Passo

### 1. Reconhecimento do stack

- Framework HTTP (Express, FastAPI, Spring, etc.)
- Padrão de camadas (controller → service → repository)
- ORM/query builder e estratégia de migrations
- Auth (JWT, session, OAuth)
- Formato de erros e logging existente

### 2. Modelagem e contrato

Antes de codificar:
1. Definir **operação de sistema** (entrada, saída, erros)
2. Especificar **pré/pós-condições** quando complexo
3. Identificar entidades e invariantes
4. Documentar contrato (OpenAPI, protobuf, comentários)

### 3. Implementação por camadas

Ordem típica:
1. **Domínio** — entidades, value objects, regras puras
2. **Aplicação** — casos de uso, orquestração
3. **Infraestrutura** — DB, HTTP clients, filas
4. **Interface** — controllers, DTOs, validação de entrada
5. **Migrations** — schema changes reversíveis quando possível

Princípios:
- Validar entrada na borda (fail fast)
- Regras de negócio no domínio, não no controller
- Transações onde consistência exige
- Idempotência em operações críticas
- Paginação em listagens
- Rate limiting quando exposto publicamente

### 4. Segurança

- Autenticação e autorização em cada endpoint protegido
- Sanitizar inputs (SQL injection, XSS em respostas)
- Não logar dados sensíveis
- Princípio do menor privilégio em credenciais DB
- Escalar decisões destrutivas para humano

### 5. Validação

1. Testes unitários de domínio
2. Testes de integração com DB (test container ou in-memory)
3. Testes de contrato de API
4. Linter e type checker

### Formato de saída

```markdown
## Backend — [Feature]

### Endpoints / Operações
| Método | Path | Descrição |
|--------|------|-----------|

### Modelo / Schema
- ...

### Arquivos
- ...

### Segurança
- ...

### Testes
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"CRUD de categorias com soft delete."

### Saída esperada
Endpoints REST, migration com deleted_at, testes de integração, validação de nome único.

### Entrada
"Webhook para processar pagamento confirmado."

### Saída esperada
Handler idempotente, verificação de assinatura, atualização transacional de pedido, retry policy.

## Casos Limite e Armadilhas

- **Anemic domain model**: lógica só no service sem entidades ricas.
- **N+1 queries**: eager load ou batch quando listar.
- **God controller**: controller fino, service com regras.
- **Migration destrutiva** sem backup/plano de rollback.
- **Breaking API** sem versionamento ou deprecação.

## Referências Consultadas

- OWASP API Security Top 10: https://owasp.org/API-Security/
- Richardson Maturity Model (REST)
- Hexagonal Architecture (Ports & Adapters)
- Skill pai: `desenvolvedor-codigo`

