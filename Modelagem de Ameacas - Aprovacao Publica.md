# Relatório de AppSec: Modelagem de Ameaças (Threat Modeling)
**Escopo:** Feature de Aprovação Externa de Pagamentos de Alto Valor (Public/No-Auth)
**Classificação de Risco:** CRÍTICO (Insecure by Design)

## Resumo Executivo de Segurança
A proposta original de expor uma interface pública, sem camada de autenticação, para aprovação de transações financeiras de alto valor e exibição de dados sensíveis constitui uma violação frontal dos princípios de *Zero Trust* e *Defense in Depth*. A solução, como solicitada inicialmente, é classificada como **Insecure by Design** (OWASP A04:2021), apresentando riscos catastróficos de fraude financeira e vazamento de dados. 

Abaixo, o mapeamento técnico dos vetores de ataque e vulnerabilidades introduzidas por este modelo.

---

## 1. Broken Access Control & Falta de Não-Repúdio (Non-Repudiation)
A ausência de um mecanismo robusto de autenticação (SSO, MFA ou Biometria) na borda elimina qualquer capacidade de rastreabilidade legal da ação.

*   **Risco de Repúdio (STRIDE - Repudiation):** Em caso de fraude, o aprovador legítimo pode facilmente alegar que não autorizou a transação (repúdio). Sem o uso de assinaturas criptográficas (como WebAuthn/FIDO2) atreladas ao dispositivo ou biometria do usuário, a instituição não possui evidências técnicas ou legais para provar quem clicou no botão de aprovação.
*   **Spoofing de Identidade:** Qualquer indivíduo ou script que possuir acesso à URL pública tem permissão implícita de atuar em nome do executivo aprovador, caracterizando falsificação de identidade sistêmica.
*   **BOLA/IDOR (Broken Object Level Authorization):** Se o identificador da transação na URL (ex: `/aprovar/12345`) for previsível ou sequencial, um atacante pode iterar sobre os IDs (Força Bruta) e aprovar/visualizar pagamentos que não lhe pertencem.

## 2. Interceptação e Man-in-the-Middle (MitM)
O envio de URLs de acesso direto para redes externas (fora do perímetro corporativo ou VPN) expõe a sessão a agentes intermediários.

*   **Man-in-the-Middle (MitM):** Como o usuário estará operando a partir de redes não confiáveis (Wi-Fi de aeroportos, hotéis ou redes 4G/5G), agentes maliciosos monitorando o tráfego de rede — ou proxies de inspeção SSL/TLS corporativos — podem interceptar a URL (via proxy, DNS hijacking ou roteadores comprometidos). Ao capturar o link ativo, o atacante pode consumi-lo antes do usuário legítimo.
*   **Vazamento via Referer / Histórico:** URLs contendo tokens de acesso sensíveis podem vazar por meio de cabeçalhos HTTP `Referer` para serviços de terceiros, logs de proxies, histórico do navegador do usuário e extensões maliciosas no browser.

## 3. Sensitive Data Exposure (OWASP A02:2021 - Cryptographic Failures)
A exposição de dados transacionais na internet pública fere regulamentações de privacidade e sigilo bancário.

*   **Vazamento de PII e Dados Financeiros:** A exibição de CPFs, CNPJs, nomes, contas de destino e volumes financeiros sem *Data Masking* adequado, e sem validação prévia de quem está acessando a tela, garante que qualquer pessoa com a URL veja dados classificados como estritamente confidenciais.
*   **Scraping e Enumeração:** Botnets podem automatizar o acesso a essas páginas para raspar (scrape) e compilar dados de fluxo de caixa, estrutura de fornecedores e carteira de pagamentos da empresa, caracterizando espionagem corporativa.

## 4. Business Logic Flaws & Falhas de Idempotência
A camada de negócio bancário não pode confiar na integridade de uma requisição originada em uma tela pública sem sessão estabelecida.

*   **Replay Attacks (Ataques de Repetição):** Se o endpoint de aprovação não implementar idempotência atômica e estrita (consumo de token único/One-Time Use com travamento de concorrência), um atacante pode capturar a requisição HTTP `POST` e reenviá-la repetidas vezes, gerando múltiplas liquidações indesejadas para a mesma origem.
*   **Tampering (Manipulação de Payload):** Se a aprovação depender de dados enviados do frontend para o backend (como valor e conta), um atacante interceptando a requisição pode modificar o payload no navegador (via *DevTools* ou *Burp Suite*), alterando a conta de destino da transferência de altíssimo valor antes da requisição atingir o WAF.

---
**Conclusão da Equipe de AppSec:** 
A implementação do requisito na forma solicitada deve ser classificada como "Risco Inaceitável / No-Go". A liberação de recursos financeiros de alto valor exige *Transaction Binding* e autenticação baseada em risco (RBA). A migração para um fluxo com tokens opacos efêmeros e validação biométrica em hardware (Passkeys) é mandatória para mitigar essas vulnerabilidades.