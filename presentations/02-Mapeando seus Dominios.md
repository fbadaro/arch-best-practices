---
marp: true
---

# Mapeando seus Domínios
Transformando complexidade em clareza com Domain-Driven Design

---

## O que é DDD em um tweet?
<!-- Domain Driven Design (DDD) é uma abordagem para o desenvolvimento de software que enfatiza a importância de entender o domínio do problema e modelar o software de acordo com esse entendimento. -->

_"DDD não é framework, é mentalidade: negócios primeiro, código depois. Foque no problema, modele domínios, conecte arquitetura à estratégia, não à tabelas"_

Introduzido por Eric Evans em seu livro "Domain-Driven Design: Tackling Complexity in the Heart of Software".

Massss...

---

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

![w:840 center](../src/02/01.jpg)

<br> 

_A leitura do livro da capa azul é consideralvemente extensiva e complexa, mas existem outras fontes que podem ajudar a entender os conceitos de DDD rapidamente._

---

![w:760 center](../src/02/02.png)

---

# Por que mapear domínios é crítico?
> “Por que tantos projetos falham mesmo com tecnologias modernas? Porque não entendem o negócio que estão tentando resolver.”

---

- DDD não é sobre código, mas sobre compreensão do domínio.
- Focar no domínio ajuda a criar soluções que realmente atendem às necessidades do negócio.
- Evita desperdício de tempo e recursos em funcionalidades desnecessárias.

<br>

_**“Arquitetura começa no entendimento do negócio, não no framework.**”_

---

![center](../src/02/03.png)

---

![center](../src/02/04.png)

---

![center](../src/02/05.png)

---

#  O que é um domínio e por que ele importa?
O dominio de negocio define qual é a área de atividade de uma empresa, ou seja, o conjunto de processos, regras e conceitos que regem o funcionamento do negócio.

Ex: Amazon, Uber, Netflix - todos operam em vários domínios específicos.

---

![center](../src/02/06.png)

---

#  O que é um subdomínio e por que ele importa?
Um subdomínio é um subconjunto do espaço do problema que somados formam o domínio principal.

Ex: No domínio de e-commerce, podemos ter subdomínios como "Gerenciamento de Produtos", "Processamento de Pedidos" e "Pagamento".

---

![center](../src/02/07.png)

---

# Tipos de Subdomínios
- **Core Domain (Domínio Central)**: O coração do negócio, onde está o maior valor.
- **Supporting Subdomain (Subdomínio de Suporte)**: Funcionalidades que suportam o domínio central.
- **Generic Subdomain (Subdomínio Genérico)**: Funcionalidades comuns que podem ser reutilizadas em diferentes contextos.

---

# Como arquitetos podem mapear corretamente?
Como no nosso dia a dia podemos mapear corretamente os domínios e subdomínios?

---

## Dica 1: Comece pelo Core Domain

Pergunte: “Qual parte do negócio gera mais valor?”
Foque onde a empresa é única.

---

## Dica 2: Identifique Subdomínios

Classifique em:

- Core (estratégico)
- Supporting (apoio)
- Generic (commodity)

---

![w:940 center](../src/02/08.jpg)

---

![w:1040 center](../src/02/09.jpg)

---

### Cases:

1 - [Netflix: De logistica de DVDs para streaming global](https://pt.wikipedia.org/wiki/Netflix).
2 - [Amazon: De livraria online para marketplace global](https://pt.wikipedia.org/wiki/Amazon.com).

---

## Dica 1: Comece pelo Core Domain

Pergunte: “Qual parte do negócio gera mais valor?”
Foque onde a empresa é única.

---

## Dica 2: Identifique Subdomínios

Classifique em:
- Core (estratégico)
- Supporting (apoio)
- Generic (commodity)

---

## Dica 3: Use Linguagem Ubíqua

Crie um vocabulário comum entre negócio e TI.
Evite termos técnicos que não fazem sentido para o negócio.

---

## Dica 4: Contextos Delimitados (Bounded Context)

Cada subdomínio deve ter seu próprio modelo.
Evite modelos gigantes que tentam resolver tudo.

---

## Dica 5: Colabore com especialistas do negócio

Workshops, Event Storming, entrevistas.
Arquitetos não mapeiam sozinhos.

---

# Dinâmica: Mapeando Domínios na Prática case Joalheria.
