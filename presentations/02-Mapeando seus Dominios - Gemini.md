---
marp: true
theme: default
paginate: true
header: 'DDD Estratégico: Mapeando Domínios'
footer: 'Workshop de Arquitetura de Software'
style: |
  section {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }
  h1 { color: #2c3e50; }
  blockquote {
    background: #f9f9f9;
    border-left: 10px solid #2c3e50;
    font-style: italic;
  }
  img[alt~="center"] {
    display: block;
    margin: 0 auto;
  }
---

# DDD: A Estratégia por trás do Código
### Transformando Complexidade de Negócio em Vantagem Competitiva

---

## O que é DDD em um tweet?
_"DDD não é framework, é mentalidade: negócios primeiro, código depois. Foque no problema, modele domínios, conecte arquitetura à estratégia, não à tabelas"_

Introduzido por Eric Evans em seu livro "Domain-Driven Design: Tackling Complexity in the Heart of Software".

Massss...

---

Imagem do livro de DDD da capa azul, com a legenda:

_A leitura do livro da capa azul é consideralvemente extensiva e complexa, mas existem outras fontes que podem ajudar a entender os conceitos de DDD rapidamente._

---

Aqui minha imagem das recomendacoes de leitura, com a legenda:

_Essas são minhas recomendações de leitura para quem quer entender DDD de forma rápida e prática. O livro da capa azul é ótimo, mas pode ser denso para primeira leitura._

---

## DDD em Nível Executivo 👔

> "DDD não é sobre padrões de código (como Repositories ou Entities). É sobre conectar o design do software à estratégia de negócio."

**A mentalidade:**
- **Negócio Primeiro:** O código deve refletir a realidade do mercado.
- **Foco no Problema:** Modelar a solução apenas após entender profundamente a dor.
- **Arquitetura viva:** O software evolui junto com a estratégia da empresa.

---

## Por que 70% dos Projetos de Software Falham? 📉

Não é por falta de tecnologia (Cloud, K8s, AI), mas por **falta de entendimento do domínio**.

- **Desperdício:** Construir funcionalidades que o cliente não valoriza.
- **Rigidez:** Código que não consegue acompanhar as mudanças de regras de negócio.
- **Desconexão:** Desenvolvedores e Especialistas de Negócio falando línguas diferentes.

---

## Domínio: O Coração da Operação ❤️

O domínio define a área de atividade e o conjunto de regras que regem a empresa.

| Empresa | Domínio Principal |
| :--- | :--- |
| **Amazon** | Varejo e Logística Global |
| **Uber** | Mobilidade e Logística Urbana |
| **Netflix** | Entretenimento e Streaming |

---

## Subdomínios: Decompor para Conquistar 🧩

Um domínio complexo é a soma de vários subdomínios menores.
*Exemplo: E-commerce*
- Gestão de Catálogo
- Processamento de Pagamentos
- Logística de Entrega

---

## A Matriz de Valor de Negócio 💰

Nem todo código é igual. Como arquitetos, devemos classificar os subdomínios para investir onde importa:

1.  **Core Domain (Ouro):** Onde está a inovação e o lucro. É o que te diferencia da concorrência.
2.  **Supporting (Prata):** Apoia o Core, mas não é o diferencial. Pode ser desenvolvido internamente por times menos experientes.
3.  **Generic (Commodity):** Problemas já resolvidos pelo mercado. **Compre pronto ou use SaaS.**

---

## Onde colocar seus melhores Talentos?

| Tipo | Estratégia | Esforço |
| :--- | :--- | :--- |
| **Core** | Criar algo Único | Máximo (Seniors) |
| **Supporting** | Eficiência | Médio |
| **Generic** | Integração (Buy over Build) | Mínimo |

---

## 5 Passos para um Mapeamento de Elite 🚀

1.  **Identifique o Core:** Onde a empresa perderia mais dinheiro se o sistema parasse?
2.  **Linguagem Ubíqua:** Elimine o "Juridiquês" ou o "Tech-speak". Use o vocabulário do negócio no código.
3.  **Contextos Delimitados:** Não tente criar um modelo que resolva tudo. Divida para manter a sanidade.
4.  **Colaboração Ativa:** Arquitetos não trabalham sozinhos. Use Event Storming com os stakeholders.
5.  **Evolução:** O que é "Core" hoje pode virar "Generic" amanhã. Monitore o mercado.

---

## Dinâmica Prática: Case Joalheria 💎

Vamos aplicar os conceitos:
- O que é o diferencial (Core) de uma joalheria de luxo?
- O que é apenas suporte?
- O que podemos comprar pronto?

---

# Conclusão
> "Arquitetura começa no entendimento do negócio, não no framework."