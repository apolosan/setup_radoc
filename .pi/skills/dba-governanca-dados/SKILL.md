---
name: dba-governanca-dados
description: Modelagem, administração e governança de bancos de dados — schema, migrations, índices, performance, backup, qualidade e catálogo de dados. Use para desenho de modelo, otimização de queries, políticas de retenção e integridade em universidades e setor público.
compatibility: SGBD detectado em runtime (PostgreSQL, MySQL, Oracle, etc.). Acesso a shell, migrations e ferramentas de administração.
---

# DBA / Modelagem e Governança de Dados

## Visão Geral

Skill para **modelar, administrar e governar dados** persistidos em sistemas institucionais. Vai além de ORM: cobre modelagem conceitual/lógica, integridade, performance, ciclo de vida, qualidade e alinhamento com LGPD e missão institucional.

Complementa `desenvolvedor-backend` (código de acesso) com visão de **dados como ativo**.

## Audiência e Contexto de Uso

- **Quem**: DBAs, arquitetos de dados, desenvolvedores sênior.
- **Quando acionar**:
  - modelagem de novo domínio (alunos, protocolos, patrimônio)
  - revisão de schema e normalização
  - migrations complexas ou destrutivas
  - queries lentas, índices, planos de execução
  - política de retenção, arquivo, purga
  - catálogo de dados e linhagem
  - separação prod/homolog, mascaramento de dados

## Ambiente e Dependências

- Detectar SGBD, versão, convenções de naming do projeto.
- Integrar com `especialista-lgpd` (dados pessoais) e `especialista-continuidade-dr` (backup/RPO).
- Migrations destrutivas em produção exigem aprovação e plano de rollback.

## Comportamento Passo a Passo

### 1. Entender domínio (5W2H)

- Entidades, relacionamentos, cardinalidades
- Volume atual e projetado
- Padrões de leitura vs escrita
- Requisitos de auditoria e histórico
- Dados pessoais e sensíveis no modelo

### 2. Modelagem

Ordem:
1. **Conceitual** — entidades de negócio, sem tecnologia
2. **Lógico** — tabelas, PK/FK, normalização (3FN mínimo; desnormalizar com justificativa)
3. **Físico** — tipos, índices, partições, constraints

Princípios:
- Nomes claros e consistentes com o projeto
- Constraints no banco (NOT NULL, UNIQUE, FK, CHECK)
- Soft delete quando regra de negócio exigir histórico
- Tabelas de auditoria para operações críticas
- Evitar JSON genérico quando estrutura é estável

### 3. Migrations

- Versionadas e reversíveis quando possível
- Transacionais para DDL suportado
- Dados de migração em scripts separados
- Testar em cópia de produção ou snapshot
- Janela de manutenção para locks longos

### 4. Performance

1. `EXPLAIN` / plano de execução
2. Índices em FK e filtros frequentes — evitar excesso
3. N+1 na aplicação vs JOIN adequado
4. Particionamento para tabelas muito grandes (logs, histórico)
5. Connection pooling e limites de conexão
6. Vacuum/analyze (PostgreSQL) ou equivalentes

### 5. Governança de dados

Artefatos:
- **Catálogo** — dicionário de dados (tabela, coluna, dono, classificação)
- **Linhagem** — origem e destino de dados integrados
- **Qualidade** — regras (CPF válido, email único, domínios)
- **Retenção** — prazo legal e eliminação segura
- **Ambientes** — nunca dados reais de produção em dev sem mascaramento

### Formato de saída

```markdown
## Modelo de Dados — [Domínio]

### Diagrama ER
[mermaid erDiagram]

### Tabelas principais
| Tabela | Propósito | Dados pessoais? |

### Índices recomendados
- ...

### Migrations
- `YYYYMMDD_descricao.sql`

### Políticas
- Retenção: ...
- Backup RPO: (ver continuidade-dr)
```

## Exemplos de Entrada e Saída

### Entrada
"Modelar banco para controle de estágios obrigatórios."

### Saída esperada
ER com aluno, empresa, termo, supervisor, período; FKs; índices; campos LGPD; migration inicial.

### Entrada
"Listagem de protocolos está lenta com 2M registros."

### Saída esperada
Análise EXPLAIN, índice composto, paginação, arquivo de registros antigos, estimativa de ganho.

## Casos Limite e Armadilhas

- **Lógica só no app**: integridade frágil sem constraints.
- **Migration irreversível sem backup**: perda de dados.
- **Índice em tudo**: degrada escrita.
- **CLOB em tabela quente**: considerar separação.
- **PII em claro**: criptografia ou tokenização quando exigido.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Código ORM/API | `desenvolvedor-backend` |
| LGPD / retenção | `especialista-lgpd` |
| Backup/DR | `especialista-continuidade-dr` |
| Integrações | `integrador-sistemas-institucionais` |

## Referências Consultadas

- PostgreSQL Documentation: https://www.postgresql.org/docs/
- Martin Fowler — Evolutionary Database Design
- DAMA-DMBOK (governança de dados — conceitos)
- PPSI — Modelo Política Backup (Governo Digital): https://www.gov.br/governodigital/
