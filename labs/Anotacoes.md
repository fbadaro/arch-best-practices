# Anotacoes

Junte material suficiente sobre determinado tema e peca para a IA transformar aquele conteudo em LLM friendly, ou seja, em um formato que seja facilmente compreendido e processado por modelos de linguagem como o ChatGPT. Isso pode incluir a organização do conteúdo em tópicos, a simplificação da linguagem, a remoção de informações irrelevantes e a estruturação dos dados de maneira clara e concisa. O objetivo é criar um conjunto de dados que permita à IA aprender e responder de forma eficaz sobre o tema em questão.

## Exemplo DDD - Domain-Driven Design

- **Contexto**: O DDD é uma abordagem de desenvolvimento de software que enfatiza a importância de entender o domínio do problema e modelar o software de acordo com esse entendimento. Ele se concentra em criar um modelo de domínio rico e expressivo que reflete as complexidades do mundo real.
- **Tópicos**:
  - **Entidades**: Objetos que possuem identidade única e são definidos por suas propriedades e comportamentos. Exemplo: Cliente, Pedido.
  - **Value Objects**: Objetos que não possuem identidade única e são definidos apenas por suas propriedades. Exemplo: Endereço, Dinheiro.
  - **Agregados**: Conjuntos de entidades e value objects que são tratados como uma unidade de consistência. Exemplo: Pedido (agregado) pode conter itens (entidades) e um endereço de entrega (value object).
  - **Repositórios**: Interfaces que fornecem métodos para acessar e persistir agregados. Exemplo: PedidoRepository.
  - **Serviços de Domínio**: Operações que não pertencem a nenhuma entidade ou value object, mas são importantes para o domínio. Exemplo: ProcessarPagamento.
  - **Eventos de Domínio**: Eventos que ocorrem dentro do domínio e podem ser usados para comunicar mudanças ou ações importantes. Exemplo: PedidoCriado, PagamentoProcessado.
- **Benefícios**: O DDD ajuda a criar software que é mais alinhado com as necessidades do negócio, facilita a comunicação entre desenvolvedores e especialistas do domínio, e promove a manutenção e evolução do software ao longo do tempo. Ele também pode melhorar a qualidade do código e reduzir a complexidade, tornando-o mais fácil de entender e modificar.

Sugestao de nome: DDD-STRATEGIC-DESIGN-THEORY.md

Posteriormente voce podera pediar para a IA criar um guideline para a identificar dominios e subdominios, ou seja, um processo passo a passo para analisar um problema e identificar quais são os domínios envolvidos, quais são os subdomínios dentro de cada domínio, e como eles se relacionam entre si. Isso pode incluir técnicas como entrevistas com especialistas do domínio, análise de processos de negócios, mapeamento de fluxos de trabalho, e a criação de diagramas de contexto para visualizar as interações entre os domínios. O objetivo é fornecer uma abordagem sistemática para entender o domínio do problema e criar um modelo de domínio eficaz.

Sugestao de nome: DDD-DOMAIN-IDENTIFICATION-GUIDELINE.md

1. Criando Guidelines de Domain-Driven Design Estratégico

- Extrair conhecimento de livros (DDD, arquitetura) e torná-los "LLM-friendly"
- Criar documentação sobre: domínios, subdomínios, bounded contexts
- Ensinar a IA conceitos como:
    - Baixa coesão: coisas próximas mas não relacionadas
    - Linguagem ubíqua: termos específicos por contexto (user vs customer)
    - Core domain vs supporting subdomain vs generic subdomain
    - Fronteiras linguísticas entre contextos

2. Guidelines para Identificação de Domínios

- Como identificar domínios através de:
    - Classes, entities, services, controllers
    - Agrupamento por linguagem ubíqua
    - Relacionamentos e dependências
    - Análise de vocabulário de negócio
- Métricas de coesão (score 0-3):
    - 3: Alta coesão - conceitos compartilham vocabulário de negócio
    - 2: Média - alguns relacionamentos diretos
    - 1: Baixa - conceitos raramente usados juntos
    - 0: Muito baixa - sem relacionamentos, mudam independentemente

- Uso do Git history para identificar se arquivos mudam juntos