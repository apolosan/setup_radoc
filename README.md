# setup_radoc

Configuração do ambiente de **desenvolvimento assistido por IA** usado no projeto [RADOC](https://radoc.ufcat.edu.br) (Relatório Anual de Atividades Docentes — UFCAT). Este repositório concentra os artefatos que orientam agentes em **OpenCode** e **Pi Coding Agent**: regras de comportamento, servidores MCP e skills especializadas.

## Contexto

No RADOC, a evolução do código não foi tratada como diálogo isolado com um modelo, mas como fluxo apoiado por **engenharia de contexto**: recuperação de artefatos (RAG), memória em grafo, controle ativo de estado do repositório, recomendação de padrões de projeto, TDD e revisão humana. As superfícies de orquestração foram **OpenCode** e **Pi Coding Agent**, configuradas por arquivos versionados neste repositório.

**Stack do sistema RADOC:** React 19, NestJS 11, PostgreSQL (Prisma), shadcn/ui, TailwindCSS, SSO Keycloak, integrações institucionais e suíte de testes (Jest, Vitest, Playwright).

## Estrutura do repositório

| Caminho | Função |
|---------|--------|
| [`AGENTS.md`](AGENTS.md) | Regras globais para agentes: 5W2H, hierarquia MCP, orquestração por subagentes, context-mode |
| [`opencode.json`](opencode.json) | Configuração de servidores MCP e plugins para **OpenCode** |
| [`.pi/mcp.json`](.pi/mcp.json) | Configuração de servidores MCP para **Pi Coding Agent** |
| [`.pi/skills/analist/`](.pi/skills/analist/) | Skill de execução direta (análise, implementação, debug) com 5W1H e PDCA |
| [`.pi/skills/orchestrator/`](.pi/skills/orchestrator/) | Skill de orquestração com delegação a subagentes |
| [`data/`](data/) | Dados locais do ChunkHound (ex.: `chunks.duckdb`) — gerado em uso |

## Servidores MCP configurados

Ambos os manifestos (`opencode.json` e `.pi/mcp.json`) definem, entre outros:

| Servidor | Uso principal |
|----------|----------------|
| `sequential-thinking` | Raciocínio estruturado e planejamento |
| `ripgrep` | Busca textual de alta performance no código |
| `chunkhound` | Busca semântica/regex indexada (DuckDB) |
| `filesystem` | Leitura e escrita de arquivos no workspace |
| `neo4j-memory` | Memória em grafo para decisões e contexto |
| `codebase-state-manager` | Rastreamento explícito de estados do repositório |
| `design-patterns` | Recomendação de padrões de projeto |
| `desktop-commander` | Execução de comandos e processos |
| `taskmanager` | Gerenciamento de tarefas do agente |
| `shadcn`, `radix-mcp-server`, `flyonui` | Componentes e documentação de UI |
| `knip` | Detecção de código não utilizado |
| `grep` | Busca em repositórios GitHub (remoto) |
| `arxiv` | Papers locais via Docker |
| `context-mode` | Proteção de janela de contexto (OpenCode) |
| `ssh-server` | Operações SSH (OpenCode) |

Consulte os JSON para a lista completa, comandos de inicialização e variáveis de ambiente.

## Pré-requisitos

Adapte caminhos e credenciais aos seu ambiente antes de usar em produção.

- **Node.js** 20+ e **Bun** (vários servidores MCP usam `bunx` / `bun`)
- **Docker** (servidor `arxiv`, opcional)
- **Neo4j** em execução local, se usar `neo4j-memory` com Bolt (`bolt://localhost:7687`)
- Repositório **RADOC** clonado e caminhos absolutos ajustados nos JSON
- Servidores MCP locais referenciados nos manifestos (ex.: `design_patterns_mcp`, `codebase_state_manager_mcp`, `knip-mcp-server`, `mcp-shrimp-task-manager`) instalados nos caminhos indicados
- **uv** (Python) para `codebase-state-manager`
- **ChunkHound** com `.chunkhound.json` no projeto alvo e diretório `data/` para o índice

## Uso rápido

### 1. Clonar e personalizar caminhos

```bash
git clone https://github.com/apolosan/setup_radoc.git
cd setup_radoc
```

Substitua ocorrências de `/home/user/Documentos/` (e equivalentes) em `opencode.json` e `.pi/mcp.json` pelo caminho real do seu clone do RADOC e das ferramentas MCP locais.

### 2. OpenCode

Copie ou aponte o `opencode.json` para o diretório do projeto RADOC (ou use como referência na configuração global do OpenCode). Instale dependências dos servidores conforme a documentação de cada MCP.

### 3. Pi Coding Agent

O diretório `.pi/` segue a convenção do Pi: `mcp.json` registra servidores; `skills/` expõe **analist** (execução direta) e **orchestrator** (delegação). Ative skills com `/skill:analist` ou `/skill:orchestrator` conforme a tarefa.

### 4. AGENTS.md

Mantenha `AGENTS.md` na raiz do repositório de aplicação (RADOC) ou incorpore seu conteúdo às regras do agente. Ele define o protocolo 5W2H, prioridade MCP e fluxos de subagentes.

## Segurança

Os arquivos de configuração podem conter **placeholders de credenciais** (Neo4j, tokens de API). Antes de publicar forks ou usar em rede compartilhada:

1. Rotacione senhas e tokens expostos em commits anteriores.
2. Prefira variáveis de ambiente ou arquivos locais não versionados (`.env`) para segredos.
3. Revise manualmente código gerado por agentes, complementando com análise estática (ex.: Semgrep).

## Licença

Consulte o [site do RADOC](https://radoc.ufcat.edu.br) para a licença do software. Este repositório de configuração segue a mesma política adotada pelo projeto RADOC/UFCAT, salvo indicação em contrário.

## Autor

**Einar César Santos** — Universidade Federal de Catalão (UFCAT) — [einar@ufcat.edu.br](mailto:einar@ufcat.edu.br)
