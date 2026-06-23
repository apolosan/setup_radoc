---
name: desenvolvedor-blockchain
description: Desenvolve soluções blockchain — smart contracts, integração wallet, transações on-chain, testes em rede local e auditoria básica de segurança. Use para DeFi, NFTs, dApps, tokens e integração Web3.
compatibility: EVM ou outra VM detectada no projeto (Solidity, Rust/Anchor, etc.). Node de teste ou testnet quando necessário.
---

# Desenvolvedor Blockchain

## Visão Geral

Sub-skill de `desenvolvedor-codigo` para **sistemas distribuídos on-chain** e integração off-chain. Enfatiza segurança, custo de gas, imutabilidade e testes rigorosos — bugs em produção são frequentemente irreversíveis.

## Audiência e Contexto de Uso

- **Quem**: desenvolvedores Web3, agentes de smart contracts.
- **Quando acionar**:
  - smart contracts (ERC-20, ERC-721, custom)
  - dApps e integração wallet (MetaMask, WalletConnect)
  - deploy e upgrade patterns
  - indexação de eventos
  - testes em Hardhat/Foundry/Anchor

## Ambiente e Dependências

- Herda `desenvolvedor-codigo` e ciclo padrão do agente.
- Detectar: chain alvo, linguagem de contrato, framework de teste.
- **Nunca** expor private keys; usar env seguro e test accounts locais.

## Comportamento Passo a Passo

### 1. Reconhecimento

- Rede alvo (mainnet, L2, testnet, local)
- Linguagem (Solidity, Vyper, Rust, etc.)
- Framework (Hardhat, Foundry, Truffle, Anchor)
- Padrões existentes (OpenZeppelin, upgradeable proxies)
- Frontend Web3 lib (ethers.js, viem, wagmi)

### 2. Design on-chain

Considerar **antes** de codificar:
| Fator | Pergunta |
|-------|----------|
| Imutabilidade | Upgrade necessário? Qual pattern? |
| Gas | Operações custosas em loop? |
| Segurança | Reentrancy, overflow, access control? |
| Oráculos | Dados externos confiáveis? |
| Composabilidade | Interfaces padrão (ERC)? |

### 3. Implementação de contratos

Ordem:
1. **Interfaces** e events
2. **Storage layout** — evitar collision em upgrades
3. **Lógica core** com checks-effects-interactions
4. **Access control** (Ownable, RBAC, roles)
5. **Testes** unitários e de integração on-chain

Princípios:
- Preferir libs auditadas (OpenZeppelin)
- `require`/`revert` com mensagens ou custom errors (gas)
- Pull over push para pagamentos
- Pausable quando risco alto
- Documentar invariantes no NatSpec

### 4. Integração off-chain

- ABI e endereços por rede em config
- Tratar rejeição de wallet e chain mismatch
- Aguardar confirmações adequadas
- Indexar eventos para UI responsiva
- Nunca confiar só no frontend — validar on-chain

### 5. Validação

1. Suite de testes (100% paths críticos)
2. Static analysis (Slither, Mythril) quando disponível
3. Gas report em funções principais
4. Testnet deploy antes de mainnet
5. Escalar mainnet deploy para aprovação humana

### Formato de saída

```markdown
## Blockchain — [Feature]

### Contratos
- `Contract.sol` — responsabilidade

### Redes / Endereços
- local | testnet | mainnet (pendente aprovação)

### Riscos de segurança
- ...

### Gas (estimativa)
- ...

### Testes
- ...
```

## Exemplos de Entrada e Saída

### Entrada
"Token ERC-20 com mint restrito a owner."

### Saída esperada
Contrato usando OpenZeppelin, testes de mint/burn/transfer, eventos, deploy script local.

### Entrada
"Frontend para conectar wallet e exibir saldo."

### Saída esperada
Integração com lib Web3 do projeto, tratamento de rede errada, leitura via contract call.

## Casos Limite e Armadilhas

- **Reentrancy**: usar CEI pattern ou ReentrancyGuard.
- **Integer issues**: Solidity 0.8+ ou SafeMath.
- **Front-running**: considerar commit-reveal ou private mempool.
- **Upgrade sem storage gap**: corrupção de estado.
- **Deploy mainnet sem auditoria**: escalar risco ao usuário.

## Referências Consultadas

- OpenZeppelin Contracts: https://docs.openzeppelin.com/contracts/
- Ethereum Smart Contract Best Practices: https://consensys.github.io/smart-contract-best-practices/
- SWC Registry (vulnerabilidades): https://swcregistry.io/
- Skill pai: `desenvolvedor-codigo`
- EIPs em ethereum.org
