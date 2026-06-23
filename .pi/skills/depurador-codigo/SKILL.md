---
name: depurador-codigo
description: Depura código-fonte com método científico sistemático — reproduzir, hipotetizar, testar e corrigir causa raiz. Use para bugs, erros de runtime, testes falhando, regressões, comportamento inesperado ou incidentes em produção.
compatibility: Acesso a leitura de código, execução de testes, logs, debugger e shell. Stack-agnóstico.
---

# Depurador de Código-Fonte

## Visão Geral

Skill para **investigar e eliminar defeitos** seguindo debugging científico em quatro fases. Prioriza **causa raiz** sobre correção de sintoma. Não implementa features novas — foca em entender, reproduzir, corrigir e prevenir regressão.

## Audiência e Contexto de Uso

- **Quem**: desenvolvedores, agentes de correção, SRE em análise de incidentes.
- **Quando acionar**:
  - "Por que isso está falhando?"
  - "Debug este erro"
  - testes vermelhos sem causa óbvia
  - bug intermitente ou não reproduzível à primeira vista
  - regressão após deploy ou refactor

## Ambiente e Dependências

- Leitura do codebase, execução de testes e comandos de build.
- Acesso a logs, stack traces, métricas quando disponíveis.
- Seguir ciclo padrão do agente: contexto antes de ação; registrar estado após correção.

## Comportamento Passo a Passo

### Fase 1 — Investigação da causa raiz

1. **Ler mensagens de erro integralmente** — não assumir; citar erro exato.
2. **Reproduzir de forma consistente** — passos mínimos; se intermitente, isolar variáveis (carga, timing, dados).
3. **Verificar mudanças recentes** — git log, diff, deploys, config.
4. **Coletar evidências** em sistemas multi-componente — logs por camada, request IDs, traces.
5. **Rastrear fluxo de dados** — do sintoma até a origem (binary search no espaço do problema).

**Checklist de conclusão da Fase 1:**
- [ ] Sintoma descrito com precisão
- [ ] Reprodução documentada (ou hipótese de intermitência)
- [ ] Escopo delimitado (arquivo, módulo, camada)

### Fase 2 — Análise de padrões

1. Encontrar **exemplos funcionais** similares no mesmo codebase.
2. **Comparar** implementação quebrada vs referência.
3. Identificar diferenças (config, tipos, nullability, async, permissões).
4. Mapear **dependências** externas afetadas.

### Fase 3 — Hipótese e teste

1. Formular **uma hipótese falsificável** por vez.
   - Ruim: "Algo está errado no cache."
   - Bom: "O cache retorna stale porque TTL não invalida após update do perfil."
2. **Prever** resultado se hipótese for verdadeira.
3. **Experimentar minimamente** — um breakpoint, um log, um stub, um teste isolado.
4. **Atualizar** hipótese com base em evidência; repetir.

Técnicas auxiliares:
- **Binary search** no código e nos dados
- **5 Whys** para ir além do sintoma
- **Logbook** de hipóteses testadas (evitar repetir falhas)

### Fase 4 — Implementação da correção

1. **Criar teste que reproduz o bug** (deve falhar antes do fix).
2. **Corrigir causa raiz** — mudança mínima, sem refactor oportunista.
3. **Verificar**: teste novo verde + suite existente verde.
4. **Regra dos três**: se 3+ fixes falharam, parar e questionar arquitetura.
5. **Registrar** causa, fix e prevenção em `log de eventos do ciclo`.

### Formato de saída

```markdown
## Relatório de Debug

### Sintoma
...

### Reprodução
1. ...

### Causa raiz
...

### Evidência
- ...

### Correção
- Arquivo: ...
- Mudança: ...

### Verificação
- [ ] Teste de regressão adicionado
- [ ] Suite passando

### Prevenção
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"O endpoint /users retorna 500 quando email tem acento."

### Saída esperada
Reprodução com curl, trace até encoding/validação, hipótese testada, fix na camada correta, teste com email acentuado.

### Entrada
"Testes passam localmente mas falham no CI."

### Saída esperada
Comparar ambiente (versão, env vars, timezone, paralelismo), hipótese de flakiness ou diferença de config, evidência de logs CI.

## Casos Limite e Armadilhas

- **Fix de sintoma**: mascarar null com default sem entender por que é null.
- **Shotgun debugging**: mudanças aleatórias sem hipótese.
- **Múltiplas hipóteses simultâneas**: testar uma variável por vez.
- **Heisenbug**: adicionar logs que alteram timing — usar breakpoints condicionais.
- **Corrigir sem teste**: regressão garantida no futuro.

## Anti-padrões

| Evitar | Fazer |
|--------|-------|
| "Deve ser race condition" sem evidência | Reproduzir com carga controlada |
| Refatorar durante debug | Fix mínimo; refactor depois |
| Ignorar mensagem de erro | Ler stack trace completo |
| Workaround em produção sem RCA | Documentar workaround + ticket de causa raiz |

## Referências Consultadas

- Zeller, "Why Programs Fail" (Scientific Debugging): https://courses.cs.duke.edu/compsci308/spring26/readings/Zeller_Scientific_Debugging.pdf
- Scientific Mindset in Debugging: https://www.thisdot.co/blog/the-importance-of-a-scientific-mindset-in-software-engineering-part-2
- Debugging using the Scientific Method: https://richardbrunt.co.uk/blog/debugging-using-the-scientific-method/
- Systematic Debugging (4 phases): https://docs.gormes.ai/upstream-hermes/user-guide/skills/bundled/software-development/software-development-systematic-debugging/
- Debugging as Scientific Reasoning: https://siliconwit.com/education/critical-thinking-engineers/debugging-as-reasoning/
