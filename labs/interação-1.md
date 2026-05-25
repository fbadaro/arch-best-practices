## 1) O que é “montar o cérebro” de uma LLM especialista de domínio?

Pense no “cérebro” como um conjunto de camadas:

### Camada A — **Conhecimento (Domain Source of Truth)**

Um repositório “vivo” de **artefatos de DDD** que representam o domínio com rastreabilidade:

* **Ubiquitous Language / Visual Glossary** (termos, definições, sinônimos proibidos, exemplos) [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
* **Domain Stories / Domain Storytelling** (como o negócio realmente opera) [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
* **EventStorming / Domain Events** (eventos, comandos, políticas, regras) [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
* **Context Map** (bounded contexts e relações/padrões de integração) [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
* **Especificações** (OpenAPI/AsyncAPI, schemas de eventos, contratos) [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
* **Exemplos executáveis** (Example Mapping / cenários BDD) [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
* **ADRs e decisões arquiteturais** (o “porquê” do design)

👉 Isso é crucial porque melhora a qualidade de output da IA conforme os artefatos ficam mais ricos, formando uma “esteira” de especificação → design → código. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

***

### Camada B — **Grounding (RAG + estrutura)**

Em vez de “treinar” a LLM para saber o domínio, você **ancora** as respostas nesses artefatos:

* **RAG semântico** para buscar trechos relevantes (glossário, eventos, contratos, ADRs).
* **Estrutura por tipo de artefato** (ex.: priorizar glossário para termos; priorizar eventos para comportamento; priorizar contratos para integração).
* (Opcional) **Grafo de conhecimento** leve: termos ↔ eventos ↔ agregados ↔ contextos ↔ endpoints.

📌 Um ganho enorme aqui é usar IA para **detectar inconsistências** entre artefatos (ex.: termo do glossário divergindo do campo no contrato; nomes diferentes para o mesmo conceito). Isso é algo que IA faz bem: comparar artefatos em escala e apontar drift. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

***

### Camada C — **Raciocínio orientado a tarefa (Agents/papéis)**

Você não quer “um chat inteligente”; quer um **time de agentes** com responsabilidades claras. Exemplo:

1. **Agent de Modelagem (DDD)**
   * Propõe eventos, termos, bounded contexts (como *drafts/provocações*, não verdades). [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

2. **Agent de Consistência / Governança**
   * Valida: nomes, duplicidades, drift de termos, alinhamento com contratos e eventos. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

3. **Agent de Código / Brownfield**
   * Mapeia “onde no código” vivem regras e fluxos que correspondem a eventos/termos (mais abaixo eu detalho).

4. **Agent de Integração**
   * Gera/valida contratos, eventos, políticas de versionamento, padrões do Context Map.

5. **Agent de Exemplos (BDD)**
   * Converte regras em exemplos, cenários e critérios de aceitação (e ajuda a testar).

***

### Camada D — **Avaliação (Evals) com experts e método híbrido**

Esse é o ponto que mais diferencia “parece especialista” vs “é confiável”.

Práticas que funcionam bem na vida real:

* **Co-criar critérios de avaliação com domain experts** (rubricas/heurísticas do que é “boa resposta” naquele domínio). [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)
* Usar **avaliação híbrida**: expert + dev + LLM-as-a-judge (com cautela e calibragem). [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)
* Reconhecer limitações: experts cansam com tarefas repetitivas; é preciso desenhar um fluxo sustentável (amostragens, sessões curtas, automação onde dá). [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)
* Tratar **consentimento/posse do conhecimento**: experts podem ter receio de “entregar” know-how; isso precisa ser explícito. [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)

***

## 2) Greenfield: como “montar o cérebro” quando o sistema ainda não existe

Aqui você tem um superpoder: pode começar correto, usando a “esteira DDD → especificação → software”.

### Fluxo recomendado (muito direto ao ponto)

1. **Descoberta e limites iniciais**
   * Capability map, visão, objetivos.
2. **Domain Storytelling + Glossário**
   * Captura o jeito real do negócio operar e cria linguagem comum. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
3. **EventStorming + Event Modeling**
   * Revela eventos, regras e candidatas a fronteiras. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
4. **Context Map**
   * Define relações e padrões de integração. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
5. **Contratos (OpenAPI/AsyncAPI) e schemas**
6. **Tactical design por contexto**
   * Agregados, entidades, VOs, invariantes.
7. **Example Mapping e testes**
8. **Implementação + documentação**

O papel da IA no greenfield:

* **Quebrar o “cold start”**: gerar um primeiro rascunho de eventos, mapa de capacidades, decomposição inicial — como provocação para o time reagir mais rápido. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
* **Acelerar o pipeline de artefatos**: quanto melhores os artefatos, melhor o output; eles alimentam o RAG, os contratos e os testes. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
* **Validar consistência** do glossário ↔ contratos ↔ eventos ↔ contexto. [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)

✅ Resultado: a LLM “vira especialista” porque **ela sempre responde com base no seu conjunto de artefatos**, e você evita que “opiniões” virem “verdades”.

***

## 3) Brownfield: como “montar o cérebro” quando o sistema já existe

No brownfield, o problema é **descobrir o domínio escondido no legado** (código, integrações, tabelas, tickets, regras implícitas).

### Estratégia: “As-Is → domínio → To-Be”

Você cria um ciclo de engenharia reversa guiado por DDD:

**Passo 1 — Ingestão do legado**

* Código, docs, ADRs (se existirem), APIs, eventos, esquemas de BD, incidentes/bugs, histórias/tickets.

**Passo 2 — Extração do domínio (agentic)**

* Agent de Código identifica:
  * **Vocabulário no código** (nomes de classes, endpoints, mensagens) → candidatos a termos do glossário.
  * **Regras** (if/else, validações, constraints) → candidatos a invariantes de agregados.
  * **Fluxos** (sagas/workflows) → candidatos a processos/políticas e eventos.

**Passo 3 — Reconciliação com experts**

* Você valida o que o legado “faz” vs o que o negócio “quer”.
* Aqui, avaliação e rubricas com experts são críticas: os devs não são experts do domínio e tendem a “normalizar” bizarrices do legado como se fossem regra de negócio. A pesquisa de práticas reais mostra o quanto é importante co-desenhar critérios e usar avaliação híbrida quando o tempo do expert é limitado. [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)

**Passo 4 — Context Map As-Is**

* Mapeia bounded contexts reais (mesmo que bagunçados) e integrações.

**Passo 5 — Plano de modernização**

* Definir onde extrair contextos (strangler fig, anti-corruption layer, etc.)
* Criar contratos novos com backward compatibility.

👉 Aqui a IA ajuda MUITO em duas coisas já comprovadamente úteis:

* **Comparar artefatos e apontar inconsistências/drifts** (ex.: nomes diferentes para o mesmo conceito em APIs distintas). [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
* **Criar critérios e rotinas de avaliação com experts** para manter o ciclo sustentável (sem esgotar o expert). [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)

***

## 4) Uma arquitetura agêntica “template” (funciona para os dois cenários)

### Componentes (visão de alto nível)

**1) Domain Artifact Store (fonte de verdade)**

* Glossário (termos + exemplos + sinônimos proibidos)
* Eventos, context map, contratos, ADRs, exemplos BDD

**2) Ingestion Pipeline**

* Normaliza, versiona e “tagueia” por contexto/tipo

**3) Retrieval (RAG)**

* Índice por tipo + ranking por prioridade (glossário > eventos > contratos > ADR)

**4) Orchestrator (Agent Router)**

* Classifica intenção (modelagem? código? integração? regra? teste?)
* Chama o agente adequado

**5) Deterministic Guards**

* Validadores de nomenclatura, consistência de termos, checagem de schema, lint de contratos

**6) Eval Harness**

* Rubricas do domínio + testes de regressão de respostas
* Avaliação híbrida (amostragem humana + LLM judge calibrado) [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)

***

## 5) “RAG primeiro, fine-tuning depois” (e por quê)

Para virar especialista, **geralmente você não precisa fine-tune logo de cara**. Em domínios corporativos, a maior dor é **acesso e atualização do conhecimento**.

Um padrão observado em times reais: quando dados de experts são limitados, eles recorrem a **augmentação/sintético**, mas isso traz trade-offs (qualidade, alinhamento, governança).   
Então o caminho mais seguro costuma ser: [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)

1. **RAG + artefatos fortes (DDD)**
2. **Evals + rubricas + correções rápidas nos artefatos**
3. Só então considerar **fine-tuning** para estilo, formato, ou “regras de redação” — não para ser “a fonte do domínio”.

***

## 6) Checklist prático para você começar (sem depender de “mágica”)

### Se for Greenfield (ordem ideal)

1. Criar **Glossário inicial** (mesmo que imperfeito) [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
2. Rodar **Domain Storytelling** e **EventStorming** [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
3. Definir **Context Map** e padrões de integração [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
4. Gerar contratos e exemplos
5. Conectar tudo ao **RAG**
6. Criar **validador de consistência** (termos/contratos/eventos) [\[codecentric.de\]](https://www.codecentric.de/en/knowledge-hub/blog/the-synergetic-blueprint-revisited-and-why-ai-changes-everything)
7. Definir **rubricas de qualidade com experts** [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)

### Se for Brownfield

1. Ingestão de código/APIs/BD/tickets
2. Extração de **termos + regras + eventos candidatos**
3. Montar **As-Is context map**
4. Validar com experts e registrar divergências (legado vs regra de negócio) [\[arxiv.org\]](https://arxiv.org/pdf/2602.14357)
5. Criar **ACL/strangler** e começar a “limpar o domínio” incrementalmente
6. Evals contínuos para não “regredir” entendimento

***

## Uma única pergunta (pra eu te dar um blueprint bem “no seu contexto”)

Qual é o **domínio de software** que você quer especializar (ex.: crédito, logística, manufatura, IAM, billing) e, no brownfield, quais artefatos você já tem mais disponíveis (**código**, **OpenAPI**, **eventos**, **tabelas**, **tickets**, **docs**)?

Se você responder isso em uma linha, eu te devolvo um desenho de agentes + artefatos mínimos + fluxo de ingestão/RAG/evals já bem “pé no chão” para o seu cenário.
