---
name: desenvolvedor-devops
description: Desenvolve automação DevOps de aplicação — CI/CD pipelines, Dockerfiles, scripts de build/deploy, health checks e configuração de runtime. Use para GitHub Actions, GitLab CI, containers, release automation e smoke tests pós-deploy.
compatibility: Acesso a repositório, shell e plataformas CI/CD configuradas. Diferente de infra-devops (foco em plataforma/nuvem).
---

# Desenvolvedor DevOps

## Visão Geral

Sub-skill de `desenvolvedor-codigo` focada em **automação do ciclo de vida da aplicação**: build, test, package, deploy e verificação. Distinto de `infra-devops`, que trata plataforma, rede e recursos cloud em escala maior.

| Aspecto | desenvolvedor-devops | infra-devops |
|---------|---------------------|--------------|
| Foco | Pipeline da app, container, release | Cluster, rede, IAM, observabilidade plataforma |
| Artefatos | Dockerfile, workflow YAML, scripts | Terraform, K8s manifests, policies |
| Dono típico | time de produto | time de plataforma/SRE |

## Audiência e Contexto de Uso

- **Quem**: desenvolvedores com ownership de delivery, platform engineers de app.
- **Quando acionar**:
  - criar/ajustar pipeline CI/CD
  - Dockerfile e docker-compose
  - scripts de build e release
  - gates de qualidade no CI (test, lint, scan)
  - deploy automatizado para staging
  - smoke tests pós-deploy

## Ambiente e Dependências

- Herda `desenvolvedor-codigo` e ciclo padrão do agente.
- Detectar: CI existente, registry de imagens, estratégia de deploy.
- Deploy em produção exige aprovação humana.

## Comportamento Passo a Passo

### 1. Mapear pipeline atual

- Onde roda CI? (GitHub Actions, GitLab, Jenkins)
- Stages existentes: lint → test → build → deploy?
- Como secrets são injetados?
- Ambientes: dev, staging, prod

### 2. Princípios de pipeline

- **Fast feedback**: jobs paralelos, cache de deps
- **Fail fast**: lint antes de testes longos
- **Reprodutibilidade**: versões pinadas, lockfiles
- **Least privilege**: tokens com escopo mínimo
- **Idempotência**: re-run seguro

### 3. Implementação típica

**CI workflow:**
```yaml
# Estrutura conceitual
on: [push, pull_request]
jobs:
  lint: ...
  test: ...  # paralelo
  build: ... # needs: [lint, test]
  deploy-staging: ... # needs: build, branch main
```

**Dockerfile:**
- Multi-stage build (builder + runtime slim)
- Non-root user
- .dockerignore adequado
- Health check quando aplicável

**Scripts:**
- `build.sh`, `deploy.sh` com flags explícitas
- Exit codes corretos para CI

### 4. Quality gates

Incluir no CI quando existir no projeto:
- Unit/integration tests
- Linter e type check
- Security scan (dependências, container)
- Coverage threshold se definido

### 5. Deploy e verificação

1. Deploy para ambiente não-prod primeiro
2. Smoke test automatizado (health endpoint, critical path)
3. Rollback documentado se smoke falhar
4. Registrar evidência em `log de eventos do ciclo`

### Formato de saída

```markdown
## DevOps — [Feature]

### Pipeline
- Trigger: ...
- Jobs: ...

### Arquivos
- `.github/workflows/...`
- `Dockerfile`

### Secrets necessários
- (nomes apenas, nunca valores)

### Smoke test
- ...

### Rollback
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"Adicione CI que roda testes em cada PR."

### Saída esperada
Workflow com cache, matrix se múltiplas versões, status check obrigatório.

### Entrada
"Containerize a API para deploy."

### Saída esperada
Dockerfile multi-stage, compose opcional para dev, healthcheck, documentação de env vars.

## Casos Limite e Armadilhas

- **Secrets no YAML**: usar secret store do CI.
- **Deploy prod automático sem gate**: exigir aprovação manual.
- **Imagem latest em prod**: pin por digest ou tag semver.
- **CI lento**: paralelizar; não rodar E2E em todo commit se caro.
- **Confundir com infra**: K8s cluster/terraform → `infra-devops`.

## Referências Consultadas

- GitHub Actions Documentation: https://docs.github.com/en/actions
- Docker Best Practices: https://docs.docker.com/develop/dev-best-practices/
- The Twelve-Factor App: https://12factor.net/
- Skill pai: `desenvolvedor-codigo`
- DORA metrics (deployment frequency, lead time)
