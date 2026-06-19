---
marp: true
theme: default
paginate: true
footer: 'Procurement — Uma Visão Estratégica'
style: |
  :root {
    --color-primary: #1a3a5c;
    --color-accent: #0077b6;
    --color-light: #e8f4fc;
    --color-muted: #64748b;
    --color-bg: #ffffff;
  }

  section {
    background-color: var(--color-bg);
    color: #1e293b;
    font-family: 'Segoe UI', system-ui, sans-serif;
    font-size: 18px;
    padding: 40px 60px;
  }

  /* Slide de título (capa) */
  section.lead {
    background:
      radial-gradient(ellipse at 25% 75%, rgba(0,180,255,0.18) 0%, transparent 55%),
      linear-gradient(160deg, #0a1f36 0%, #1a3a5c 50%, #0a5c8a 100%);
    color: #ffffff;
    text-align: center;
    justify-content: center;
    padding: 60px 80px;
  }
  section.lead h1 {
    font-size: 4em;
    color: #ffffff;
    border: none;
    margin-bottom: 0.1em;
    font-weight: 900;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    line-height: 1.05;
  }
  section.lead h1::after {
    content: '';
    display: block;
    width: 80px;
    height: 4px;
    background: #38bdf8;
    margin: 0.35em auto 0;
    border-radius: 2px;
  }
  section.lead h2 {
    color: rgba(255,255,255,0.72);
    font-size: 1.35em;
    font-weight: 300;
    letter-spacing: 0.1em;
    margin-top: 0.6em;
    margin-bottom: 1em;
  }
  section.lead p {
    color: rgba(255,255,255,0.5);
    font-size: 0.88em;
    max-width: 560px;
    margin: 0 auto 0.8em;
    line-height: 1.65;
  }
  section.lead strong {
    color: #7dd3fc;
    font-size: 1.05em;
    letter-spacing: 0.05em;
  }

  /* Slides de seção (# H1 como divisor de capítulo) */
  section.section-title {
    background: linear-gradient(160deg, #0a1f36 0%, #1a3a5c 100%);
    color: #ffffff;
    display: flex;
    flex-direction: column;
    justify-content: center;
    border-left: 8px solid #38bdf8;
  }
  section.section-title h1 {
    color: #ffffff;
    font-size: 2.2em;
    border: none;
    border-bottom: 2px solid rgba(56,189,248,0.45);
    padding-bottom: 0.3em;
    margin-bottom: 0.4em;
    font-weight: 800;
    letter-spacing: 0.03em;
  }
  section.section-title h2 {
    color: #7dd3fc;
    font-size: 1.1em;
    font-weight: 400;
    border: none;
    padding-left: 0;
    letter-spacing: 0.05em;
    margin-top: 0.2em;
  }
  section.section-title p,
  section.section-title ul {
    color: rgba(255,255,255,0.75);
    font-size: 0.92em;
  }

  /* Headings gerais */
  h1 {
    color: var(--color-primary);
    font-size: 1.6em;
    border-bottom: 2px solid var(--color-accent);
    padding-bottom: 0.2em;
    margin-bottom: 0.5em;
  }
  h2 {
    color: var(--color-accent);
    font-size: 1.25em;
    margin-bottom: 0.4em;
    padding-left: 12px;
    border-left: 4px solid var(--color-accent);
  }
  h3 {
    color: var(--color-primary);
    font-size: 1.05em;
  }

  /* Tabelas */
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.82em;
    margin-top: 0.6em;
  }
  thead tr {
    background-color: var(--color-primary);
    color: #ffffff;
  }
  th {
    padding: 8px 12px;
    text-align: left;
    font-weight: 600;
  }
  td {
    padding: 6px 12px;
    border-bottom: 1px solid #e2e8f0;
  }
  tr:nth-child(even) td {
    background-color: var(--color-light);
  }
  tr:hover td {
    background-color: #dbeafe;
  }

  /* Blockquotes */
  blockquote {
    background: var(--color-light);
    border-left: 4px solid var(--color-accent);
    border-radius: 0 6px 6px 0;
    padding: 12px 20px;
    margin: 12px 0;
    color: var(--color-primary);
    font-style: italic;
  }
  blockquote p { margin: 0; }

  /* Listas */
  ul, ol {
    padding-left: 1.4em;
    line-height: 1.7;
  }
  li { margin-bottom: 0.15em; }

  /* Paginação e rodapé */
  footer {
    font-size: 0.68em;
    color: var(--color-muted);
    border-top: 1px solid #e2e8f0;
    padding-top: 4px;
  }
  section::after {
    color: var(--color-muted);
    font-size: 0.72em;
    font-weight: 600;
  }

  /* Agenda slide */
  section.agenda {
    padding: 40px 80px;
  }
  section.agenda ol {
    list-style: none;
    padding: 0;
    counter-reset: agenda-counter;
  }
  section.agenda ol li {
    counter-increment: agenda-counter;
    display: flex;
    align-items: baseline;
    gap: 14px;
    padding: 6px 0;
    border-bottom: 1px solid #e2e8f0;
    font-size: 1.05em;
  }
  section.agenda ol li::before {
    content: counter(agenda-counter, decimal-leading-zero);
    color: var(--color-accent);
    font-size: 0.85em;
    font-weight: 700;
    min-width: 26px;
  }

  /* Tabelas em slides de seção (fundo escuro) */
  section.section-title table { font-size: 0.82em; }
  section.section-title thead tr { background-color: rgba(255,255,255,0.2); }
  section.section-title th { color: #ffffff; }
  section.section-title td { color: rgba(255,255,255,0.9); border-bottom: 1px solid rgba(255,255,255,0.15); }
  section.section-title tr:nth-child(even) td { background-color: rgba(255,255,255,0.08); }
  section.section-title tr:hover td { background-color: rgba(255,255,255,0.15); }

  /* Strong / em em destaque */
  strong { color: var(--color-primary); }
  section.section-title strong,
  section.lead strong { color: #ffffff; }
---

<!-- _paginate: false -->
<!-- _footer: '' -->
<!-- _class: lead -->

# Procurement
## Uma Visão Estratégica

Uma análise completa do mercado de Compras Corporativas: conceitos, desafios, oportunidades e o futuro orientado por IA.

**2026**

---

<!-- _paginate: false -->
<!-- _class: agenda -->

## Agenda

1. O que é Procurement
2. Responsabilidades, Importância e Benefícios
3. Procurement × Sourcing × Supply Chain × Purchasing
4. O Ciclo de Vida do Procurement (S2P)
5. Desafios do Procurement
6. Oportunidades em Procurement
7. Principais KPIs
8. Tendências Futuras
9. Conclusão e Recomendações

---

# O que é Procurement

Procurement é um conjunto de **atividades estratégicas**, implementadas no setor de compras, com o objetivo de gerar mais eficiência e menos riscos ao processo de aquisição de bens e/ou serviços.

De forma prática, contempla etapas que vão desde o **levantamento de necessidades**, passando pela **pesquisa de mercado e cotação**, chegando até o **fechamento de contrato** com o fornecedor.

Ou seja, são várias ações pensadas, definidas e aplicadas de forma estratégica, as quais permitem um monitoramento mais preciso de tudo o que envolve a gestão da cadeia de abastecimento.

> É o que alinha o departamento de compras com outros setores, fortalecendo as relações da empresa com sua rede de fornecedores e parceiros.

---

## Responsabilidades da Área de Procurement

A área é responsável por garantir acesso aos produtos e serviços necessários, otimizando custos e minimizando riscos.

- Identificação das necessidades da empresa (cadeia de suprimentos)
- Busca e seleção de bons fornecedores
- Negociação com os fornecedores escolhidos
- Pesquisa de preços e de itens de qualidade
- Elaboração e acompanhamento de cronogramas de reposição
- Gestão de contratos
- Avaliação de desempenho dos fornecedores

---

## Importância e Grandes Números

| Indicador | Dado |
|---|---|
| Controle financeiro | **40%–80%** de todos os custos da empresa |
| Gasto com fornecedores externos (indústria) | **50%–70%** da receita |
| Mercado global de softwares de compras (2024) | **US$ 7,5 bilhões** |
| Previsão de mercado de softwares (2034) | **US$ 17,8 bilhões** |
| Mercado de terceirização de compras (2024) | **US$ 6,87 bilhões** |
| Impacto da IA na eficiência das equipes | **+30%** |
| Economias diretas geradas por IA | **5%–10%** nos gastos |
| CPOs reportando ao CEO (2026) | **33%** (dobrou) |

---

## Para que serve o Procurement?

Além de economizar dinheiro, o procurement é fundamental para:

- **Garantir a operação:** Evita paralisação das atividades por falta de materiais e serviços
- **Garantir a qualidade:** Assegura que os insumos adquiridos tenham o padrão necessário para não prejudicar o produto final
- **Reduzir riscos e desperdícios:** Protege contra problemas com fornecedores, atrasos, fraudes e desperdício de recursos

> O procurement deixou de ser apenas um setor que "gasta dinheiro" para se tornar um **pilar estratégico** que protege e gera valor para a companhia.

---

## Vantagens e Benefícios

- Processos otimizados, mais eficientes e realizados com mais agilidade
- Redução dos gastos relacionados à cadeia de suprimentos
- Aprimoramento do relacionamento com os fornecedores
- Realização de negociações mais vantajosas para a companhia
- Aumento da qualidade dos itens e serviços adquiridos
- Crescimento do nível de satisfação dos clientes
- Elevação do poder competitivo da empresa
- Aumento da capacidade de adequação a diferentes cenários
- Crescimento do potencial para tomar decisões mais precisas
- Mitigação dos riscos característicos da contratação de fornecedores

---

## Principais Desafios do Procurement

Desafios estruturais mais comuns:

- Dificuldade de encontrar fornecedores qualificados e confiáveis
- Complexidade na gestão de contratos e acompanhamento de desempenho
- Riscos de compliance e fraudes em processos complexos
- Pressão por redução de custos sem considerar o valor total e os riscos

**No dia a dia:**

- **Trabalho manual e burocracia:** Excesso de tarefas repetitivas e falta de automação
- **Falhas de comunicação:** Pedidos urgentes ou desalinhados de outras áreas
- **Atrasos e imprevistos:** Fornecedores que não entregam no prazo
- **Equilibrar custo e qualidade:** Pressão para reduzir custos sem perder eficácia
- **Compliance:** Garantir que fornecedores sigam todas as leis e normas regulatórias

---

<!-- _class: section-title -->

# Diferenças Conceituais

## Procurement × Sourcing × Supply Chain × Purchasing

---

## Procurement × Sourcing

A principal diferença é o **escopo**:

- **Sourcing** analisa aspectos mais **pontuais** da área de compras — é o *início* do fluxo de aquisição
- **Procurement** é mais **abrangente**, gerenciando toda a estratégia de aquisição de ponta a ponta

O processo de sourcing **faz parte** do procurement: identifica necessidades e orienta profissionais de compras a pesquisar, avaliar, negociar e contratar fornecedores.

É possível dizer que sourcing é a etapa responsável pelos elos da cadeia de suprimentos que garante o necessário para aquisições da maneira certa.

---

## 📊 Comparativo: Sourcing vs. Procurement

| Característica | Sourcing | Procurement |
|---|---|---|
| **Foco** | Encontrar e selecionar os melhores fornecedores | Garantir o funcionamento estratégico de toda a cadeia |
| **Escopo** | Analisa aspectos pontuais | Gerencia toda a estratégia de aquisição |
| **Fase** | Início do fluxo de aquisição | Ponta a ponta (planejamento ao pagamento) |
| **Principais Ações** | Pesquisar, avaliar, negociar e contratar | Gerenciar transações, contratos, pedidos e histórico |
| **Relação** | Opera dentro das regras do Procurement | Estabelece as regras e métricas para as parcerias |

---

## Procurement × Supply Chain

A diferença fundamental é a **abrangência**:

- **Supply Chain** é o **macroprocesso** — fluxo completo de ponta a ponta
- **Procurement** é uma **função estratégica** de aquisição que existe *dentro* da Supply Chain

### 🌐 Supply Chain (Cadeia de Suprimentos)
Representa o fluxo completo desde a **obtenção de matéria-prima** até a **entrega ao consumidor final**, incluindo fabricação, armazenamento, transporte, centros de distribuição e gestão de estoques.

### 🏢 Procurement
"Porta de entrada" dos recursos: foco específico no **abastecimento da empresa** — da pesquisa de mercado ao fechamento de contratos, com governança, redução de custos e mitigação de riscos.

---

## 📊 Comparativo: Procurement vs. Supply Chain

| Característica | Procurement | Supply Chain |
|---|---|---|
| **Definição** | Estratégias para abastecimento da empresa | Cadeia completa: matéria-prima → cliente |
| **Posição** | Departamento/processo dentro da Supply Chain | Estrutura logística global que engloba o Procurement |
| **Foco de Ação** | Pesquisa, avaliação, negociação, contratos | Produção, inventário, transporte, distribuição |
| **Objetivo Final** | Adquirir com melhor custo-benefício e risco controlado | Orquestrar o fluxo para o consumidor no prazo e qualidade certos |

---

## Procurement × Purchasing

A diferença central é o **nível estratégico**:

- **Procurement** — processo estratégico completo (Source-to-Pay)
- **Purchasing** (Compras) — atividade operacional focada na **execução da compra**

**Purchasing inclui:**
- Emissão de pedidos de compras
- Recebimento de insumos
- Conferência de qualidade e quantidade
- Confirmação de atendimento de prazos

São atividades **complementares**: juntas, aumentam a eficiência da cadeia de suprimentos.

---

## 📊 Comparativo: Purchasing vs. Procurement

| Característica | Purchasing (Compras) | Procurement |
|---|---|---|
| **Natureza** | Tática, operacional e transacional | Estratégica, analítica e proativa |
| **Foco de Custo** | Menor preço de aquisição | Custo Total de Propriedade (TCO) e valor agregado |
| **Relacionamento** | Transacional e de curto prazo | Parcerias estratégicas de longo prazo |
| **Escopo** | Pedidos, recebimento e pagamento (P2P) | Identificação → sourcing → negociação → gestão (S2P) |
| **Objetivo Final** | Executar a compra com eficiência e rapidez | Vantagem competitiva, mitigar riscos e maximizar ROI |

---

<!-- _class: section-title -->

# O Ciclo de Vida do Procurement

**Source-to-Pay (S2P)** — da estratégia ao pagamento

| Bloco | Descrição |
|---|---|
| **Source-to-Contract (S2C)** | Fase estratégica de planejamento e negociação |
| **Procure-to-Pay (P2P)** | Fase operacional de execução e pagamento |
| **SRM** | Gestão contínua do relacionamento com fornecedores |

O ciclo é um ecossistema complexo que vai muito além da simples aquisição de bens ou serviços.

---

## Fase 1: Source-to-Contract (S2C)
### Estratégia e Negociação

**1. Identificação e Planejamento**
- **Levantamento de Demanda:** Áreas identificam necessidades futuras de materiais e serviços
- **Análise de Spend:** Consolida histórico de gastos para ganhar alavancagem na negociação

**2. Descoberta e Qualificação de Fornecedores**
- **Análise de Mercado:** Busca de parceiros globais e locais
- **Qualificação e Onboarding:** Avaliação técnica, financeira e legal
  - Categorização em bancos de dados de grafos para mapear dependências
  - Aplicação de *resilience scoring* para antecipar rupturas

---

## Fase 1: S2C (continuação)
### Sourcing Estratégico e Contratação

**3. Sourcing Estratégico (Eventos de Cotação)**
- **RFI:** Coleta de informações sobre capacidade dos fornecedores
- **RFP / RFQ:** Solicitação formal de propostas e cotações de preços
- **Bid Analysis:** Comparação por TCO, qualidade, prazo e SLA

**4. Negociação e Contratação**
- **Negociação:** Alinhamento final de preços, prazos e condições contratuais
- **CLM (Contract Lifecycle Management):** Elaboração, revisão jurídica, assinatura digital e armazenamento do contrato que guiará as transações futuras

---

## Fase 2: Procure-to-Pay (P2P)
### Operação e Execução

**5. Requisição de Compra (PR)**
- Usuário final solicita o item ou serviço internamente
- PR passa por aprovações baseadas em orçamento e hierarquia (workflow)

**6. Ordem de Compra (PO)**
- Requisição aprovada convertida em documento legal enviado ao fornecedor
- Autoriza o envio do material ou início do serviço com base no contrato do S2C

**7. Recebimento (Goods/Services Receipt)**
- Entrega física ou digital pelo fornecedor
- Registro de entrada confirmando qualidade e quantidade conforme a PO

---

## Fase 2: P2P (continuação)
### Conciliação, Faturamento e Pagamento

**8. Conciliação e Faturamento (Invoice Processing)**
- Recebimento da Nota Fiscal no sistema
- **3-Way Match:** Cruzamento de PO + Registro de Recebimento + Nota Fiscal → libera o pagamento

**9. Pagamento**
- Fatura aprovada vai para Contas a Pagar
- Execução do pagamento ao fornecedor
- Gerenciamento estratégico do **DPO** (*Days Payable Outstanding*): equilibra retenção de caixa sem estrangular os parceiros

---

## Fase 3: Supplier Relationship Management (SRM)
### Gestão Contínua

Opera em paralelo ao S2C e ao P2P, garantindo a saúde da cadeia no longo prazo.

- **Monitoramento de Performance:** Avaliação contínua de SLAs, qualidade de entrega e índices de devolução
- **Gestão de Risco Contínua:** Monitoramento de fatores macroeconômicos, ESG e saúde financeira dos fornecedores ativos

> **Insight:** A fase de P2P — especialmente na etapa de **Conciliação e Faturamento** — é onde a automação inteligente pode gerar os maiores ganhos, especialmente no processo de **3-Way Match**.

---

## Ciclo de Vida do Procurement — Visão Geral

![Ciclo de vida do Procurement](procurement-lifecycle-1.png)

---

<!-- _class: section-title -->

# Desafios do Procurement

A área deixou de ser um mero departamento transacional para se tornar um **pilar estratégico** de sobrevivência e vantagem competitiva.

Com a instabilidade global acelerando, os desafios atuais e futuros misturam:
- Gestão de riscos
- Arquitetura de dados
- Otimização financeira

---

## Desafio 1: Visibilidade e Resiliência da Cadeia

**Situação Atual:**
A maioria das empresas só enxerga seus fornecedores **Tier 1**. Crises geopolíticas, climáticas ou sanitárias em Tier 3 ou 4 podem parar a produção sem aviso prévio.

**Fronteira Futura:**
- Mapear o ecossistema completo de fornecedores
- Adotar **bancos de dados orientados a grafos** para modelar dependências ocultas entre os elos
- Criar um ***resilience score* dinâmico** — simulando o impacto da quebra de um elo antes que ela ocorra na vida real

---

## Desafio 2: IA e Arquiteturas *AI First*

**Situação Atual:**
Volume massivo de dados não estruturados (contratos em PDF, e-mails, catálogos inconsistentes). Alto atrito operacional na criação de Requisições de Compra (PR).

**Fronteira Futura:**
- Não apenas colar RPA sobre processos ineficientes — **reconstruir o fluxo com mentalidade AI First**
- Implantar **agentes de compras especializados em IA Generativa** que:
  - Traduzem a intenção de compra para a especificação técnica correta
  - Checam orçamento disponível e validam contratos vigentes
  - Garantem compliance em tempo real, reduzindo atritos e devoluções

---

## Desafio 3: Otimização Financeira e Gestão de Caixa

**Situação Atual:**
Pressão da diretoria financeira para preservar caixa em cenários macroeconômicos voláteis e com juros elevados.

**Fronteira Futura:**
- Equilibrar o **DPO** (*Days Payable Outstanding*) de forma inteligente
- Alongar prazos melhora o balanço, mas pode **estrangular parceiros estratégicos** de menor porte
- Adotar soluções integradas de ***Supply Chain Finance*** (risco sacado inteligente):
  - Otimiza capital de giro da compradora
  - Garante oxigênio financeiro para os fornecedores da cadeia

---

## Desafio 4: Sustentabilidade e ESG Rastreável

**Situação Atual:**
Regulações globais crescentemente severas (diretrizes de *due diligence* europeias). Empresas são **legalmente corresponsáveis** pelas práticas de seus parceiros.

**Fronteira Futura:**
- Rastrear o **"Escopo 3"** de emissões de carbono (emissões indiretas da cadeia de valor)
- Captura e auditoria contínua de dados **ESG** dos fornecedores
- Compliance ambiental, social e de governança como **pré-requisito bloqueante** para onboarding e emissão de Ordens de Compra

---

## Desafio 5: Evolução do Talento — O "Novo Comprador"

**Situação Atual:**
O perfil do profissional de compras está em forte transição. O negociador tradicional focado apenas em "espremer" o preço da tabela está perdendo espaço para a tecnologia.

**Fronteira Futura:**
- Demanda por **profissionais híbridos** que combinam:
  - Nuances das negociações estratégicas de alto valor
  - Fluência tecnológica para dialogar com equipes de engenharia de software
  - Capacidade de integrar plataformas e ecossistemas complexos de S2P

---

<!-- _class: section-title -->

# Minhas Impressões: Desafios do Procurement

---

## Os 4 Principais Pontos de Atrito

**1. Dados desencontrados e falta de visibilidade**
Dados de fornecedores, contratos, pedidos e entregas espalhados em planilhas, e-mails e sistemas desconectados → atrasos, erros e risco de não conformidade.

**2. Dificuldade na Gestão de Catálogos e Fornecedores**
Falta de padronização → dificuldade de busca e comparação → risco de compras erradas e insatisfação dos usuários.

**3. Múltiplos sistemas sem integração E2E**
Silos de informação → visibilidade fragmentada → erros de comunicação e inconsistências na cadeia.

**4. Falta de visibilidade do processo como um todo**
Sem visão end-to-end → dificuldade de identificar gargalos e tomar decisões estratégicas.

**Além de:** dificuldade de encontrar fornecedores qualificados · complexidade contratual · riscos de compliance e fraudes · pressão por redução de custos

---

<!-- _class: section-title -->

# Oportunidades em Procurement

Com base no **GEP Spend Category Outlook 2026**, as oportunidades deixam de ser focadas apenas em redução de custos e passam a englobar **resiliência, inovação e estratégia**.

---

## Oportunidade 1: Expansão do Papel Estratégico

Procurement em 2026 tem um mandato muito mais amplo: transição de **"controlador de gastos"** para **"orquestrador corporativo"**.

O setor agora conecta e é responsável por:
- **Finanças** — preservação do caixa e otimização de custos
- **Cadeia de suprimentos** — resiliência e continuidade
- **Sustentabilidade** — ESG rastreável e certificado
- **Gestão de riscos** — interpretação de sinais e avaliação de exposições

---

## Oportunidade 2: Adoção Avançada de IA

A IA tornou-se parte do trabalho **diário** de Procurement:

- Melhorar previsões de demanda com modelos preditivos
- Gerenciar contratos de forma inteligente (CLM com IA)
- Orientar a demanda para fornecedores preferenciais e catálogos aprovados
- Monitorar o desempenho dos fornecedores em tempo real
- Em MRO: recomendações inteligentes para guiar demanda a catálogos aprovados

---

## Oportunidade 3: Regionalização e Resiliência

Com tarifas, pressões climáticas e fragmentação geopolítica influenciando custos:

- **Diversificar** fontes de aquisição e pegadas de produção para gerenciar exposição a tarifas
- Expandir parcerias regionais (**nearshoring**) para mitigar riscos geopolíticos
- Mudar o foco: de simples redução de custos → **construção de forças de suprimento de longo prazo**

---

## Oportunidade 4: Liderança em Sustentabilidade e ESG

- Liderar adoção de padrões de **compras verdes**
- Integrar metas de circularidade e materiais recicláveis nas decisões de sourcing
- Conformidade com emissões de carbono (ex: Mecanismo de Ajuste de Fronteira de Carbono — Europa)
- ESG integrado como critério em **todas as decisões de sourcing** e contratos de fornecedores

---

## Oportunidade 5: Novos Modelos de Contratação

**Contratos baseados em resultados**
- Substituem "tempo e material" por entregas atreladas a **KPIs claros** e valor quantificável
- Especialmente relevante em serviços profissionais e consultoria

**Modelos de assinatura e consumo**
- Em categorias como software: mudança para modelos recorrentes e escaláveis
- Exige que Procurement gerencie contratos de forma mais fluida e adaptável

> Em resumo, a maior oportunidade para Procurement em 2026 é atuar como uma **ponte estratégica** que equilibra controle de custos, inovação, conformidade e estabilidade da cadeia frente a disrupções globais.

---

## Oportunidades Técnicas: Arquitetura e IA

**6. Desacoplamento e Arquiteturas "Composables"**
- Transição de ERPs monolíticos para **arquiteturas composable**
- *Domain-Driven Design* (DDD) por contexto: Sourcing, Gestão de Contratos, P2P
- Microsserviços com princípios SOLID → soluções **best-of-breed** sem quebrar o ecossistema principal

**7. Agentes de Compras e Interfaces AI First**
- Substituir formulários por **agentes conversacionais baseados em IA Generativa**
- Usuário descreve a necessidade em linguagem natural → IA traduz para PR correta
- Compliance nativo desde a origem, reduzindo retrabalho e devoluções

---

## Oportunidades Técnicas: Risco e Financeiro

**8. Inteligência de Risco Baseada em Grafos**
- Bancos de dados orientados a grafos para mapear dependências ocultas na cadeia
- **Resilience score** preditivo: alertas em tempo real sobre fornecedores secundários em risco
- Permite acionar rotas alternativas *antes* da ruptura

**9. Otimização Dinâmica do Ciclo Financeiro (DPO)**
- Programas de **desconto dinâmico** e antecipação de recebíveis com base em dados
- Compradora ganha fôlego no caixa; pequeno fornecedor recebe oxigênio antecipado

> **Insight principal:** As maiores oportunidades exigem que Procurement se torne um ecossistema fluído, onde **engenharia de software** e **inteligência de negócios** atuam juntas.

---

## Visão da Transformação

![Transformação do Procurement](procurement-transformation-1.png)

---

<!-- _class: section-title -->

# Principais KPIs de Procurement

Os KPIs (Key Performance Indicators) evoluíram: hoje medem muito **mais do que redução de preço**. São métricas essenciais para avaliar eficiência, eficácia e impacto estratégico do departamento de compras.

---

## KPIs Financeiros e Operacionais

| KPI | O que mede |
|---|---|
| **Economia Direta (Saving)** | Diferença entre valor pago e preço histórico/orçado |
| **Custo Evitado (Cost Avoidance)** | Valor deixado de gastar por negociação que evitou aumento |
| **Gasto sob Gestão** | % dos gastos ativamente negociados e controlados pelo time |
| **Tempo de Ciclo e Lead Time** | Velocidade da operação — emissão de pedido → entrega |
| **Taxa de Defeitos do Fornecedor** | Qualidade das entregas (itens com problemas anulam o saving) |
| **Taxa de Compras Emergenciais** | Volume de pedidos urgentes (indicam falhas no planejamento) |
| **Índice de Conformidade (Compliance)** | % de compras que seguem políticas e contratos estabelecidos |
| **Satisfação dos Clientes Internos** | Percepção das áreas de negócio sobre o serviço de compras |

---

## KPIs Estratégicos e ESG

| KPI | O que mede |
|---|---|
| **Diversidade de Fornecedores** | % de gastos com fornecedores diversos (MPEs, minorias, etc.) |
| **Índice de Sustentabilidade** | % de compras que atendem critérios ambientais responsáveis |
| **Índice de Inovação** | % de gastos com fornecedores de soluções inovadoras e disruptivas |
| **Parcerias Estratégicas** | % de gastos com parceiros de longo prazo e colaborativos |
| **Risco do Fornecedor** | Exposição a instabilidades financeiras, de compliance e operacionais |
| **Resiliência da Cadeia** | Capacidade de recuperação rápida a interrupções e crises |
| **Índice de Automação** | % de processos automatizados — indicador de adoção tecnológica |

---

<!-- _class: section-title -->

# Tendências Futuras em Procurement

O papel da área evolui de **departamento de suporte** para **pilar estratégico** de sobrevivência e vantagem competitiva, impulsionado por IA, novos modelos organizacionais e ecossistemas inteligentes.

---

## A IA em 5 Domínios Agênticos

| Domínio | Agente | Função |
|---|---|---|
| **A. Estratégia** | Agente de estratégia | Analisar gastos, gerar ideias de saving, otimizar estratégia |
| **B. Sourcing** | Agente de sourcing | Automatizar go-to-market: descoberta → geração de RFX |
| **C. Negociação** | Agente de negociação | Negociar e colaborar com fornecedores (incl. long tail) |
| **D. Operações** | Agente operacional | Gerenciar transações e desempenho de fornecedores |
| **E. Valor** | Agente de preservação | Garantir condições contratadas e compliance em faturas |

**Impacto esperado:** +30% de eficiência · 5–10% de economias liberadas de gastos

---

## Legenda de Autonomia

| Nível | Símbolo | Descrição |
|---|---|---|
| **Totalmente autônomo** | **(T)** | Apenas supervisão Human-In-The-Loop (HITL) |
| **Parcialmente autônomo** | **(P)** | IA executa com validação humana pontual |
| **Tarefa humana essencial** | **(E)** | Humano lidera, com suporte de IA agêntica |

---

## IA por Processo — Nível de Autonomia

| Gestão de demanda | Sourcing estratégico | Gestão de Contratos | Compra a Recebimento | Fatura a Pagamento | Gestão de fornecedores | Excelência |
|---|---|---|---|---|---|---|
| Avaliação de tendências **(T)** | Cubo de gastos **(P)** | Elaboração de contratos **(T)** | Identificação da necessidade **(T)** | Recebimento de faturas **(T)** | Informações de fornecedores **(T)** | Governança **(E)** |
| Avaliação de demanda **(E)** | Estratégia de categoria **(E)** | Repositório de contratos **(P)** | Compras spot **(T)** | Conciliação de faturas **(P)** | Desempenho de fornecedores **(T)** | Conformidade **(E)** |
| | Identificação de oportunidades **(T)** | Desempenho de contratos **(T)** | Criação de PO **(P)** | Gestão de exceções **(T)** | Riscos de fornecedores **(T)** | Processos **(E)** |
| | Descoberta de fornecedores **(T)** | | Agilização **(T)** | Processamento de pagamentos **(P)** | Relacionamento com fornecedores **(E)** | Gestão de desempenho **(E)** |
| | Evento RFx **(T)** | | Recebimento de mercadorias **(P)** | | Inovação **(E)** | |
| | Negociações **(E)** | | | | | |

---

## 🧩 Organização de Compras do Futuro

| Arquétipo | Objetivo | Natureza | Papel na arquitetura |
|---|---|---|---|
| **A. Núcleo Estratégico** | Definir estratégia de compras | Humano + IA | Define regras, políticas e direcionamento |
| **B. Equipes de Especialistas** | Complementar agentes com expertise | Humano-in-the-loop | Trata exceções, validações e decisões complexas |
| **C. Rede Agentiva** | Execução autônoma em escala | IA-first | Execução operacional (automação distribuída) |
| **D. CoE de IA** | Governança e adoção de IA | Governança | Define padrões, modelos, segurança e compliance |

---

## 🧭 Jornada de 5 Anos — Evolução para Função Agêntica

| Hoje (As-is) | Horizonte ~1–2 anos | Horizonte ~2–3 anos |
|---|---|---|
| Alta automação transacional com aplicações iniciais de GenAI | **Agentes isolados** gerenciam fluxos individuais com supervisão humana direcionada | **Rede agentiva** impulsiona execução autônoma, com humanos orquestrando resultados de negócio |
| Stack S2P em nuvem integrado ao ERP — execução **humana** dos fluxos | **Pool flexível de especialistas** alocados às equipes de categoria por sprint | **Especialistas como HITL** validando saídas agentivas com julgamento contextual |
| Gestão de categorias clara e consistente, atividades centrais e locais equilibradas | **Força de trabalho híbrida** humanos + agentes; foco em pensamento crítico | **Núcleo estratégico evoluído**: foco em fornecedores-chave e profundidade técnica |

---

## 🧠 Tradução para Arquitetura Técnica

| Estágio | Arquitetura dominante |
|---|---|
| **Hoje** | APIs + workflows + automação tradicional |
| **1–2 anos** | Microsserviços + workers + primeiros agentes (task-level) |
| **2–3 anos** | **Event-driven + agent mesh + orchestration híbrida (human + IA)** |

---

## 🤖 AI-driven Engagement — Evolução de Experiências

| Fase | Conceito | Papel da IA |
|---|---|---|
| **Seamlessly Embedded AI** | IA integrada de forma transparente nos processos | IA como capability transversal |
| **Agent-driven Autonomous** | Agentes autônomos em nível de função ou cross-domínio | IA como executor |
| **AI-native Applications** | Aplicações construídas com arquitetura AI-first | IA como foundation |
| **App-less Experiences** | Experiências sem apps tradicionais (chat, comandos) | IA como interface |
| **No-apps Experiences** | Experiências e apps gerados em tempo real por IA | IA como plataforma |

---

<!-- _class: section-title -->

# Conclusão e Recomendações

O Procurement está passando por uma **transformação profunda**, impulsionada por avanços tecnológicos e mudanças nas expectativas dos clientes.

As oportunidades vão muito além da redução de custos:
- **Resiliência** da cadeia de suprimentos frente a disrupções globais
- **IA e automação** inteligente nos processos de ponta a ponta
- **Regionalização** como estratégia de continuidade operacional
- **Liderança em sustentabilidade** com ESG rastreável e auditável
- **Talentos híbridos** capazes de navegar no novo cenário complexo e dinâmico

Para aproveitar essas oportunidades, as empresas precisam:
1. Repensar suas **estratégias** de compras com visão de longo prazo
2. Investir em **tecnologia** de forma planejada e orientada a valor
3. Desenvolver **talentos híbridos** para o ecossistema inteligente e dinâmico
