---
name: especialista-autenticacao-federada
description: Implementa autenticação federada e SSO — CAFe/RNP (SAML 2.0), Gov.br, LDAP/Active Directory institucional, OAuth2/OIDC. Use para login único, integração de IdP/SP, atributos eduPerson e acesso a serviços acadêmicos.
compatibility: Acesso a metadados SAML/OIDC, certificados, documentação RNP e Gov.br. Ambiente de homologação federado quando disponível.
---

# Especialista em Autenticação Federada

## Visão Geral

Skill para **projetar e integrar autenticação** em ambientes acadêmicos e públicos brasileiros. Cobre federação acadêmica (CAFe/RNP com SAML 2.0), login cidadão (Gov.br), diretórios institucionais (LDAP/AD) e protocolos modernos (OAuth2/OIDC).

CAFe permite SSO para milhares de serviços com credenciais institucionais; Gov.br atende serviços públicos ao cidadão — são ecossistemas complementares, não idênticos.

## Audiência e Contexto de Uso

- **Quem**: desenvolvedores, administradores de identidade, equipe de TI/RNP.
- **Quando acionar**:
  - integrar sistema como Service Provider (SP) na CAFe
  - configurar IdP institucional (Shibboleth, Keycloak, etc.)
  - login com Gov.br em portal público
  - sincronizar LDAP/AD com aplicação
  - mapear atributos (eduPerson, brEduPerson, grupos)
  - MFA e políticas de sessão

## Ambiente e Dependências

- Herda `desenvolvedor-backend` e `especialista-seguranca-informacao`.
- Certificados TLS e metadados federados via canais oficiais RNP.
- Homologação na federação antes de produção.
- LGPD: atributos mínimos necessários (data minimization).

## Comportamento Passo a Passo

### 1. Escolher mecanismo

| Cenário | Mecanismo típico |
|---------|------------------|
| Serviço para comunidade acadêmica | CAFe (SAML 2.0) |
| Serviço público ao cidadão | Gov.br (OAuth2/OIDC) |
| Sistemas internos campus | LDAP/AD ou SSO local |
| API moderna / mobile | OIDC + JWT |
| Parceiro internacional | eduGAIN (via CAFe) |

### 2. CAFe — fluxo SAML 2.0

Papéis:
- **IdP** (Identity Provider) — universidade autentica usuário
- **SP** (Service Provider) — aplicação consome asserção

Fluxo resumido:
```
Usuário → SP → IdP (login) → Asserção SAML → SP (sessão)
```

Requisitos técnicos CAFe:
- Protocolo SAML 2.0
- Shibboleth 2.x suportado; outros sob responsabilidade da instituição
- Metadados SAML 2.0 Metadata
- Certificados: FQDN, RSA/ECDSA SHA-2, serverAuth/clientAuth
- Atributos recomendados: eduPerson, brEduPerson (formato urn:oid:)

### 3. Atributos e autorização

- Separar **autenticação** (quem é) de **autorização** (o que pode)
- Mapear atributos liberados pelo IdP para roles locais
- Não confiar apenas em email sem validar asserção assinada
- Documentar atributos obrigatórios vs opcionais no metadata do SP

### 4. Gov.br

- Integração via OpenID Connect conforme documentação oficial
- Níveis de conta (bronze, prata, ouro) podem afetar elegibilidade
- Usar para serviços ao cidadão; combinar com CAFe para servidores/alunos quando política permitir

### 5. LDAP/AD institucional

- Bind autenticado, TLS obrigatório (LDAPS ou StartTLS)
- Conta de serviço com least privilege
- Cache de grupos com TTL; invalidação em logout
- Não expor LDAP à internet

### 6. Implementação segura

- HTTPS em todos os endpoints
- Validar assinatura e validade de asserções SAML
- Proteção CSRF no fluxo OAuth/OIDC (state, PKCE)
- Rotação de certificados antes do vencimento
- Logout federado (SLO) quando suportado
- Sessão com timeout e cookies Secure/HttpOnly/SameSite

### Formato de saída

```markdown
## Integração de Autenticação — [Sistema]

### Mecanismo
CAFe SAML 2.0 | Gov.br OIDC | LDAP

### Papéis
- IdP: ...
- SP: ...

### Atributos
| Atributo | Uso | Obrigatório |

### Fluxo
[diagrama]

### Checklist homologação
- [ ] Metadata publicado
- [ ] Certificado válido
- [ ] Teste com conta real
- [ ] Logout | MFA | Autorização por perfil
```

## Exemplos de Entrada e Saída

### Entrada
"Novo portal de pesquisa deve usar login CAFe."

### Saída esperada
SP Shibboleth ou middleware SAML, metadata, atributos eduPersonPrincipalName, mapeamento para perfis, ambiente de testes federado.

### Entrada
"API REST precisa de tokens para apps mobile internos."

### Saída esperada
OIDC com Keycloak/institucional, PKCE, scopes mínimos, refresh token policy, integração opcional com CAFe para mesmo realm.

## Casos Limite e Armadilhas

- **Misturar CAFe e Gov.br no mesmo fluxo sem política**: confusão de identidade.
- **Atributos a mais**: violação LGPD e rejeição pelo IdP.
- **Certificado expirado**: indisponibilidade total do login.
- **SP sem metadata atualizado**: falha após rotação de cert IdP.
- **Autorização só no frontend**: sempre validar no backend.

## Integração com outras skills

| Situação | Skill |
|----------|-------|
| Código auth | `desenvolvedor-backend` |
| Segurança | `especialista-seguranca-informacao` |
| LGPD em atributos | `especialista-lgpd` |
| Integração SIG | `integrador-sistemas-institucionais` |

## Referências Consultadas

- CAFe — Central de Ajuda RNP: https://ajuda.rnp.br/cafe
- Especificações Técnicas CAFe (SAML 2.0): https://plataforma.rnp.br/
- RNP — Serviços de Gestão de Identidade: https://www.rnp.br/servicos/servicos-do-sistema-rnp/
- Gov.br — Documentação integração: https://www.gov.br/governodigital/
- SeamlessAccess / discovery: https://seamlessaccess.org/
