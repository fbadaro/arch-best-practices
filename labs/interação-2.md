## 1) Princípio-mãe: histórias boas exigem **especificação**, não “criatividade”

O que costuma dar errado em IA gerando histórias é a mesma coisa que dá errado em IA gerando código: ela inventa detalhes “plausíveis” porque **não tem acesso ao seu domínio**. O remédio não é prompt mais esperto — é **melhor especificação e artefatos de modelagem** (Domain Storytelling, EventStorming etc.) alimentando o cérebro do sistema. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/from-stories-to-code-how-domain-storytelling-and-eventstorming-give-llms-the-context-they-need), [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

Então o seu “Story Agent” tem que ser **grounded** nos artefatos do “cérebro especialista” (UL/glossário, eventos, context map, contratos, exemplos).

***

## 2) Desenho do “Story Agent” por papéis (Drafter / Validator / Provocateur)

Use explicitamente os 3 papéis (isso reduz muito comportamento errático):

### 2.1 Drafter (rascunha a história)

Gera uma primeira versão **concreta o suficiente para estar errada** — isso acelera refinamento. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/ai-as-a-design-partner-drafter-validator-provocateur)

### 2.2 Validator (valida consistência e impacto)

Checa alinhamento com o que “já é verdade” no domínio: termos, eventos, bounded contexts, contratos, decisões. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/ai-as-a-design-partner-drafter-validator-provocateur), [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

### 2.3 Provocateur (confronta decisões e riscos)

Faz as perguntas difíceis: “isso cria novo contexto?”, “mistura linguagem?”, “quebra integração?”, “introduz conceito ambíguo?”. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/ai-as-a-design-partner-drafter-validator-provocateur)

> Importante: a própria Junker ressalta que **não dá para o mesmo ator fazer tudo ao mesmo tempo** (draftar e provocar simultaneamente); separar papéis deixa mais previsível o que esperar e o que revisar. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/ai-as-a-design-partner-drafter-validator-provocateur)

***

## 3) Pipeline de decisão (o coração do seu caso de uso)

A sua necessidade (“dada uma história, identificar subdomínio/BC/UL…”) vira um pipeline determinístico com checkpoints.

### Etapa 0 — Intake estruturado (entrada da história)

A entrada pode ser:

* ideia solta (“precisamos permitir cancelamento…”)
* história já escrita
* bug/incident
* solicitação técnica

**Normalize** para um formato canônico (campos obrigatórios) antes de qualquer LLM, para reduzir ambiguidade (ex.: objetivo, ator, motivação, resultado, regras, integrações). Isso é essencial porque “ambiguidade é o inimigo”. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/from-stories-to-code-how-domain-storytelling-and-eventstorming-give-llms-the-context-they-need)

***

### Etapa 1 — Classificação: subdomínio / capability / BC candidato

Aqui tem um insight valioso do Eric Evans ao integrar IA em sistemas determinísticos: **separe tarefa de classificação (onde LLM é forte) de tarefa de modelagem (onde LLM tende a alucinar)**. [\[domainlanguage.com\]](https://www.domainlanguage.com/articles/context-mapping-an-ai-based-component/)

Então faça assim:

**1A. Classifier Agent (LLM forte)**

* Classifica a história em:
  * subdomínio/capability
  * bounded context mais provável (entre os existentes)
  * “unknown/novo” quando não encaixa

**1B. Evidence pack (RAG)**

* Para cada candidato, puxa evidências:
  * termos do glossário usados ali
  * eventos daquele contexto
  * APIs/eventos/contratos
  * ADRs relevantes

> Esse “pacote de evidências” é o que impede a IA de “chutar”. (E bate com a ideia de que IA falha porque não tem acesso ao domínio; você dá acesso via artefatos.) [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/from-stories-to-code-how-domain-storytelling-and-eventstorming-give-llms-the-context-they-need)

**Saídas possíveis da etapa 1:**

* ✅ “Encaixa em BC X (confiança alta)”
* ⚠️ “Toca BC X e BC Y (risco de boundary leak)”
* 🆕 “Não encaixa — candidato a novo BC ou revisão do mapa”

***

### Etapa 2 — Análise de impacto em Bounded Context (novo BC? ou só feature?)

Crie um **Context Impact Analyzer** que aplica heurísticas DDD:

**Sinais de “talvez novo BC”:**

* linguagem nova com significado próprio e regras fortes
* lifecycle e consistência diferentes
* integrações novas com sistemas externos
* múltiplas equipes/owners interessados
* o fluxo vira “plataforma” para outras features

Aqui o **Provocateur** entra com perguntas e cenários, mas a decisão final é humana.

E registre o resultado como um artefato:

* “BC atual”
* “BC atual + colaboração com outro (definir relação no context map)”
* “novo BC candidato (discovery needed)”

Context mapping é justamente o artefato para representar esses limites e relações. [\[domainlanguage.com\]](https://www.domainlanguage.com/articles/context-mapping-an-ai-based-component/), [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

***

### Etapa 3 — Detecção de nova linguagem ubíqua (UL drift / termos novos / conflitos)

Você quer que o agente identifique:

* termos desconhecidos
* sinônimos perigosos
* termo igual com significado diferente em contextos diferentes

Isso é um ponto onde IA agrega muito: comparar artefatos e apontar inconsistências em escala. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

**UL Drift Detector (Validator Agent)**

* extrai termos-chave da história
* consulta glossário:
  * existe? (sim/não)
  * existe com outro nome? (sinônimo)
  * existe com definição divergente em outro BC? (conflito semântico)

**Saídas:**

* ✅ “Termos ok”
* ➕ “Sugere adicionar termo ao glossário” (+ template de definição + exemplos)
* ⚠️ “Conflito: termo X significa A no BC1 e B no BC2 → precisa qualificar”

***

### Etapa 4 — Refinamento da história (Story slicing + critérios de aceitação)

Com BC/UL mais claros, o **Drafter** gera histórias refinadas:

* 1 épico → N histórias fatiadas (vertical slices)
* cada história com:
  * valor
  * regras
  * exemplos
  * dependências
  * riscos/decisões

Esse passo deve puxar regras e eventos do contexto para não inventar domínio. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/from-stories-to-code-how-domain-storytelling-and-eventstorming-give-llms-the-context-they-need), [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

***

### Etapa 5 — Confronto de decisões (Decision Challenger / Provocateur)

Aqui é onde você “confronta decisões” como você descreveu.

O agente cria um bloco tipo:

* “Decisões impactadas”
* “Assunções novas”
* “Perguntas ao PO/Especialista”
* “Alternativas e trade-offs”

Ele pode inclusive apontar:

* se a história viola regra existente (ex.: evento/contrato)
* se contradiz ADR
* se cria acoplamento indevido entre BCs

Separar isso num papel de **Provocateur** é exatamente o padrão sugerido (nomear o papel para saber o que esperar e como revisar). [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/ai-as-a-design-partner-drafter-validator-provocateur)

***

## 4) Formato de saída “industrial” (para integrar em Jira/Azure DevOps)

Sugestão de output **sempre estruturado** (JSON/YAML) + versão human-friendly.

### 4.1 Saída estruturada (exemplo de campos)

* `classification`:
  * `subdomain`
  * `bounded_context`
  * `confidence`
  * `evidence_links` (artefatos usados)
* `impact`:
  * `touches_other_contexts`
  * `new_bc_candidate` (true/false)
  * `context_map_change_suggestion`
* `ubiquitous_language`:
  * `new_terms[]`
  * `conflicts[]`
  * `glossary_patch[]`
* `stories[]`:
  * `title`
  * `as_a / i_want / so_that`
  * `rules[]`
  * `acceptance_examples[]` (Given/When/Then)
  * `non_functional[]`
  * `dependencies[]`
* `provocations[]` (perguntas e trade-offs)

Isso vira “contrato” entre agentes e ferramentas (determinístico).

***

## 5) Guardrails: como evitar o agente “viajar” (muito importante)

Três proteções práticas:

### 5.1 “Evidence-first”

O agente só pode afirmar algo se vier do **evidence pack** (artefatos). Se não achar evidência, ele deve marcar como **hipótese**.

Isso ataca o problema raiz (modelo sem acesso ao domínio) descrito no “From Stories to Code”. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/from-stories-to-code-how-domain-storytelling-and-eventstorming-give-llms-the-context-they-need)

### 5.2 Separar classificação de modelagem

De novo, seguindo o Evans: classificação (LLM forte) ≠ modelagem/decisão (precisa guardrails e validação). [\[domainlanguage.com\]](https://www.domainlanguage.com/articles/context-mapping-an-ai-based-component/)

### 5.3 Avaliação contínua com experts (híbrida)

Para o “Story Agent” ficar bom de verdade, você vai precisar de um loop de avaliação:

* rubricas co-criadas com especialistas
* avaliação híbrida (especialista + dev + LLM judge com critério)
* cuidado com fadiga e custo de especialistas

Essas práticas aparecem como padrão em estudo real de desenvolvimento de LLM com especialistas: co-desenhar critérios e adotar avaliação híbrida por restrição de tempo/recursos. [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)

***

## 6) Onde isso encaixa na “esteira” DDD → software

Esse “Story Agent” é praticamente um “módulo” dentro de uma esteira end-to-end (ideação → requisitos → solution design → tático → código), que é exatamente a lacuna que o Synergetic Blueprint tenta fechar: sair do workshop/descoberta e chegar em software com rastreabilidade. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

***

## 7) Uma pergunta (única) para eu fechar o desenho com mais precisão

Quando esse agente “criar histórias”, ele deve **apenas sugerir** (e abrir PR/rascunho) ou ele vai **publicar automaticamente** no Jira/Azure DevOps com estados/labels?

> Isso muda o nível de guardrails, aprovação humana e auditabilidade que você precisa no fluxo. [\[domainlanguage.com\]](https://www.domainlanguage.com/articles/context-mapping-an-ai-based-component/)
