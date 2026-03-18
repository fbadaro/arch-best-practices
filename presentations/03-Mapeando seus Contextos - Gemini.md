---
marp: true
theme: default
paginate: true
header: 'DDD Estratégico: Mapeando Contextos'
footer: 'Workshop de Arquitetura de Software'
style: |
  section {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }
  h1 { color: #1a5276; }
  blockquote {
    background: #f4f6f7;
    border-left: 10px solid #1a5276;
    font-style: italic;
  }
  img[alt~="center"] {
    display: block;
    margin: 0 auto;
  }
---

# Mapeando Contextos e Fronteiras
### Definindo a Governança e a Autonomia entre Sistemas e Times

---

## O Limite da Linguagem 🗣️

No DDD, um **Contexto Delimitado (Bounded Context)** não é apenas uma fronteira de código, mas uma fronteira de significado.

> "Se você precisa explicar 'o que isso significa aqui', você cruzou uma fronteira de contexto."

**Por que isso importa?**
- Evita modelos gigantes e confusos ("Entidades Deus").
- Garante que a **Linguagem Ubíqua** seja consistente dentro do seu limite.
- Permite que cada contexto tenha sua própria arquitetura e evolução independente.

---

## O Custo da Integração Mal Planejada 💸

Sistemas reais raramente vivem isolados. Integrações sem estratégia geram:
- **Alto Acoplamento:** Uma mudança em um sistema quebra outros cinco.
- **Efeitos Colaterais:** Comportamentos imprevisíveis em produção.
- **Lentidão:** Times dependentes uns dos outros para qualquer entrega.

**Sintomas Críticos:** Reuso forçado de entidades, compartilhamento de banco de dados e APIs genéricas/anêmicas.

---

## Context Map: O Contrato Social da Arquitetura 🗺️

O Mapa de Contexto descreve como diferentes áreas e times colaboram e trocam informações através de padrões definidos.

### Os 3 Pilares de Relacionamento entre Times:
1.  **Cooperação:** Times trabalham juntos em um objetivo comum.
2.  **Cliente-Fornecedor:** Um time depende do serviço do outro.
3.  **Caminhos Separados:** Autonomia total, sem integração direta.

---

## Estratégias de Cooperação 🤝

Para contextos fortemente acoplados ao negócio:

- **Partnership (Parceria):** Decisões tomadas em conjunto. Alto alinhamento, mas exige maturidade semelhante entre os times.
- **Shared Kernel (Núcleo Compartilhado):** Parte do modelo é comum a ambos. Reduz duplicação, mas exige forte governança para evitar que mudanças impactem o vizinho.

---

## Estratégias Cliente-Fornecedor: Protegendo seu Domínio 🛡️

Quando você depende de um sistema externo ou legado:

- **ACL (Camada Anticorrupção):** A estratégia mais segura. Cria uma camada de tradução que protege seu domínio interno de "sujeiras" externas.
- **Conformist (Conformista):** Você aceita o modelo do fornecedor como ele é. Simples, mas você perde a expressividade do seu próprio negócio.
- **Open Host Service (OHS):** O fornecedor oferece uma API padronizada para múltiplos consumidores, facilitando a escala.

---

## O Risco da "Big Ball of Mud" ⚠️

Um sistema sem limites claros torna-se uma "Grande Bola de Lama": modelos mistos, regras inconsistentes e manutenção impossível.

**A Regra de Ouro:**
Se você identifica uma "bola de lama" em um sistema legado, use uma **ACL** para garantir que essa bagunça não se propague para os seus novos contextos.

---

## Decisão de Arquitetura: Buy, Build or Integrate?

| Padrão | Autonomia | Esforço de Tradução | Risco |
| :--- | :--- | :--- | :--- |
| **ACL** | Alta | Alto | Baixo |
| **Conformist** | Baixa | Nulo | Alto |
| **Partnership** | Média | Baixo | Médio |
| **Separate Ways**| Total | Nulo | Inconsistência |

---

## Conclusão: Arquitetura é sobre Limites

- Mapear contextos é crucial para gerenciar a complexidade em escala.
- A escolha do padrão de integração é uma decisão **estratégica**, não apenas técnica.
- Um bom Context Map é uma ferramenta viva que reflete a estrutura da organização.

---

# Próximos Passos
### Como os seus times estão se falando hoje? Existe uma ACL protegendo seu Core Domain?