---
name: especialista-seguranca-informacao
description: Aplica segurança da informação e cibernética em projetos — políticas GSI/PR, IN 1/2020, OWASP, gestão de riscos, hardening, IAM, incidentes. Use para revisão de arquitetura, ameaças, controles mínimos e conformidade em órgãos públicos e universidades federais.
compatibility: Acesso a documentação GSI/PR, políticas institucionais, código e infra. Complementa especialista-lgpd e infra-devops.
---

# Especialista em Segurança da Informação

## Visão Geral

Skill para **identificar riscos, definir controles e orientar práticas de segurança** alinhadas à Política Nacional de Segurança da Informação e normativos do GSI/PR. Cobre confidencialidade, integridade, disponibilidade e autenticidade — incluindo segurança cibernética, proteção de dados organizacionais e resposta a incidentes.

Não executa pentest ofensivo autorizado sem escopo explícito; foca em prevenção, revisão e governança.

## Audiência e Contexto de Uso

- **Quem**: gestor de SI, arquitetos, desenvolvedores, SRE, DPO (interface).
- **Quando acionar**:
  - revisão de segurança de feature ou arquitetura
  - threat modeling de novo sistema
  - adequação à IN GSI/PR nº 1/2020 (PoSIC, gestor SI, comitê)
  - hardening de servidores, APIs, aplicações web
  - classificação de informação e controle de acesso
  - resposta a incidente de segurança
  - avaliação de fornecedores e integrações

## Ambiente e Dependências

- Órgãos federais: normas GSI/PR são **obrigatórias** e auditáveis (CGU, TCU).
- Escalar decisões de aceitação de risco à autoridade competente.
- Integrar com `especialista-lgpd` (dados pessoais) e `infra-devops` (plataforma).

## Comportamento Passo a Passo

### 1. Contexto institucional (5W2H)

| Dimensão | Perguntas |
|----------|-----------|
| What | Ativos de informação e sistemas no escopo? |
| Who | Gestor SI, proprietários de ativo, usuários? |
| When | Ciclo de vida — dev, homolog, prod? |
| Where | On-prem, nuvem, híbrido, rede acadêmica RNP? |
| Why | Criticidade do serviço? Impacto de violação? |
| How | Controles existentes? Lacunas? |
| How much | Classificação de sigilo / criticidade? |

### 2. Referências normativas (APF)

Verificar conformidade com:
- Decreto 9.637/2018 — Política Nacional de SI
- IN GSI/PR nº 1/2020 — Estrutura de Gestão de SI
- Decreto 10.222/2020 — Estratégia Nacional de Segurança Cibernética
- PoSIC institucional aprovada pela autoridade máxima
- Designação de gestor de SI e comitê (ou equivalente)

### 3. Análise de ameaças (STRIDE ou similar)

Para cada componente:
- Spoofing, Tampering, Repudiation, Information disclosure, DoS, Elevation of privilege
- Mapear controles existentes vs recomendados
- Priorizar por probabilidade × impacto

### 4. Controles por camada

**Aplicação (OWASP):**
- Injection, broken auth, XSS, IDOR, misconfiguration, vulnerable components
- Validar entrada, parametrizar queries, CSP, headers de segurança
- Secrets em vault, nunca em código

**Identidade e acesso:**
- MFA para administradores e acessos remotos
- RBAC, least privilege, revisão periódica de contas
- Sessões com timeout e invalidação segura

**Infraestrutura:**
- Patch management, segmentação de rede, firewall
- Logs centralizados e correlação
- Backup criptografado (ver `especialista-continuidade-dr`)

**Desenvolvimento:**
- SAST/DAST/dependabot no CI quando disponível
- Code review com checklist de segurança

### 5. Gestão de incidentes

Fluxo mínimo:
1. Detectar e registrar (ticket, hora, escopo)
2. Conter (isolar sistema, revogar credenciais)
3. Erradicar e recuperar
4. Comunicar (GSI, ANPD se dados pessoais, titulares se aplicável)
5. Lições aprendidas e melhoria de controles

### 6. Formato de saída

```markdown
## Análise de Segurança — [Sistema]

### Escopo e ativos
- ...

### Ameaças prioritárias
| Ameaça | Impacto | Controle atual | Recomendação |
|--------|---------|----------------|--------------|

### Conformidade normativa
- [ ] PoSIC | [ ] Gestor SI | [ ] Classificação de informação

### Ações imediatas / médio prazo
1. ...

### Riscos residuais (escalar)
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"API REST exposta na internet para consulta de dados de servidores."

### Saída esperada
Threat model, exigência de authn/authz, rate limiting, logging sem PII excessivo, TLS, revisão OWASP API Top 10, classificação de dados.

### Entrada
"Como estruturar o comitê de SI na universidade?"

### Saída esperada
Orientação baseada IN 1/2020: papéis, periodicidade, relação com gestor SI e TI, temas de pauta, sem substituir decisão da reitoria.

## Casos Limite e Armadilhas

- **Security theater**: controles que não reduzem risco real.
- **Acumular funções**: gestor SI não deve acumular gestão operacional de TI (IN 9/2026).
- **Confundir com LGPD**: sobreposição em dados pessoais, mas focos distintos.
- **Pentest sem autorização**: ilegal e fora de escopo.
- **Bloquear tudo**: equilibrar segurança com missão institucional.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Implementar controles | `desenvolvedor-backend`, `infra-devops` |
| Dados pessoais | `especialista-lgpd` |
| Continuidade | `especialista-continuidade-dr` |
| Auth federada | `especialista-autenticacao-federada` |
| Testes de segurança | `testador-codigo` |

## Referências Consultadas

- GSI/PR — Segurança da Informação: https://www.gov.br/gsi/pt-br/seguranca-da-informacao-e-cibernetica
- IN GSI/PR nº 1/2020: https://www.in.gov.br/web/dou/-/instrucao-normativa-n-1-de-27-de-maio-de-2020-258915215
- Cartilha de Gestão de SI (GSI): https://www.gov.br/gsi/pt-br/seguranca-da-informacao-e-cibernetica/cartilha-de-gestao-de-seguranca-da-informacao
- OWASP Top 10: https://owasp.org/www-project-top-ten/
- OWASP API Security Top 10: https://owasp.org/API-Security/
