---
name: integrador-sistemas-institucionais
description: Integra sistemas de universidades federais — SIGAA, SIPAC, SUAP, SEI, Moodle, GLPI, SIAPE/SIAFI — via web services, e-PING e padrões institucionais. Use para interoperabilidade, sincronização de dados, APIs e fluxos administrativos/acadêmicos.
compatibility: Acesso a documentação local do sistema, credenciais de homologação, rede institucional. Stack detectada em runtime; APIs variam por campus.
---

# Integrador de Sistemas Institucionais

## Visão Geral

Skill para **planejar e implementar integrações** entre sistemas usados em universidades federais brasileiras. O ecossistema típico inclui SIG (SIGAA acadêmico, SIPAC administrativo, SIGRH/SIGPRH pessoal), SUAP (IFs), SEI (processos), Moodle (EAD), GLPI (chamados) e sistemas estruturantes do Governo Federal (SIAFI, SIASG, SIAPE, SCDP, ComprasNet).

Integrações seguem frequentemente **web services** e padrões **e-PING** (interoperabilidade de governo eletrônico). Não há API pública unificada nacional — cada instituição pode customizar.

## Audiência e Contexto de Uso

- **Quem**: desenvolvedores, analistas de integração, equipe de TI institucional.
- **Quando acionar**:
  - sincronizar dados aluno/servidor entre sistemas
  - expor ou consumir API de sistema legado
  - integrar novo sistema com SIGAA/SIPAC/SUAP
  - tramitação SEI automatizada
  - matrícula, notas, patrimônio, compras, RH
  - interoperabilidade com sistemas federais (SCDP, SIAFI)

## Ambiente e Dependências

- Herda `desenvolvedor-backend` para implementação.
- Obter documentação e ambiente de **homologação** da própria instituição.
- Respeitar `especialista-lgpd` (dados pessoais) e `especialista-seguranca-informacao`.
- Credenciais e certificados via canais oficiais — nunca hardcoded.

## Comportamento Passo a Passo

### 1. Inventário de sistemas

| Sistema | Domínio | Dados típicos | Integração comum |
|---------|---------|---------------|------------------|
| SIGAA | Acadêmico | Alunos, turmas, notas, matrículas | Web services, views, jobs |
| SIPAC | Admin/Patrimônio | Compras, almoxarifado, patrimônio | WS, SIAFI/SIASG |
| SUAP | Acadêmico+Admin (IFs) | API REST documentada por campus | REST |
| SEI | Processos | Tramitação documental | Web services SEI |
| Moodle | EAD | Usuários, cursos, notas | LTI, web services Moodle |
| GLPI | Service desk | Chamados, inventário | API GLPI |
| SIAPE/SIAFI | Federal | Servidores, financeiro | Conectores estruturantes |

Confirmar **qual sistema e versão** a instituição usa — SIGAA/UFRN é referência comum mas há forks.

### 2. Análise de integração (5W2H)

- **Fonte da verdade** — qual sistema é master para cada entidade?
- **Direção** — unidirecional vs bidirecional
- **Frequência** — tempo real, batch noturno, evento
- **Volume e janela** — picos (matrícula, rematrícula)
- **Idempotência** — reprocessamento seguro
- **Contrato** — campos, formatos, códigos (CPF, matrícula, SIAPE)

### 3. Padrões técnicos

**e-PING / interoperabilidade:**
- Preferir padrões abertos (XML, JSON, REST, SOAP quando legado)
- UTF-8, HTTPS, autenticação documentada
- Catálogo de interoperabilidade quando aplicável

**Padrões de integração:**
- **API REST/JSON** — preferido para novos sistemas
- **Web services SOAP** — comum em SIG legado
- **ETL/batch** — grandes volumes, baixa latência aceitável
- **Message queue** — desacoplamento e resiliência
- **Evitar scraping** em produção — frágil e sem suporte; usar APIs oficiais

### 4. Implementação segura

1. Autenticação (token, certificado, IP allowlist)
2. Mapeamento de entidades e tratamento de inconsistências
3. Log de integração (sem PII desnecessário)
4. Dead letter / fila de erros para reprocessamento
5. Monitoramento e alertas de falha
6. Testes em homologação com dados sintéticos

### 5. Documentação de entrega

```markdown
## Integração — [Sistema A] ↔ [Sistema B]

### Fluxo
[mermaid sequenceDiagram]

### Mapeamento de campos
| Origem | Destino | Transformação |

### Autenticação
- ...

### Tratamento de erros
- ...

### SLAs e janelas
- ...

### Dependências institucionais
- Contato TI, credenciais, firewall
```

## Exemplos de Entrada e Saída

### Entrada
"Sincronizar usuários do SIGAA com Moodle no início do semestre."

### Saída esperada
Fluxo batch, mapeamento matrícula/email, criação/suspensão de contas, web services Moodle, cron, rollback, testes em homologação.

### Entrada
"Consultar status de processo no SEI a partir do portal interno."

### Saída esperada
Análise API SEI, autenticação, polling vs webhook, cache, permissões por unidade.

## Casos Limite e Armadilhas

- **Assumir API igual em todas as federais**: validar com TI local.
- **Bidirecional sem master**: conflitos de dados.
- **Integração em pico de matrícula**: planejar carga e filas.
- **Dados pessoais em log**: viola LGPD.
- **Versão desatualizada do SIG**: contratos podem diferir.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Código da integração | `desenvolvedor-backend` |
| Auth SSO | `especialista-autenticacao-federada` |
| Modelagem de dados | `dba-governanca-dados` |
| Pesquisa de padrões | `pesquisador-tecnico` |
| Testes | `testador-codigo` |

## Referências Consultadas

- e-PING — Padrões de Interoperabilidade: http://www.eping.e.gov.br/
- SIGAA (referência UFRN): documentação institucional local
- SUAP — documentação por IF: repositórios oficiais
- SEI — Módulo de Integração: documentação do TRF
- Moodle Web Services: https://docs.moodle.org/dev/Web_services
- Interoperabilidade InfraSIG (exemplo UFRN): manuais de integração SIPAC/SIAFI
