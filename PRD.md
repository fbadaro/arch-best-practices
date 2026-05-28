# Product Requirements Document (PRD)
**Projeto:** Arquitetura de Agentes AI-First
**Domínio Principal:** Plataforma de Engenharia / Domain-Driven Design (DDD)
**Objetivo:** Estruturar um ecossistema de agentes autônomos (LLMs) para atuar como Especialistas de Domínio e Arquitetos de Software, separando o Espaço do Problema (Intenção) do Espaço da Solução (Legado) para governar a evolução de sistemas corporativos complexos.

---

## 1. Visão Geral do Produto e Topologia
O sistema resolve a falha crítica de LLMs genéricas que absorvem "dívida técnica" ao analisar repositórios corporativos (*Brownfield*). A arquitetura é dividida em três zonas, governadas por 5 agentes específicos.

1. **Zona Corrompida (Mundo Real):** Onde residem o código as-is, os bancos de dados e os vieses humanos.
2. **A Fronteira (Gatekeeper):** A aduana de reconciliação que audita o conhecimento através de *Ontology Pull Requests (OPRs)*.
3. **Zona Pura (Golden Source):** O santuário arquitetural contendo a ontologia perfeita do negócio, livre de restrições tecnológicas.

---

## 2. Especificação dos Agentes (Instruções para GitHub Copilot)

### 2.1. `the-code-digger` (Analista do Espaço da Solução)
* **Missão:** Fazer engenharia reversa do "As-Is" físico. Ele é cego para o negócio e documenta apenas restrições, acoplamentos e a nomenclatura exata (frequentemente tóxica) da infraestrutura.
* **Inputs:** Repositórios de código (AST), esquemas SQL relacionais, logs de execução.
* **Outputs (Artefatos DDD):**
    * *As-Is Context Map:* Mapa de integrações físicas atuais.
    * *Topologia de Módulos:* Agrupamentos físicos e *namespaces*.
    * *Physical Aggregates:* Exposição de *God Classes* e Domínios Anêmicos.
    * *Léxico de Implementação:* Vocabulário literal do código (ex: `Tb_Fornec`, `CalcDPO`).

### 2.2. `the-business-digger` (Etnógrafo do Espaço do Problema)
* **Missão:** Extrair a intenção estratégica e a operação humana. Ele é cego para o software e foca estritamente no jargão do domínio e na geração de valor.
* **Inputs:** Transcrições de reuniões (EventStorming), políticas corporativas, manuais de operações.
* **Outputs (Artefatos DDD):**
    * *Subdomain Map:* Identificação e classificação de capacidades (*Core, Supporting, Generic*).
    * *Event/Command Catalog:* Linha do tempo de fatos de negócio (ex: `RupturaRegistrada`).
    * *Glossário Bruto do Negócio:* A linguagem natural falada pelos especialistas (ex: "Parceiro Comercial").

### 2.3. `the-lexicon-weaver` (O Juiz da Fronteira)
* **Missão:** Atuar como *Gatekeeper* entre a Zona Corrompida e a Zona Pura. Ele confronta as saídas dos dois *diggers*, detecta anomalias linguísticas e forja a Pedra de Roseta do sistema.
* **Mecânica Crítica:** * Caça *falsos cognatos* e colapsa *sinônimos descontrolados*.
    * Compara novas entradas com o estado atual da *Golden Source*.
    * Se detectar uma mutação estrutural nas regras, não atualiza a *Golden Source* diretamente; ele gera um **Ontology Pull Request (OPR)** para aprovação humana (avaliando o "raio de explosão" via travessia de grafos).
* **Output (Artefato DDD):** * *Dicionário de Linguagem Ubíqua:* Tabela mapeando Termos Oficiais, Definições e Termos Tóxicos (a serem banidos).

### 2.4. `the-context-mapper` (O Arquiteto de Software)
* **Missão:** Desenhar a arquitetura To-Be consumindo o Léxico Oficial e a Intenção do Negócio.
* **Inputs:** O output aprovado do `the-lexicon-weaver`.
* **Outputs (Artefatos DDD):**
    * *To-Be Context Map:* Fronteiras lógicas dos novos microsserviços.
    * *Agregados Táticos:* Definição de raízes de agregação para o *Core Domain*.
    * *Anti-Corruption Layers (ACL):* Especificações estritas de como o novo contexto deve traduzir dados ao ler/gravar na base mapeada pelo `the-code-digger` (Estrangulamento do Legado).

### 2.5. `the-storyteller` (O Orquestrador de Execução)
* **Missão:** Transformar os desejos de negócio (Produto) em *User Stories* acionáveis para a engenharia, garantindo que o código a ser escrito respeite a *Golden Source* e contorne corretamente o legado.
* **Mecânica de Triangulação:**
    1. Lê a intenção crua do PM.
    2. Consulta a *Golden Source* para traduzir a intenção para a Linguagem Ubíqua.
    3. Consulta o `the-code-digger` para saber quais classes e tabelas do sistema atual serão impactadas.
* **Outputs (Agile Artifacts):**
    * *User Stories Purificadas:* Escritas exclusivamente com a Linguagem Ubíqua.
    * *Execution Plan (Tasks):* Fatiado obrigatoriamente entre **Tarefas Core** (lógica pura de domínio) e **Tarefas de Fronteira** (implementação de ACLs baseadas na topologia real corrompida).
    * *Critérios de Aceite (BDD):* Estrutura Given-When-Then amarrada aos eventos de domínio.

---

## 3. Fluxo de Trabalho (Loop Contínuo de Integração)

| Etapa | Ação no Sistema | Agente Envolvido |
| :--- | :--- | :--- |
| **1. Extração** | Leitura simultânea do ecossistema legado e dos normativos do negócio. | `the-code-digger`, `the-business-digger` |
| **2. Reconciliação** | Resolução de conflitos, bloqueio de vieses tóxicos e geração do Léxico. | `the-lexicon-weaver` |
| **3. Governança** | Aprovação humana de eventuais mutações no domínio (OPR). Aprovado, atualiza o grafo via Event Sourcing. | `Human-in-the-Loop` |
| **4. Arquitetura** | Atualização da topologia To-Be e projeção de ACLs para as áreas impactadas. | `the-context-mapper` |
| **5. Execução** | Conversão de *features* em tarefas técnicas de domínio (Core) e infraestrutura (ACL). | `the-storyteller` |
| **6. Atualização** | O código é implantado. O estado da aplicação muda. O ciclo reinicia (Passo 1). | `CI/CD Trigger` |

---

## 4. Requisitos Não Funcionais Críticos
* **Contratos de Comunicação:** A comunicação entre os agentes ocorrerá via artefatos Markdown estruturados para garantir rastreabilidade analítica.
* **Isolamento do Motor de Grafo:** A *Golden Source* deve ser armazenada utilizando um Graph Database (GraphRAG) para permitir análises precisas de dependência de contexto quando o `the-lexicon-weaver` for aprovar OPRs.
* **Restrição de System Prompts:** O agente de código deve ser restrito (via prompt) para *não deduzir* regras de negócio, e o agente de negócio restrito para *ignorar limitações* técnicas.