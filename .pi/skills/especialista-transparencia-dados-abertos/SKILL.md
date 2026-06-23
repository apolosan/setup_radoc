---
name: especialista-transparencia-dados-abertos
description: Implementa transparência ativa, LAI (Lei 12.527/2011) e dados abertos em portais públicos — publicação proativa, formatos abertos, APIs, Fala.BR e Portal da Transparência. Use para conformidade, catalogação de datasets e balanceamento com LGPD.
compatibility: Acesso a portais institucionais, políticas de dados abertos e documentação CGU. Stack-agnóstico.
---

# Especialista em Transparência e Dados Abertos

## Visão Geral

Skill para **garantir transparência pública** em sistemas e portais de universidades federais e órgãos públicos. A LAI estabelece publicidade como regra e sigilo como exceção; exige transparência ativa (divulgação proativa) e transparência passiva (pedidos via Fala.BR/SIC).

Dados abertos (formatos estruturados, legíveis por máquina) complementam a LAI e permitem reuso e controle social.

## Audiência e Contexto de Uso

- **Quem**: gestores de portal, desenvolvedores, assessoria de comunicação, ouvidoria.
- **Quando acionar**:
  - seção "Acesso à Informação" no site institucional
  - publicação de datasets (orçamento, folha, contratos, editais)
  - API de dados abertos
  - balancear transparência com proteção de dados pessoais
  - integração com Portal da Transparência / dados.gov.br
  - atendimento a pedidos LAI com componente técnico

## Ambiente e Dependências

- Integrar com `especialista-lgpd` — dado pessoal não vai para aberto sem base legal e anonimização.
- Decreto 7.724/2012 e Guia de Transparência Ativa (GAT/CGU).
- Política de Dados Abertos do Governo Federal (2016+).

## Comportamento Passo a Passo

### 1. Princípios LAI

- **Transparência ativa** — publicar sem pedido prévio
- **Transparência passiva** — responder pedidos em até 20 dias (+10 prorrogáveis)
- Linguagem clara, formatos eletrônicos abertos quando possível
- Ferramenta de pesquisa no site
- Acesso automatizado por sistemas externos (API)

### 2. Itens de transparência ativa (referência federal)

Verificar lista atualizada no GAT/CGU; tipicamente inclui:
- Estrutura organizacional e competências
- Repasses e transferências
- Despesas e receitas
- Licitações e contratos
- Servidores (quando legalmente publicável)
- Obras e metas
- Perguntas frequentes e canal SIC

Universidades: adaptar ao regimento e órgãos de controle (TCU).

### 3. Dados abertos

Critérios (5 estrelas de Tim Berners-Lee como referência):
1. Disponível na web
2. Formato estruturado (CSV, JSON, XML)
3. Formato não proprietário
4. URIs estáveis para identificação
5. Linkagem entre datasets

**Formatos preferidos:** CSV, JSON, XML, ODS — evitar PDF só para dados tabulares.

Metadados DCAT-AP ou perfil institucional; catalogar no Portal Brasileiro de Dados Abertos quando aplicável.

### 4. APIs de transparência

Requisitos LAI (art. 8º, §3º):
- Acesso automatizado
- Formatos abertos e legíveis por máquina
- Autenticidade e integridade
- Documentação pública (OpenAPI)

Práticas:
- Versionamento de API
- Rate limiting
- Sem dados pessoais identificáveis em endpoints abertos
- Changelog de datasets

### 5. LGPD × LAI

| Cenário | Ação |
|---------|------|
| Dado já público por lei | Publicar conforme norma específica |
| Dado pessoal em pedido LAI | Avaliar sigilo (art. 31 LAI) e anonimizar agregados |
| Dataset estatístico | Agregar para k-anonimidade mínima |
| Servidor — remuneração | Verificar o que é transparência obrigatória vs redação |

Escalar conflitos ao jurídico/encarregado LGPD.

### 6. Fala.BR e SIC

- Sistema eletrônico federal para pedidos LAI
- Portal deve linkar SIC e instruções
- Prazos e recursos documentados
- Métricas no Painel LAI (CGU)

### Formato de saída

```markdown
## Plano de Transparência — [Órgão/Sistema]

### Lacunas de transparência ativa
| Item GAT | Status | Ação | Responsável |

### Datasets propostos
| Nome | Formato | Periodicidade | PII |

### API
- Endpoints: ...
- Documentação: ...

### Conformidade LGPD
- ...

### Próximos passos
1. ...
```

## Exemplos de Entrada e Saída

### Entrada
"Publicar dados de bolsas de pesquisa em formato aberto."

### Saída esperada
Schema CSV, campos permitidos, agregação se necessário, periodicidade, página no portal, metadados, revisão LGPD.

### Entrada
"Cidadão pediu via LAI lista de emails de alunos."

### Saída esperada
Análise de sigilo e LGPD, provável negativa parcial ou anonimização, modelo de resposta, fundamento legal, canal de recurso.

## Casos Limite e Armadilhas

- **Publicar PII "porque é público"**: erro grave sem base legal.
- **PDF escaneado**: não é dado aberto útil.
- **Dataset sem atualização**: transparência defasada.
- **API sem documentação**: barreira ao reuso.
- **Ignorar Fala.BR**: descumprimento de prazo.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Portal/API | `desenvolvedor-backend`, `desenvolvedor-frontend` |
| LGPD | `especialista-lgpd` |
| Acessibilidade do portal | `especialista-acessibilidade-digital` |
| Modelagem de publicação | `dba-governanca-dados` |

## Referências Consultadas

- Lei 12.527/2011 (LAI): https://www.planalto.gov.br/ccivil_03/_ato2011-2014/2011/lei/l12527.htm
- Acesso à Informação — Governo: https://www.gov.br/acessoainformacao/pt-br/
- WikiLAI: https://wikilai.fiquemsabendo.com.br/
- Portal da Transparência: https://portaldatransparencia.gov.br/
- dados.gov.br: https://dados.gov.br/
- CGU — Guia de Transparência Ativa (GAT)
