---
name: especialista-ux-design
description: Pesquisa com usuários, jornadas, wireframes, prototipação e testes de usabilidade para interfaces digitais. Use antes ou durante o desenvolvimento de telas — Design Thinking, validação de fluxos e handoff para frontend em contexto institucional.
compatibility: Ferramentas de prototipação (Figma, papel) e acesso a usuários/stakeholders. Complementa desenvolvedor-frontend e especialista-acessibilidade-digital.
---

# Especialista em UX / Design de Interfaces

## Visão Geral

Skill para **entender usuários e projetar experiências** antes da implementação visual final. Aplica Design Thinking e processo centrado no usuário: pesquisa, definição do problema, ideação, prototipação e teste — com entregáveis de wireframes, fluxos e critérios de usabilidade.

Em universidades federais: considerar diversidade de perfis (estudante, servidor, professor, candidato PcD), jargão institucional e obrigações de acessibilidade (e-MAG).

## Audiência e Contexto de Uso

- **Quem**: UX designers, product owners, desenvolvedores em discovery.
- **Quando acionar**:
  - novo sistema ou redesign de portal
  - fluxo complexo (matrícula, protocolo, bolsas)
  - reclamações de usabilidade ou alta taxa de abandono
  - wireframes antes de codificar
  - teste de usabilidade com usuários reais
  - alinhamento entre áreas (DTI, pró-reitorias, usuários)

## Ambiente e Dependências

- Handoff para `desenvolvedor-frontend` e `especialista-acessibilidade-digital`.
- Não substitui identidade visual institucional — consultar manual de marca se existir.
- Pesquisa com usuários: consentimento e LGPD quando gravar dados pessoais.

## Comportamento Passo a Passo

### 1. Empatizar — pesquisa

Métodos conforme prazo:
| Método | Quando |
|--------|--------|
| Entrevistas (5–8 usuários) | Entender motivações e dores |
| Contextual inquiry | Observar uso real no ambiente |
| Survey | Validar hipóteses em escala |
| Analytics | Onde abandonam, erros frequentes |
| Desk research | Normas, sistemas legados, concorrentes |

Produzir **personas** e **jornadas as-is** com evidência, não suposição.

### 2. Definir — problema

Sintetizar em ponto de vista (POV):
> [Persona] precisa [necessidade] porque [insight].

Priorizar com matriz impacto × esforço ou MoSCoW.

Pergunta de design:
> Como podemos [objetivo] para [persona] de forma [critério]?

### 3. Idear — soluções

- Brainstorm sem julgamento inicial
- Crazy 8s, how might we
- Convergir em 1–3 conceitos testáveis
- Mapear fluxos (happy path + erros + edge cases)

### 4. Prototipar — wireframes

Fidelidade conforme objetivo do teste:

| Fidelidade | Uso |
|------------|-----|
| Papel / sketch | Explorar layout rápido |
| Wireframe baixa | Estrutura, hierarquia, navegação |
| Protótipo clicável (Figma) | Teste de fluxo antes do dev |
| Alta fidelidade | Validação visual (com UI designer) |

Wireframe define:
- O que existe em cada tela
- Ordem de informação
- Navegação e CTAs
- Estados: vazio, loading, erro, sucesso

**Não** focar em cor/tipografia final no wireframe — isso é UI.

### 5. Testar — usabilidade

Protocolo mínimo:
1. Tarefas realistas ("Inscreva-se no evento X")
2. Think-aloud ou observação silenciosa
3. 5 usuários costumam revelar maioria dos problemas graves
4. Métricas: taxa de conclusão, tempo, erros, SUS se aplicável
5. Debrief e priorização de achados

Iterar protótipo antes de desenvolvimento.

### 6. Handoff para desenvolvimento

Entregar:
- Fluxo de navegação (flowchart)
- Wireframes anotados com comportamento
- Estados e mensagens de erro
- Requisitos de acessibilidade (com `especialista-acessibilidade-digital`)
- Critérios de aceite de usabilidade

### Formato de saída

```markdown
## UX — [Projeto/Feature]

### Personas
- ...

### Jornada (to-be)
[mermaid ou etapas]

### Wireframes
- [links ou descrição por tela]

### Resultados de teste
| Tarefa | Sucesso | Problemas |

### Recomendações prioritárias
1. ...

### Handoff
- Arquivos: ...
- Critérios de aceite: ...
```

## Exemplos de Entrada e Saída

### Entrada
"Redesenhar fluxo de solicitação de auxílio estudantil."

### Saída esperada
Entrevistas resumidas, jornada atual vs proposta, wireframes do wizard, teste com 5 estudantes, lista de mudanças antes do dev.

### Entrada
"Wireframe da área do servidor para consultar holerite."

### Saída esperada
Baixa fidelidade, menu, estados de erro de sessão, link para acessibilidade, handoff com notas de comportamento.

## Casos Limite e Armadilhas

- **Design sem pesquisa**: solução para designer, não para usuário.
- **Pular teste por prazo**: retrabalho de dev mais caro.
- **Wireframe de alta fidelidade cedo**: apego prematuro a layout.
- **Ignorar legado**: usuário mental model do SIGAA importa.
- **UX sem a11y**: inviável em órgão público.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Implementar UI | `desenvolvedor-frontend` |
| Acessibilidade | `especialista-acessibilidade-digital` |
| Requisitos formais | `gerente-projetos-ti` + especificação de interface |
| QA exploratório | `analista-qa` |
| Pesquisa de padrões | `pesquisador-tecnico` |

## Referências Consultadas

- Interaction Design Foundation — UX Design Processes: https://ixdf.org/literature/topics/ux-design-processes
- Stanford d.school — Design Thinking: https://dschool.stanford.edu/
- Nielsen Norman Group — Usability Testing: https://www.nngroup.com/topic/usability-testing/
- Design Thinking (IDEO/d.school framework)
- e-MAG / acessibilidade em UX público: https://emag.governoeletronico.gov.br/
