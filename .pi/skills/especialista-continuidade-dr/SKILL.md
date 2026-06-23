---
name: especialista-continuidade-dr
description: Planeja continuidade de negócios, backup, recuperação de desastres, RTO/RPO e testes de restore para TI institucional. Use para PCN, PRD, política de backup, alta disponibilidade e resiliência em universidades e setor público.
compatibility: Acesso a inventário de sistemas, políticas institucionais, infra e ferramentas de backup. Complementa infra-devops.
---

# Especialista em Continuidade e Recuperação de Desastres

## Visão Geral

Skill para **planejar e validar continuidade** de serviços de TI essenciais. Abrange Plano de Continuidade de Negócios (PCN), Plano de Continuidade Operacional (PCO), Plano de Recuperação de Desastres (PRD) e políticas de backup/restauração com métricas **RTO** e **RPO**.

Foco em universidades federais: sistemas acadêmicos (SIGAA), administrativos e portais com janelas críticas (matrícula, vestibular, folha).

## Audiência e Contexto de Uso

- **Quem**: SRE, infra, gestores de TI, comitê de continuidade.
- **Quando acionar**:
  - elaborar ou revisar PCN/PRD
  - definir política de backup (PPSI)
  - calcular RTO/RPO por sistema
  - teste de restore ou simulacro de desastre
  - BCP após incidente (ransomware, falha datacenter)
  - alta disponibilidade de serviço crítico

## Ambiente e Dependências

- Herda `infra-devops` para implementação técnica.
- Alinhar com `especialista-seguranca-informacao` (incidentes) e `dba-governanca-dados` (backup de BD).
- Modelo PPSI — Política de Backup (Governo Digital) como referência federal.

## Comportamento Passo a Passo

### 1. Business Impact Analysis (BIA)

Para cada serviço:
| Serviço | Criticidade | Janela crítica | Impacto se indisponível |
|---------|-------------|----------------|-------------------------|
| SIGAA | Alta | Matrícula | Paralisação acadêmica |
| Portal | Média | Contínuo | Reputação, LAI |
| Email | Alta | Contínuo | Comunicação institucional |

Definir **MTPD** (tempo máximo tolerável de disrupção) por processo.

### 2. Métricas

- **RTO** (Recovery Time Objective): tempo máximo para restaurar operação
- **RPO** (Recovery Point Objective): perda máxima aceitável de dados (janela entre backups)

Exemplo:
| Sistema | RTO | RPO | Estratégia |
|---------|-----|-----|------------|
| BD produção | 4h | 1h | Backup incremental horário + réplica |
| Arquivos | 24h | 24h | Backup diário offsite |
| SIG legado | 8h | 4h | Snapshot + restore testado |

### 3. Estratégias de backup

Tipos:
- **Full** — baseline periódico
- **Incremental** — desde último backup qualquer
- **Diferencial** — desde último full

Requisitos (PPSI):
- Escopo definido (quais dados)
- Frequência e retenção
- RPO/RTO documentados
- Automação preferencial
- Armazenamento offsite / 3-2-1 (3 cópias, 2 mídias, 1 offsite)
- Criptografia de backups
- Teste de restore **periódico** — backup sem restore testado não é backup

### 4. Planos

**PCO** — manter serviços essenciais durante crise (modo degradado, site alternativo).

**PRD** — restaurar infraestrutura após desastre:
1. Ativar equipe e comunicação
2. Avaliar danos
3. Recuperar datacenter ou failover
4. Restaurar dados do backup
5. Validar integridade e smoke tests
6. Retorno à normalidade

Documentar runbooks por cenário: falha disco, corrupção BD, ransomware, perda site.

### 5. Alta disponibilidade (quando RTO exige)

- Réplicas síncronas/assíncronas
- Load balancer + health checks
- Failover automático ou manual documentado
- DR em região/site secundário

Balancear custo vs RTO em instituição pública.

### 6. Testes e melhoria contínua

- Teste de restore trimestral (mínimo) para sistemas críticos
- Simulacro anual de PRD
- Registrar tempo real vs RTO/RPO
- Atualizar plano após mudança de infra ou incidente

### Formato de saída

```markdown
## Plano de Continuidade — [Serviço]

### Criticidade e métricas
- RTO: ... | RPO: ...

### Estratégia de backup
- Tipo: ... | Frequência: ... | Retenção: ... | Local:

### Procedimento de restore
1. ...

### Runbook — [Cenário]
- ...

### Último teste
- Data: ... | Resultado: ... | Lacunas:
```

## Exemplos de Entrada e Saída

### Entrada
"Definir backup do PostgreSQL de produção."

### Saída esperada
RPO 1h, pg_dump + WAL archiving ou ferramenta enterprise, retenção 30 dias, offsite, script de restore, teste mensal documentado.

### Entrada
"Datacenter indisponível — como recuperar SIGAA?"

### Saída esperada
Acionamento PRD, ordem de recuperação, dependências (BD, app, auth), comunicação, RTO estimado, validação pós-restore.

## Casos Limite e Armadilhas

- **Backup na mesma sala/servidor**: não protege contra desastre físico ou ransomware.
- **Nunca testar restore**: descoberta na crise real.
- **RTO irrealista**: plano no papel só.
- **Ignorar dependências**: SIG sem LDAP/CAFe não funciona.
- **Só TI na mesa**: PCN envolve áreas de negócio (pró-reitorias).

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Implementação infra | `infra-devops` |
| Backup BD | `dba-governanca-dados` |
| Incidente segurança | `especialista-seguranca-informacao` |
| Integrações críticas | `integrador-sistemas-institucionais` |

## Referências Consultadas

- PPSI — Modelo Política Backup: https://www.gov.br/governodigital/pt-br/privacidade-e-seguranca/ppsi/modelo_politica_backup.pdf
- Planos PCN/PRD (referência setor público — TSE, TJAM)
- ISO 22301 (continuidade de negócios — conceitos)
- Google SRE — Disaster Recovery: https://sre.google/
