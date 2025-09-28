---
marp: true
---

# Building Effective Agents
A Journey from Prototyping to Production

---

_Depois de testar vários frameworks de LLM chains e agentes e ter construindo produtos de IA de verdade, **percebi que quase ninguém usa esses frameworks em produção.**_

_As aplicações de IA que funcionam bem costumam ser feitas com código próprio, não com frameworks prontos. Isso porque os agentes que realmente entregam valor não são tão "agentes" assim, são softwares determinísticos, com chamadas pontuais de LLM nos lugares certos._

Daniel Romiero - AI Engineer | Generative AI, RAG & Agents 

---

...onde comecou a jornada.

# 12-factor-agents

Assim como em determinado tempo os 12 fatores foram a base para o desenvolvimento de aplicações web escaláveis, devido ao crescimento desenfreado eis que surgiram os 12 fatores base para o desenvolvimento de agentes eficazes.

---

### 1. Domine a Conversão de Linguagem Natural para Chamada de Ferramenta

A capacidade de transformar solicitações em linguagem natural em chamadas de ferramentas estruturadas está no coração da funcionalidade do agente. É isso que permite ao agente pegar um comando como **“crie um link de pagamento de R$750 para Terri para o meetup dos IA Tinkerers de fevereiro”** e convertê-lo em uma chamada de API devidamente formatada.

---

![Texto alternativo da imagem](https://github.com/humanlayer/12-factor-agents/raw/main/img/110-natural-language-tool-calls.png)

---

Exemplo de saida estruturada.

```json
{
  "function": {
    "name": "create_payment_link",
    "parameters": {
      "amount": 750,
      "customer": "cust_128934ddasf9",
      "product": "prod_8675309",
      "price": "prc_09874329fds",
      "quantity": 1,
      "memo": "Hey Jeff - see below for the payment link for the february ai tinkerers meetup"
    }
  }
}
```

---

A saida estrutura lhe permitira ter mais controles de fluxos customizados.

```python
# The LLM takes natural language and returns a structured object
nextStep = await llm.determineNextStep(
  """
  create a payment link for $750 to Jeff 
  for sponsoring the february AI tinkerers meetup
  """
  )

# Handle the structured output based on its function
if nextStep.function == 'create_payment_link':
    stripe.paymentlinks.create(nextStep.parameters)
    return  # or whatever you want, see below
elif nextStep.function == 'something_else':
    # ... more cases
    pass
else:  # the model didn't call a tool we know about
    # do something else
    pass
```

---

✅ Essa abordagem traz algumas vantagens:

- **Confiabilidade**: menos risco de “alucinação”.
- **Segurança**: o agente só chama APIs autorizadas.
- **Testabilidade**: fácil criar testes automatizados para entradas/saídas.
- **Escalabilidade**: novas ferramentas podem ser plugadas sem mudar a lógica central.

---

> Esse fator é como transformar o agente de um “gerador de texto” em um **“orquestrador de ações reais**”.

---

### 2. Tenha Total Domínio dos Seus Prompts

Seus prompts são a interface entre sua aplicação e o modelo de linguagem — **trate-os como código de primeira classe.** Embora frameworks que abstraem prompts possam parecer convenientes, muitas vezes eles ocultam como as instruções são passadas para o LLM, dificultando ou impossibilitando ajustes finos.

---

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

![w:640 center](https://hamel.dev/blog/posts/prompt/slap_3.jpeg)

---

![Texto alternativo da imagem](https://github.com/humanlayer/12-factor-agents/raw/main/img/120-own-your-prompts.png)

---

```python
def DetermineNextStep(thread: string) 
-> DoneForNow | ListGitTags | DeployBackend | DeployFrontend | RequestMoreInformation {
  prompt #"
    {{ _.role("system") }}
    
    ... Some Instructions
    
    Always think about what to do first, like:
    - Check current deployment status
    - Verify the deployment tag exists
    - Request approval if needed
    - Deploy to staging before production
    - Monitor deployment progress
    
    {{ _.role("user") }}

    {{ thread }}
    
    What should the next step be?
  "#
}
```

---

✅ Essa abordagem traz algumas vantagens:

- **Confiabilidade**: Controle total para escrever instruções precisas sob medida para seu caso de uso.
- **Testabilidade**: Capacidade de criar avaliações e testes para prompts como qualquer outro código.
- **Manutenabilidade**: Transparência para entender exatamente o que o LLM recebe.
- **Observabilidade**: Liberdade para iterar com base em métricas de desempenho.

---

> Esse fator garante que o agente não vire uma “caixa-preta imprevisível” — o prompt é código, deve ser versionado, testado e auditado como tal.

---

### 3. Projete sua Janela de Contexto de Forma Estratégica

A janela de contexto serve como entrada para o LLM, abrangendo prompts, histórico de conversas e dados externos. Otimizá-la aumenta o desempenho e a eficiência do uso dos tokens.

---

![Texto alternativo da imagem](https://raw.githubusercontent.com/humanlayer/12-factor-agents/main/img/220-context-engineering.png)

---

Gerencie a janela de contexto do LLM de forma proativa e intencional.

- Garanta que a janela de contexto contenha apenas informações diretamente relevantes para a tarefa atual.
- Remova dados desnecessários para evitar "deriva de contexto".
- Melhore o desempenho do LLM e reduza o uso de tokens.

---

> Esse fator garante que o agente não desperdice contexto, usando recuperação inteligente e resumo de estado.

---

### 4. Implemente Ferramentas com Saídas Estruturadas

No essencial, ferramentas são apenas saídas JSON do LLM que disparam ações determinísticas no seu código. Isso cria uma separação clara entre a tomada de decisão da IA e a lógica de execução.

Defina claramente os esquemas das ferramentas:

---

```python
class ResultadoTarefa(BaseModel):
    tarefa: str
    concluida: bool
    prioridade: int
    data: str | None = None


def inteligencia_estruturada(prompt: str) -> ResultadoTarefa:
    client = OpenAI()
    response = client.responses.parse(
        model="gpt-4o-mini",
        input=[
            {
                "role": "system",
                "content": "Extraia da entrada do usuário uma tarefa, seu status (concluída ou não), prioridade (1 = baixa, 2 = média, 3 = alta) e data associada, se houver.",
            },
            {"role": "user", "content": prompt},
        ],
        text_format=ResultadoTarefa,
    )
    return response.output_parsed
```

---

✅ Essa abordagem traz algumas vantagens:

- **Confiabilidade**: elimina parsing frágil de texto.
- **Segurança**: evita que o agente execute comandos errados.
- **Testabilidade**: fácil escrever testes unitários para validar entradas e saídas.
- **Escalabilidade**: múltiplos agentes/ferramentas falam a mesma “língua” (JSON).

---

> Esse fator garante que ferramentas não devolvam “texto solto”, mas sim objetos bem definidos, prontos para integração confiável.

---

### 5. Unifique Execução e Estado de Negócio

Muitos frameworks de agentes separam o estado de execução (ex: etapa atual de um processo) do estado de negócio (ex: histórico de chamadas de ferramentas e seus resultados). Essa separação adiciona complexidade desnecessária.

Em vez disso, armazene todo o estado diretamente na janela de contexto, inferindo o estado de execução a partir da sequência de eventos

---

![w:1040 center "Texto alternativo da imagem](https://github.com/humanlayer/12-factor-agents/raw/main/img/155-unify-state-animation.gif)

---

✅ Essa abordagem traz algumas vantagens:

- **Confiabilidade**: evita perda de contexto se o agente cair.
- **Governança**: auditoria clara do que foi feito.
- **Escalabilidade**: múltiplos agentes podem cooperar no mesmo fluxo.
- **Resiliência**: agentes podem retomar de onde pararam.

---

> Esse fator garante que o cérebro do agente (LLM) e o mundo real (negócio) estejam sempre sincronizados e auditáveis.

---

### 6. Projete APIs para Iniciar, Pausar e Retomar

Agentes de nível de produção precisam se integrar perfeitamente a sistemas externos, pausando para tarefas longas e retomando quando acionados por webhooks ou outros eventos.

Implemente APIs que permitam iniciar, pausar e retomar agentes, com armazenamento de estado robusto entre operações. Isso possibilita:

- Suporte flexível para fluxos de trabalho assíncronos
- Integração limpa com webhooks e outros sistemas
- Retomada confiável após interrupções sem reiniciar

---

!["Texto alternativo da imagem"](https://github.com/humanlayer/12-factor-agents/raw/main/img/165-pause-resume-animation.gif)

---

✅ Essa abordagem traz algumas vantagens:

- **Observabilidade**: sabe exatamente em que estado cada tarefa está.
- **Resiliência**: pausa ou retoma sem perder progresso.
- **Integração**: sistemas externos podem orquestrar agentes de forma confiável.
- **Segurança**: evita execuções paralelas indesejadas.

---

> Esse fator permite que tarefas longas ou agentes complexos sejam totalmente controláveis via APIs, tornando o sistema previsível, seguro e auditável.

---