---
name: infra-devops
description: Opera infraestrutura e plataforma — cloud, Kubernetes, Terraform, rede, IAM, observabilidade, capacidade e incidentes de infra. Use para provisionar recursos, configurar clusters, políticas de segurança e SRE. Diferente de desenvolvedor-devops (CI/CD da aplicação).
compatibility: Acesso a cloud CLI, Terraform/Pulumi, kubectl e dashboards de observabilidade. Credenciais via env seguro.
---

# Infra / DevOps (Plataforma)

## Visão Geral

Skill para **infraestrutura e operação de plataforma** — provisionar, configurar, monitorar e manter ambientes de execução. Foco em confiabilidade, segurança, custo e observabilidade em escala de plataforma.

| Aspecto | infra-devops | desenvolvedor-devops |
|---------|--------------|---------------------|
| Foco | Cloud, rede, cluster, IAM | Pipeline e container da app |
| Ferramentas | Terraform, K8s, CloudFormation | GitHub Actions, Dockerfile |
| Escopo | Plataforma compartilhada | Entrega de uma aplicação |

## Audiência e Contexto de Uso

- **Quem**: SRE, platform engineers, DevOps de infraestrutura.
- **Quando acionar**:
  - provisionar recursos cloud
  - configurar Kubernetes/ECS/Lambda
  - Terraform/IaC
  - rede (VPC, LB, DNS, TLS)
  - IAM, policies, secrets management
  - observabilidade (métricas, logs, traces, alertas)
  - capacity planning e custos
  - incidentes de infraestrutura

## Ambiente e Dependências

- Seguir ciclo padrão do agente — ações destrutivas exigem aprovação humana.
- Nunca expor credenciais; usar vault/secret manager.
- Estado Terraform remoto e locking quando existir.
- Documentar rollback antes de mudanças em prod.

## Comportamento Passo a Passo

### 1. Descoberta de contexto

Mapear:
- Cloud provider e regiões
- IaC existente (módulos, workspaces)
- Topologia (VPC, subnets, clusters)
- Identity (IAM roles, service accounts)
- Observabilidade (Prometheus, Grafana, CloudWatch, etc.)
- SLAs/SLOs se definidos

### 2. Princípios de infraestrutura

- **Infrastructure as Code** — tudo versionado, revisável
- **Immutability** — replace over patch quando seguro
- **Least privilege** — IAM mínimo por serviço
- **Defense in depth** — rede, auth, encryption
- **Observability by default** — métricas, logs estruturados, traces
- **Cost awareness** — tags, rightsizing, alertas de custo

### 3. Fluxo de mudança

```
Planejar → Review (terraform plan) → Aprovação → Apply staging → Validar → Apply prod
```

Para cada mudança:
1. **Impact analysis** — o que pode quebrar?
2. **Plan** — `terraform plan`, `kubectl diff`
3. **Rollback** — como reverter?
4. **Validação** — health checks, smoke, métricas
5. **Registro** — `log de eventos do ciclo`, changelog

### 4. Áreas de atuação

**Rede:**
- VPC, subnets públicas/privadas, NAT, security groups
- Load balancers, ingress, certificados TLS

**Compute:**
- K8s deployments, HPA, node pools
- Serverless config e limites

**Dados:**
- RDS, backups, retention, encryption at rest
- Redis, filas gerenciadas

**Segurança:**
- IAM policies, RBAC K8s
- Secrets rotation
- WAF, DDoS protection quando aplicável

**Observabilidade:**
- Dashboards de golden signals (latency, traffic, errors, saturation)
- Alertas acionáveis com runbooks
- Log aggregation e retention

### 5. Gestão de incidentes

1. **Triagem** — sintoma vs infra vs app
2. **Mitigar** — scale, rollback, failover
3. **Diagnosticar** — métricas, logs, traces correlacionados
4. **Resolver** — fix permanente via IaC
5. **Post-mortem** — blameless, action items

### Formato de saída

```markdown
## Infra — [Mudança]

### Objetivo
...

### Recursos afetados
- ...

### IaC
- Arquivos: ...
- Plan summary: ...

### Segurança
- IAM changes: ...
- Secrets: (nomes apenas)

### Validação
- [ ] Staging OK
- [ ] Métricas estáveis
- [ ] Rollback testado

### Aprovação necessária
- [ ] Produção — aguardando humano
```

## Exemplos de Entrada e Saída

### Entrada
"Provisione cluster EKS para staging."

### Saída esperada
Módulos Terraform, node group, IAM roles, kubectl config, documentação de acesso.

### Entrada
"API está lenta — investigar infra."

### Saída esperada
Análise de CPU/memória, throttling, conexões DB, latência de rede; recomendação com evidências.

## Casos Limite e Armadilhas

- **ClickOps**: mudança manual sem IaC.
- **Apply prod sem plan review**: sempre mostrar diff.
- **Over-permissive IAM**: `*` em policies.
- **Sem monitoramento**: voar às cegas.
- **Confundir com pipeline**: CI da app → `desenvolvedor-devops`.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Pipeline de deploy da app | `desenvolvedor-devops` |
| Bug de aplicação | `depurador-codigo` |
| Orquestrar mudanças paralelas | `orquestrador-subagentes` |
| Planejamento de capacidade | `gerente-projetos-ti` |

## Referências Consultadas

- Google SRE Book: https://sre.google/sre-book/table-of-contents/
- Terraform Best Practices: https://www.terraform.io/docs/cloud/guides/recommended-practices/
- AWS Well-Architected Framework: https://aws.amazon.com/architecture/well-architected/
- Kubernetes Documentation: https://kubernetes.io/docs/
- The Twelve-Factor App: https://12factor.net/

