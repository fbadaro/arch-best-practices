# Teoria de Design Estratégico DDD

> **Propósito**: Este documento fornece um resumo condensado e otimizado para LLM dos princípios de Design Estratégico do Domain-Driven Design. Use isso como base teórica para identificação de domínios e análise de limites.

## Definições Fundamentais

### Domínio

Um **domínio** no contexto de Domain-Driven Design (DDD) é um conceito fundamental que representa a esfera de conhecimento, influência ou atividade sobre a qual uma pessoa ou equipe tem controle.

- **Definição de Negócio**: Todo o reino das operações comerciais, know-how e métodos
- **Definição Técnica**: O espaço do problema que o software está sendo construído para resolver
- **Insight Chave**: O termo "domínio" é sobrecarregado - pode se referir ao domínio de negócio inteiro OU uma área específica (Domínio Principal, Subdomínio)

```
Domínio = Reino de negócio da organização + Como ela opera + Seu conhecimento único
```

### Subdomínio

Um **subdomínio** no contexto de Domain-Driven Design (DDD) é uma parte específica e focada de um domínio maior, representando uma área distinta de conhecimento ou responsabilidade dentro do espaço do problema.

#### Definição Fundamental de Subdomínio

Como explica Vaughn Vernon em Domain-Driven Design Distilled, "um Subdomínio é uma sub-parte de seu domínio geral". Você pode pensar em um subdomínio como representando um modelo de domínio único e lógico. Ele é uma parte do domínio que tem um propósito específico e pode ser modelado de forma independente. Subdomínios são usados para dividir a complexidade de um domínio maior em partes gerenciáveis.

A razão para essa divisão é prática: a maioria dos domínios empresariais são geralmente muito grandes e complexos para serem compreendidos como um todo, então geralmente nos preocupamos apenas com os subdomínios que devemos usar dentro de um único projeto.

#### Características Essenciais dos Subdomínios

- 1. Área de Especialização
Como destacado "outra maneira de pensar em um Subdomínio é que ele é uma área clara de especialização, assumindo que é responsável por fornecer uma solução para uma área central de seu negócio". Cada subdomínio é uma parte distinta do domínio que tem um propósito específico e pode ser modelado de forma independente.

- 2. Especialistas de Domínio
Cada subdomínio implica que terá um ou mais especialistas de domínio que compreendem muito bem os aspectos do negócio que um subdomínio específico facilita.

- 3. Significância Estratégica
O subdomínio também possui maior ou menor significância estratégica para seu negócio.

- **Propósito**: Dividir a complexidade de todo o domínio em partes gerenciáveis
- **Insight Chave**: Todo domínio de software tem múltiplos subdomínios representando diferentes funções de negócio
- **Regra**: Separar diferentes funções de negócio para evitar acúmulo de complexidade

```
Domínio = Subdomínio₁ + Subdomínio₂ + Subdomínio₃ + ... + Subdomínioₙ
```

#### Tipos de Subdomínios

| Tipo | Descrição | Valor de Negócio | Prioridade do Time | Exemplo |
|------|-------------|----------------|---------------|---------|
| **Domínio Principal** | Importância primária para o sucesso do negócio. Fornece vantagem competitiva. | Mais alto | Melhores desenvolvedores, especialistas do domínio | Algoritmo de previsão para uma empresa de varejo |
| **Subdomínio de Suporte** | Essencial mas não diferenciador. Específico do negócio, apoia o Principal. | Médio | Desenvolvedores competentes | Gerenciamento de inventário |
| **Subdomínio Genérico** | Funcionalidade comum. Pode ser comprado/terceirizado. Problema bem compreendido. | Mais baixo | Pode terceirizar | Autenticação, Envio de email |

**Matriz de Decisão para Classificação de Subdomínios:**

```
É uma vantagem competitiva? 
  SIM → Domínio Principal
  NÃO → Requer conhecimento específico do negócio?
         SIM → Subdomínio de Suporte
         NÃO → Subdomínio Genérico
```

### Exemplos

#### Domínio Principal

Como "um subdomínio central descreve a funcionalidade que dá à empresa uma vantagem competitiva". Isso inclui invenções, otimizações inteligentes de processos existentes, conhecimento único ou outras propriedades intelectuais.

**Exemplos práticos:**

- Para Netflix: o catálogo de vídeos e o algoritmo de recomendação
- Para Amazon: o sistema de gerenciamento de pedidos e logística
- Para uma empresa de seguros: o modelo de avaliação de risco e precificação

#### Subdomínio de Suporte

Estes subdomínios estão relacionados aos centrais, mas não são diferenciadores-chave, "eles podem apoiar os subdomínios centrais, mas não são essenciais para entregar valor real aos usuários". Eles são específicos do negócio, mas não fornecem vantagem competitiva.

**Exemplos práticos:**

- Para Netflix: o sistema de gerenciamento de conteúdo
- Para Amazon: o sistema de gerenciamento de inventário
- Para uma empresa de seguros: o sistema de processamento de sinistros

#### Subdomínio Genérico

Estes subdomínios são usados para completar a plataforma, "frequentemente as empresas decidem usar software pronto porque não estão estritamente relacionados ao seu domínio"

**Exemplos práticos:**

- Autenticação
- Envio de email
- Processamento de pagamento
- Gerenciamento de arquivos

### Contexto Delimitado

Um **Contexto Delimitado** é um limite explícito dentro do qual um modelo de domínio existe.

- **Natureza Primária**: Um **limite linguístico (linguagem ubíqua)** onde os termos têm significados específicos
- **Regra Chave**: Dentro do limite, todos os termos da Linguagem Ubíqua têm significado inequívoco

> **Contexto é Rei**: O mesmo termo pode ter significados completamente diferentes em contextos diferentes. Exemplo: "Conta" no Contexto Bancário vs "Conta" no Contexto Literário.

```
Contexto Delimitado = Limite Linguístico Explícito + Modelo de Domínio + Infraestrutura de Suporte
```

## Espaço do Problema vs Espaço da Solução

### Espaço do Problema

- **Definição**: As partes do Domínio que precisam ser desenvolvidas/abordadas
- **Composição**: Domínio Principal + Subdomínios de Suporte que ele precisa
- **Propósito**: Avaliação estratégica de desafios de negócio
- **Ferramenta**: Subdomínios (úteis para visualizar rapidamente diferentes partes)

### Espaço da Solução

- **Definição**: Os modelos de software específicos que realizam a solução
- **Composição**: Um ou mais Contextos Delimitados
- **Propósito**: Implementação de software real
- **Ferramenta**: Contextos Delimitados (realização de software específica)

```
Espaço do Problema (o que resolver) → avaliado usando → Subdomínios
                ↓ transforma em ↓
Espaço da Solução (como resolver) → realizado usando → Contextos Delimitados
```

**Objetivo Ideal**: Alinhar Subdomínios 1:1 com Contextos Delimitados (segrega expressamente modelos de domínio por objetivo de negócio).

**Realidade**: Em sistemas legados, Subdomínios frequentemente intersectam múltiplos Contextos Delimitados, ou um Contexto Delimitado contém múltiplos modelos implícitos.

## Linguagem Ubíqua

A **Linguagem Ubíqua** é o vocabulário compartilhado entre desenvolvedores e especialistas do domínio.

### Princípios Chave

1. **Linguagem Dirige Limites**: Os limites do Contexto Delimitado são primariamente linguísticos
2. **Mesmo Termo, Significado Diferente**: Quando termos têm significados diferentes, eles pertencem a contextos diferentes
3. **Sem Definições Globais**: Não tente criar significados globais únicos para todos os conceitos
4. **Aceite Diferenças**: Use Contextos Delimitados para delinear onde as diferenças são explícitas

### Detecção de Limites Linguísticos

**Sinais de que conceitos pertencem a Contextos Delimitados DIFERENTES:**

- Mesmo termo usado com propriedades/comportamentos diferentes
- Mesmo termo usado em diferentes estágios de um ciclo de vida
- Especialistas do domínio usam vocabulário diferente para conceitos similares
- Conceitos têm relacionamentos diferentes dependendo do contexto

**Exemplo - "Cliente" em E-Commerce:**

| Contexto | Significado de Cliente |
|---------|------------------|
| Navegação de Catálogo | Compras anteriores, fidelidade, produtos disponíveis, descontos |
| Realização de Pedido | Nome, endereço de entrega, endereço de cobrança, termos de pagamento |

→ Estes são conceitos diferentes que compartilham um nome = Contextos Delimitados diferentes

**Exemplo - "Livro" em Editora:**

| Estágio do Ciclo de Vida | Definição de Livro |
|-----------------|-----------------|
| Conceptualização | Ideia proposta, autor potencial |
| Contrato | Título tentativo, acordo com autor |
| Editorial | Rascunhos, comentários, correções, rascunho final |
| Produção | Layout de páginas, imagens de impressão, chapas |
| Marketing | Arte da capa, descrições |
| Expedição | Identidade, localização no inventário, tamanho, peso |

→ Cada estágio tem um modelo diferente de "Livro" = Contextos Delimitados separados

## Anti-Padrões de Design Estratégico

### Big Ball of Mud

**Definição**: Um sistema caótico de código espaguete onde tudo está conectado a tudo.

**Causas:**

- Misturar diferentes vocabulários de negócio em um modelo
- Não separar limites linguísticos
- Misturar Domínio Principal com preocupações Genéricas
- Falta de Contextos Delimitados explícitos

**Prevenção:**

- Compreender seu Domínio e Subdomínios
- Criar limites explícitos de Contexto Delimitado
- Separar preocupações linguísticas

### Modelo Abrangente (Modelo Empresarial)

**Definição**: Tentar criar um modelo único e coeso do domínio de negócio inteiro de uma organização.

**Por Que Falha:**

- Impossível estabelecer acordo global sobre todos os significados de conceitos
- Diferentes stakeholders têm perspectivas diferentes
- Definições globais duradouras são improváveis
- Cria conflitos e contenção

**Solução**: Aceite que diferenças sempre existem. Use Contextos Delimitados para delinear separadamente cada modelo de domínio.

### Conceitos Linguísticos Misturados

**Exemplo de SaaSOvation:**

```
❌ ERRADO: Forum, Post, Discussion acoplados com User, Permission
   - User/Permission são conceitos de identidade/segurança
   - Forum/Post/Discussion são conceitos de colaboração
   - Estes não harmonizam na Linguagem Ubíqua de Colaboração

✅ CORRETO: Forum, Post, Discussion acoplados com Author, Moderator, Participant
   - Todos os conceitos têm associação linguística com colaboração
   - Conceitos de identidade pertencem ao Contexto de Identidade e Acesso
```

**Regra**: Todo conceito em um Contexto Delimitado deve ter uma associação linguística com o domínio desse contexto.

## Tamanho dos Contextos Delimitados

### Princípio Orientador

> Um Contexto Delimitado deve ser tão grande quanto precisa ser para expressar completamente sua Linguagem Ubíqua completa.

### Princípio Mozart

> "Há apenas tantas notas quanto eu precisei, nem mais nem menos."

- Não muito pequeno (lacunas de conceitos faltantes)
- Não muito grande (águas turvas de conceitos estranhos)

### O Que Incluir

- ✅ Conceitos que são parte da Linguagem Ubíqua
- ✅ Conceitos que especialistas do domínio descrevem como relacionados
- ✅ Componentes que naturalmente se encaixam em um modelo coeso

### O Que Excluir

- ❌ Conceitos não na sua Linguagem Ubíqua
- ❌ Conceitos estranhos não verdadeiramente parte do Domínio Principal
- ❌ Funcionalidade genérica que pertence a Subdomínios de Suporte/Genéricos

### Razões Erradas para Dimensionar Contextos Delimitados

| Razão Errada | Por Que É Errado |
|--------------|----------------|
| Componentes arquiteturais | Componentes técnicos não definem limites linguísticos |
| Distribuição de tarefas de desenvolvedores | Impor limites para gerenciamento de tarefas vai contra motivações linguísticas |
| Convenções de plataforma/framework | Infraestrutura não deve direcionar limites de domínio |

**Alternativa para Distribuição de Tarefas**: Use Módulos dentro de um Contexto Delimitado para dividir responsabilidades dos desenvolvedores.

## Integração Entre Contextos Delimitados

### Insights Chave

1. **Contextos Delimitados raramente ficam sozinhos**: Mesmo sistemas grandes não podem fazer tudo
2. **Integração é necessária**: Modelos diferentes devem trabalhar juntos
3. **Mapeamento é necessário**: Ao integrar, tradução deve ocorrer entre Contextos Delimitados
4. **Identidade compartilhada, modelos diferentes**: Objetos podem compartilhar identidade entre contextos mas ter propriedades diferentes

### Exemplo de Integração

```
User (Contexto de Identidade) + Role (Contexto de Identidade)
           ↓ Integração + Tradução ↓
      Moderator (Contexto de Colaboração)
```

- Atributos de User são usados para criar um Moderator
- Mas Moderator é um conceito diferente com propriedades diferentes
- A tradução respeita as Linguagens Ubíquas de ambos os contextos

## Questões de Avaliação

### Avaliação do Espaço do Problema

1. Qual é o nome e a visão para o Domínio Principal estratégico?
2. Quais conceitos devem ser considerados parte do Domínio Principal?
3. Quais são os Subdomínios de Suporte e Subdomínios Genéricos necessários?
4. Quem deve fazer o trabalho em cada área do domínio?
5. Os times certos podem ser montados?

### Avaliação do Espaço da Solução

1. Quais ativos de software já existem, e podem ser reutilizados?
2. Quais ativos precisam ser adquiridos ou criados?
3. Como todos estes estão conectados entre si, ou integrados?
4. Que integração adicional será necessária?
5. Onde os termos das Linguagens Ubíquas são completamente diferentes?
6. Onde há sobreposição e compartilhamento de conceitos entre Contextos Delimitados?
7. Como termos compartilhados são mapeados e traduzidos entre Contextos Delimitados?
8. Qual Contexto Delimitado contém os conceitos do Domínio Principal?

## Aplicação Prática para Análise de Código

### Identificando Domínios no Código

1. **Procure por clusters de Entidades**: Grupos de entidades relacionadas com vocabulário compartilhado
2. **Analise responsabilidades de Serviços**: Que operações de negócio eles executam?
3. **Verifique escopo de Casos de Uso**: Que problemas de negócio eles resolvem?
4. **Examine agrupamentos de Controllers**: Que capacidades eles expõem?

### Detectando Limites Errados

| Sinal | Interpretação |
|--------|----------------|
| Mesma classe usada em múltiplos contextos com propriedades idênticas | Erro potencial de modelagem (a menos que Shared Kernel) |
| Conceitos com vocabulários diferentes no mesmo módulo | Limites linguísticos misturados |
| Serviço lidando com múltiplos domínios de negócio | Baixa coesão, deveria ser dividido |
| Entidade com relacionamentos a domínios não relacionados | Limites pouco claros |

### Validando Limites Corretos

| Sinal | Interpretação |
|--------|----------------|
| Todos os conceitos compartilham vocabulário de negócio | Boa coesão linguística |
| Conceitos têm propriedades/operações específicas ao contexto | Separação apropriada |
| Pontos de integração claros com outros contextos | Limites explícitos |
| Especialistas do domínio podem descrever conceitos sem confusão | Linguagem Ubíqua saudável |

## Resumo: Principais Conclusões para LLMs

1. **Domínio** = Reino de negócio. **Subdomínio** = Função de negócio específica dentro dele.
2. **Contexto Delimitado** = Limite linguístico onde termos têm significados específicos e inequívocos.
3. **Linguagem Ubíqua** dirige limites, NÃO arquitetura técnica.
4. **Domínio Principal** = Vantagem competitiva. **Suporte** = Essencial mas não diferenciador. **Genérico** = Pode terceirizar.
5. **Espaço do Problema** (Subdomínios) → **Espaço da Solução** (Contextos Delimitados).
6. **Anti-padrões**: Big Ball of Mud, Modelo Abrangente, Conceitos Linguísticos Misturados.
7. **Tamanho do contexto**: Não determinado por arquitetura ou distribuição de tarefas, mas pela completude da Linguagem Ubíqua.
8. **Integração**: Contextos Delimitados devem integrar; mapeamento/tradução é necessário.
9. **Mesmo termo, contexto diferente** = Conceito diferente = Contexto Delimitado diferente.
10. **Sempre pergunte aos especialistas do domínio** para compreender limites linguísticos.
