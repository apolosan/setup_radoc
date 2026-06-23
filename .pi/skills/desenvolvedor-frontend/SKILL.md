---
name: desenvolvedor-frontend
description: Desenvolve interfaces web e mobile — componentes, páginas, estado, roteamento, responsividade e acessibilidade. Use para UI, UX técnica, SPAs, apps mobile, design system e integração com APIs no cliente.
compatibility: Framework frontend detectado em runtime (React, Vue, Angular, Flutter, etc.). Navegador ou emulador quando necessário.
---

# Desenvolvedor Frontend — Web e Mobile

## Visão Geral

Sub-skill de `desenvolvedor-codigo` focada em **camada de apresentação e interação**. Implementa interfaces consumindo APIs, respeitando design system, acessibilidade e performance perceptível.

## Audiência e Contexto de Uso

- **Quem**: desenvolvedores frontend, agentes de UI.
- **Quando acionar**:
  - componentes, páginas, formulários, listas
  - responsividade e breakpoints
  - estado global/local, roteamento
  - apps mobile (React Native, Flutter, etc.)
  - acessibilidade (WCAG)
  - integração cliente com backend

## Ambiente e Dependências

- Herda protocolo de `desenvolvedor-codigo` e ciclo padrão do agente.
- Detectar: framework, bundler, CSS approach, libs de UI.
- Ferramentas: dev server, linter, testes de componente (Testing Library, etc.).

## Comportamento Passo a Passo

### 1. Reconhecimento do stack

Identificar no projeto:
- Framework e versão
- Gerenciamento de estado (Redux, Zustand, Context, etc.)
- Roteamento
- Design system / component library
- Padrão de fetch (REST, GraphQL, tRPC)
- Estrutura de pastas (`components/`, `pages/`, `features/`)

### 2. Análise da feature UI

| Aspecto | Perguntas |
|---------|-----------|
| Telas | Quais views/rotas? |
| Dados | Quais props/APIs? Estados loading/error/empty? |
| Interações | Eventos, validação, feedback |
| Layout | Mobile-first? Breakpoints? |
| A11y | Labels, foco, contraste, leitores de tela |

### 3. Implementação

Ordem recomendada:
1. **Tipos/contratos** de dados (interfaces, schemas)
2. **Componentes atômicos** reutilizáveis
3. **Composição** em páginas/features
4. **Estado e efeitos** (fetch, cache, formulários)
5. **Estilos** alinhados ao design system existente
6. **Acessibilidade** — semantic HTML, ARIA quando necessário, keyboard nav

Princípios:
- Componentes pequenos e com responsabilidade única
- Preferir composição sobre herança
- Loading e error states explícitos
- Evitar re-renders desnecessários (memo quando medido)
- Não hardcodar strings de UI se projeto usa i18n

### 4. Integração com API

- Consumir contratos definidos (OpenAPI, tipos compartilhados)
- Tratar 4xx/5xx com UX clara
- Debounce em buscas; otimistic UI só com rollback
- Nunca expor secrets no bundle cliente

### 5. Validação

1. Testes de componente (render, interação, a11y)
2. Visual em viewport alvo (ou screenshot se disponível)
3. Lighthouse/a11y audit quando crítico
4. Cross-browser se requisito do projeto

### Formato de saída

```markdown
## Frontend — [Feature]

### Componentes criados/alterados
- `path` — propósito

### Rotas
- ...

### Estados tratados
- loading | error | empty | success

### A11y
- ...

### Testes
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"Formulário de cadastro com validação em tempo real."

### Saída esperada
Form com campos, validação client-side alinhada a regras de negócio, mensagens de erro acessíveis, testes de interação.

### Entrada
"Lista de produtos com filtro e paginação."

### Saída esperada
Tabela/lista responsiva, debounce no filtro, skeleton loading, integração com API paginada.

## Casos Limite e Armadilhas

- **Lógica de negócio no componente**: extrair para hooks/services.
- **CSS ad hoc**: quebrar design system.
- **A11y como afterthought**: incluir desde o início.
- **Bundle gigante**: lazy load de rotas e componentes pesados.
- **Assumir desktop**: validar mobile se público inclui.

## Referências Consultadas

- WCAG 2.2 Quick Reference: https://www.w3.org/WAI/WCAG22/quickref/
- React Testing Library principles: https://testing-library.com/docs/guiding-principles/
- web.dev — Performance e a11y: https://web.dev/
- Skill pai: `desenvolvedor-codigo`
- Agent Skills Specification: https://agentskills.io/specification
