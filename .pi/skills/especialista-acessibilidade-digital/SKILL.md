---
name: especialista-acessibilidade-digital
description: Garante acessibilidade digital conforme e-MAG, WCAG 2.x e ABNT NBR 17225 em portais e aplicações de órgãos públicos. Use para auditoria, correção, checklist, componentes acessíveis, teclado, leitores de tela e conformidade em universidades federais.
compatibility: Acesso a HTML/componentes, ferramentas de auditoria (axe, Lighthouse), navegador. Complementa desenvolvedor-frontend.
---

# Especialista em Acessibilidade Digital (e-MAG / WCAG)

## Visão Geral

Skill para **auditar, especificar e validar acessibilidade** em conteúdos e aplicações web, com foco no setor público brasileiro. O e-MAG é obrigatório para órgãos do SISP; fundamenta-se na WCAG e foi evoluído pela ABNT NBR 17225/2025 (96 requisitos obrigatórios níveis A/AA).

Diferente de `desenvolvedor-frontend`: esta skill prioriza **conformidade normativa, auditoria e critérios de aceite a11y** antes da estética.

## Audiência e Contexto de Uso

- **Quem**: desenvolvedores, designers, QA, gestores de portais institucionais.
- **Quando acionar**:
  - novo portal, sistema ou feature de interface pública
  - auditoria e-MAG / WCAG / NBR 17225
  - correção de barreiras reportadas por usuários ou Ouvidoria
  - validação de componentes (formulários, tabelas, modais, CAPTCHA)
  - checklist pré-release de site institucional

## Ambiente e Dependências

- Herda contexto de `desenvolvedor-frontend` para correções de código.
- Ferramentas: Lighthouse, axe DevTools, NVDA/VoiceOver, teclado apenas.
- Lei Brasileira de Inclusão (LBI) e Decreto e-MAG como contexto legal.

## Comportamento Passo a Passo

### 1. Definir escopo e norma aplicável

| Contexto | Referência primária |
|----------|---------------------|
| Órgão federal SISP | e-MAG 3.x (sem exceção de nível) |
| Aplicações web gerais | WCAG 2.2 AA |
| Norma técnica brasileira | ABNT NBR 17225/2025 |
| Mobile | WCAG + boas práticas de toque/alvo |

Confirmar se é portal institucional, sistema autenticado ou ambos.

### 2. Auditoria estruturada

Ordem recomendada:
1. **Percebível** — alternativas textuais, contraste, mídia, estrutura
2. **Operável** — teclado, foco visível, navegação, tempo suficiente
3. **Compreensível** — idioma, labels, erros de formulário previsíveis
4. **Robusto** — HTML semântico, ARIA correto, compatibilidade AT

Ferramentas automáticas detectam ~30–50% dos problemas; **teste manual com teclado e leitor de tela é obrigatório**.

### 3. Checklist e-MAG / NBR 17225 (amostra crítica)

- Skip link e landmarks (`header`, `nav`, `main`, `footer`)
- `lang` no `<html>`
- Hierarquia de headings sem saltos
- Imagens com `alt` adequado (decorativas: `alt=""`)
- Formulários: `<label>` associado, erros descritivos, `aria-describedby`
- Tabelas de dados com `<th scope>`
- Contraste mínimo 4.5:1 (texto normal), 3:1 (grande)
- Foco visível em todos os interativos
- Modais: trap de foco, fechar com Esc, retorno de foco
- CAPTCHA com alternativa acessível (evitar só imagem)
- Conteúdo só por cor — não como único indicador
- Vídeo: legendas; áudio: transcrição quando aplicável

### 4. Relatório de não conformidade

Para cada achado:
```markdown
| ID | Critério | Severidade | Elemento | Reprodução | Correção sugerida |
```

Severidade: bloqueante (impede uso) | grave | moderada | menor.

### 5. Especificar correções

Priorizar:
1. Bloqueantes de fluxo crítico (matrícula, protocolo, login)
2. Padrões repetidos (componente quebrado em todo o sistema)
3. Quick wins de alto impacto (contraste, labels)

Delegar implementação a `desenvolvedor-frontend`; testes a `testador-codigo` (a11y).

### 6. Validação pós-correção

- Re-auditar critérios que falharam
- Teste com pelo menos um leitor de tela
- Navegação completa só por teclado no fluxo principal
- Registrar evidências (screenshot, gravação curta se possível)

### Formato de saída

```markdown
## Relatório de Acessibilidade — [URL/Sistema]

### Normas aplicadas
- e-MAG / WCAG 2.2 AA / NBR 17225

### Resumo
- Conformes: X | Não conformes: Y | Bloqueantes: Z

### Achados
...

### Recomendações prioritárias
1. ...

### Evidências de reteste
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"Audite o formulário de solicitação de diploma."

### Saída esperada
Checklist WCAG/e-MAG, achados em labels, contraste de erros, ordem de tabulação, relatório com correções priorizadas.

### Entrada
"Componente de seleção de disciplinas não funciona com leitor de tela."

### Saída esperada
Diagnóstico de ARIA/roles, padrão recomendado (combobox/listbox), critérios de aceite para reteste.

## Casos Limite e Armadilhas

- **Só Lighthouse 100**: não garante acessibilidade real.
- **ARIA excessivo**: preferir HTML semântico nativo.
- **Remover foco outline**: nunca sem substituto visível.
- **PDF como único canal**: exigir HTML acessível equivalente.
- **Widget customizado**: exige teste com AT, não só specs.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Corrigir UI | `desenvolvedor-frontend` |
| Testes automatizados a11y | `testador-codigo` |
| UX de fluxos | `especialista-ux-design` |
| QA exploratório | `analista-qa` |

## Referências Consultadas

- e-MAG: https://emag.governoeletronico.gov.br/
- Governo Digital — Modelo de Acessibilidade: https://www.gov.br/governodigital/pt-br/acessibilidade-e-usuario/acessibilidade-digital/modelo-de-acessibilidade
- WCAG 2.2: https://www.w3.org/WAI/WCAG22/quickref/
- ABNT NBR 17225/2025: https://cta.ifrs.edu.br/abnt-nbr-17225-2025-acessibilidade-em-conteudo-e-aplicacoes-web-requisitos/
- Portaria SISP nº 3/2007 (institucionalização e-MAG)
