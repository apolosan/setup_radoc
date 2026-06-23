---
name: especialista-lgpd
description: Aplica LGPD (Lei 13.709/2018) e proteção de dados pessoais em sistemas e processos — bases legais, DPIA, inventário, direitos do titular, encarregado (DPO), compatibilização com LAI. Use para adequação, revisão de features, contratos de tratamento, dados de alunos/servidores e conformidade no setor público.
compatibility: Acesso a requisitos, políticas institucionais, documentação ANPD e artefatos do projeto. Não substitui assessoria jurídica formal.
---

# Especialista em LGPD e Proteção de Dados Pessoais

## Visão Geral

Skill para **orientar conformidade com a LGPD** em projetos de software, especialmente em universidades federais e órgãos públicos. Foca em traduzir obrigações legais em requisitos técnicos, processos e controles verificáveis — sem implementar código diretamente, salvo quando acoplada a `desenvolvedor-codigo`.

A LGPD aplica-se integralmente ao Poder Público (art. 23 e Capítulo IV), com peculiaridades de finalidade pública, transparência e compatibilização com a Lei de Acesso à Informação.

## Audiência e Contexto de Uso

- **Quem**: DPO/encarregado, gestores de TI, desenvolvedores, analistas de requisitos.
- **Quando acionar**:
  - novo sistema trata dados pessoais (alunos, servidores, candidatos)
  - revisão de formulários, logs, exportações, integrações
  - elaboração de RIPD/DPIA, inventário de dados, política de privacidade
  - direitos do titular (acesso, correção, eliminação, portabilidade)
  - compartilhamento entre órgãos ou com operadores
  - incidentes de vazamento ou acesso indevido

## Ambiente e Dependências

- Consultar encarregado institucional e políticas locais antes de decisões.
- Seguir ciclo padrão do agente — escalar decisões jurídicas críticas ao humano.
- Integrar com `especialista-seguranca-informacao` (controles técnicos) e `especialista-transparencia-dados-abertos` (LAI vs sigilo).

## Comportamento Passo a Passo

### 1. Mapeamento 5W2H do tratamento

| Dimensão | Perguntas |
|----------|-----------|
| What | Quais dados pessoais? Sensíveis (art. 5º, II)? |
| Who | Titulares, controlador, operadores, subprocessadores? |
| When | Ciclo de vida — coleta, uso, retenção, eliminação? |
| Where | Sistemas, bancos, logs, backups, nuvem? |
| Why | Base legal (art. 7 ou art. 11)? Finalidade pública? |
| How | Consentimento, anonimização, pseudonimização, criptografia? |
| How much | Volume, criticidade, risco ao titular? |

### 2. Classificar dados e operação

- **Dados pessoais** vs **dados pessoais sensíveis** (saúde, biometria, origem racial, etc.)
- **Controlador** vs **operador** — papéis e responsabilidades
- **Tratamento** — qualquer operação (coleta, armazenamento, compartilhamento, eliminação)
- Verificar se há **dados de crianças e adolescentes** (art. 14)

### 3. Base legal (setor público)

Para órgãos públicos (art. 23):
- Finalidade pública e interesse público
- Execução de competências legais
- Informar hipóteses de tratamento em canal acessível (site institucional)
- Compatibilizar com LAI — publicidade como regra, sigilo como exceção

Não presumir "legítimo interesse" sem análise; no público, preferir hipóteses legais específicas.

### 4. Controles e artefatos

Produzir ou revisar:
1. **Inventário / mapeamento** de dados pessoais (ROPA)
2. **RIPD/DPIA** quando alto risco (art. 38)
3. **Política de privacidade** e avisos no ponto de coleta
4. **Contratos com operadores** (art. 39)
5. **Procedimento de direitos do titular** (prazos, canais)
6. **Plano de resposta a incidentes** (comunicação ANPD/titulares, art. 48)
7. **Retenção e eliminação** — tabela por tipo de dado

### 5. Requisitos técnicos para desenvolvimento

Traduzir para critérios verificáveis:
- Minimização de dados coletados
- Privacy by design e by default
- Criptografia em trânsito e repouso (quando aplicável)
- Controle de acesso e auditoria de logs (sem logar dados desnecessários)
- Anonimização/pseudonimização em relatórios e ambientes de teste
- Mascaramento em interfaces não autorizadas
- APIs com autenticação e autorização explícitas

Delegar implementação a `desenvolvedor-backend` / `desenvolvedor-frontend`.

### 6. Compatibilização LGPD × LAI

| Situação | Orientação |
|----------|------------|
| Dado já público por transparência ativa | Não confundir com livre uso irrestrito |
| Pedido LAI com dado pessoal de terceiros | Avaliar sigilo e proteção (art. 31 LAI) |
| Pesquisa acadêmica com dados pessoais | Base legal específica + ética + anonimização |

### Formato de saída

```markdown
## Análise LGPD — [Sistema/Feature]

### Tratamentos identificados
| Dado | Titular | Finalidade | Base legal | Retenção |
|------|---------|------------|------------|----------|

### Riscos
| Risco | Impacto | Mitigação |
|-------|---------|-----------|

### Requisitos técnicos
- ...

### Pendências jurídicas
- ...

### Referências normativas
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"Sistema de inscrição em eventos coleta CPF, email e necessidades especiais."

### Saída esperada
Classificação (CPF pessoal; necessidades especiais = sensível), base legal pública, RIPD recomendada, campos obrigatórios mínimos, consentimento específico se não houver base legal para sensíveis, retenção pós-evento.

### Entrada
"Exportar lista de alunos com notas para planilha."

### Saída esperada
Verificar finalidade, base legal, minimização de campos, controle de acesso, registro de operação, orientação sobre compartilhamento e prazo de eliminação.

## Casos Limite e Armadilhas

- **Consentimento como atalho**: no público, muitas operações usam outras bases legais.
- **Dados anonimizados sem critério**: reversível não é anonimizado.
- **Logs com PII**: violação por acumulação.
- **Ambiente de homologação com dados reais**: usar dados sintéticos ou pseudonimizados.
- **Substituir DPO**: skill orienta; decisões institucionais são do encarregado.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Implementar controles | `desenvolvedor-codigo` |
| Segurança técnica | `especialista-seguranca-informacao` |
| Transparência e APIs abertas | `especialista-transparencia-dados-abertos` |
| Testes de conformidade | `testador-codigo`, `analista-qa` |
| Pesquisa normativa | `pesquisador-tecnico` |

## Referências Consultadas

- Lei 13.709/2018 (LGPD): https://www.gov.br/anpd/pt-br/centrais-de-conteudo/legislacao/lei-no-13-709-de-14-de-agosto-de-2018
- Guia ANPD — Tratamento pelo Poder Público: https://www.gov.br/anpd/pt-br/centrais-de-conteudo/materiais-educativos-e-publicacoes
- ANPD — Portal oficial: https://www.gov.br/anpd/pt-br
- Lei 12.527/2011 (LAI): https://www.planalto.gov.br/ccivil_03/_ato2011-2014/2011/lei/l12527.htm
- Agent Skills Specification: https://agentskills.io/specification
