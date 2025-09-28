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

!["Texto alternativo da imagem"](https://github.com/humanlayer/12-factor-agents/raw/main/img/110-natural-language-tool-calls.png)

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

!["Texto alternativo da imagem"](https://github.com/humanlayer/12-factor-agents/raw/main/img/120-own-your-prompts.png)

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

!["Texto alternativo da imagem"](https://raw.githubusercontent.com/humanlayer/12-factor-agents/main/img/220-context-engineering.png)

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

### 7. Habilite Colaboração Humana com Chamadas de Ferramenta

Agentes de IA as vezes necessitam de input humano para decisões críticas ou situações ambíguas. Usar chamadas de ferramentas estruturadas torna essa interação fluida:

---

!["Texto alternativo da imagem"](https://github.com/humanlayer/12-factor-agents/raw/main/img/170-contact-humans-with-tools.png)

---

```python
class Options:
  urgency: Literal["low", "medium", "high"]
  format: Literal["free_text", "yes_no", "multiple_choice"]
  choices: List[str]

# Tool definition for human interaction
class RequestHumanInput:
  intent: "request_human_input"
  question: str
  context: str
  options: Options

# Example usage in the agent loop
if nextStep.intent == 'request_human_input':
  thread.events.append({
    type: 'human_input_requested',
    data: nextStep
  })
  thread_id = await save_state(thread)
  await notify_human(nextStep, thread_id)
  return # Break loop and wait for response to come back with thread ID
```

---

✅ Essa abordagem traz algumas vantagens:

**Segurança** e conformidade: decisões críticas passam por revisão humana.
**Resiliência**: agente continua workflow mesmo quando precisa de ajuda.
**Rastreabilidade**: histórico claro de decisões e interações.
**Escalabilidade**: humanos podem atuar apenas em pontos críticos, agentes cuidam do restante.

---

> O agente deve saber quando envolver humanos, como notificar e como processar a resposta, mantendo todo o fluxo estruturado e auditável.

---

### 8. Controle o Fluxo do Seu Agente
Fluxo de controle personalizado permite pausar para aprovação humana, armazenar resultados em cache ou implementar limitações de taxa — adaptando o comportamento do agente às suas necessidades específicas.

---

!["Texto alteranativo da imagem"](https://github.com/humanlayer/12-factor-agents/raw/main/img/180-control-flow.png)

---

```python
async function handleNextStep(thread: Thread) {
  while (true) {
    const nextStep = await determineNextStep(threadToPrompt(thread));
    if (nextStep.intent === 'request_clarification') {
      await sendMessageToHuman(nextStep);
      await db.saveThread(thread);
      break;
    } else if (nextStep.intent === 'fetch_open_issues') {
      const issues = await linearClient.issues();
      thread.events.push({ type: 'fetch_open_issues_result', data: issues });
      continue;
    }
  }
}
```

---

**💡 Tips**

- Execute um ciclo OODA explícito (Observar-Orientar-Decidir-Agir).
- Use heurísticas de convergência em vez de prompts aninhados.
- Evite que o LLM gerencie fluxos de controle complexos.

---

### Ciclo OODA

![center "Texto alternativo da imagem"](https://upload.wikimedia.org/wikipedia/commons/3/3a/OODA.Boyd.svg)

---

✅ Essa abordagem traz algumas vantagens:

**Resiliência**: lida com falhas e instabilidades externas.
**Previsibilidade**: comportamento do agente é determinístico e auditável.
**Segurança**: evita decisões automáticas fora do esperado.
**Escalabilidade**: múltiplos agentes podem seguir o mesmo fluxo padrão.

---

> O agente **não deve depender apenas do modelo para decidir o que fazer**.
Ele precisa de **fluxo de controle explícito**, com **decisões claras, retries, timeouts e fallbacks**, garantindo confiabilidade e previsibilidade.

---

### 9. Compacte erros na Janela de Contexto
Incluir erros diretamente na janela de contexto permite que agentes de IA aprendam com falhas e ajustem sua abordagem.

Modelos de linguagem têm limite de tokens, portanto não é viável enviar todas as falhas ou logs detalhados no contexto.

---

!["Texto alternativo imagem"](https://github.com/humanlayer/12-factor-agents/blob/main/img/195-factor-9-errors.gif?raw=true)

---

```python
consecutive_errors = 0

while True:

  # ... existing code ...

  try:
    result = await handle_next_step(thread, next_step)
    thread["events"].append({
      "type": next_step.intent + '_result',
      data: result,
    })
    # success! reset the error counter
    consecutive_errors = 0
  except Exception as e:
    consecutive_errors += 1
    if consecutive_errors < 3:
      # do the loop and try again
      thread["events"].append({
        "type": 'error',
        "data": format_error(e),
      })
    else:
      # break the loop, reset parts of the context window, escalate to a human, or whatever else you want to do
      break
  }
}
```

---

**💡 Tips** Compacte as informações de erro no próximo prompt para fechar o ciclo de feedback.

- Permita que o LLM aprenda e se recupere de erros.
- Forneça informações de erro estruturadas.
- Suporte a capacidade de autocorreção.

---

✅ Essa abordagem traz algumas vantagens:

**Eficiência**: menos tokens = menor custo e resposta mais rápida.
**Confiabilidade**: o agente aprende com seus erros e se recupera dos mesmos.
**Observabilidade**: e possivel identificar pontos de falha e ajustar fluxos.
**Escalabilidade**: fluxos longos ou agentes múltiplos continuam operando sem estourar contexto.

---

> Esse fator garante que o agente não perca contexto relevante e ainda economize tokens, mantendo a confiabilidade mesmo em fluxos longos ou complexos.

---

### 10. Construa Agentes Pequenos e Focados

Agentes pequenos que lidam com 3 a 20 etapas mantêm janelas de contexto administráveis, melhorando o desempenho e a confiabilidade do LLM. Essa abordagem proporciona:

- Clareza com escopo bem definido para cada agente
- Menor risco do agente perder o foco
- Testes e validação mais fáceis de funções específicas

---

!["Texto alterantivo da imagem"](https://github.com/humanlayer/12-factor-agents/raw/main/img/1a0-small-focused-agents.png)

---

```python
# agente_pedidos.py
class AgentePedidos:
    def processar_pedido(self, pedido_id):
        print(f"[AgentePedidos] Processando pedido {pedido_id}")
        return {"pedido_id": pedido_id, "status": "processado"}

# agente_pagamentos.py
class AgentePagamentos:
    def validar_pagamento(self, pedido_id):
        print(f"[AgentePagamentos] Validando pagamento do pedido {pedido_id}")
        return {"pedido_id": pedido_id, "pagamento_valido": True}

# orquestrador.py
from agente_pedidos import AgentePedidos
from agente_pagamentos import AgentePagamentos

class Orquestrador:
    def __init__(self):
        self.agente_pedidos = AgentePedidos()
        self.agente_pagamentos = AgentePagamentos()
    
    def workflow_pedido(self, pedido_id):
        resultado_pagamento = self.agente_pagamentos.validar_pagamento(pedido_id)
        if resultado_pagamento["pagamento_valido"]:
            return self.agente_pedidos.processar_pedido(pedido_id)
        else:
            return {"pedido_id": pedido_id, "status": "falha no pagamento"}

# Uso
orquestrador = Orquestrador()
resultado = orquestrador.workflow_pedido(123)
print("Resultado final:", resultado)
```

---

✅ Essa abordagem traz algumas vantagens:

**Manutenção** mais fácil: mudanças em um agente não afetam os outros.
**Confiabilidade**: menor chance de erro global.
**Testabilidade**: cada agente tem inputs e outputs bem definidos.
**Flexibilidade**: novos agentes podem ser adicionados sem refatorar o sistema existente.

---

> Cada agente tem uma responsabilidade clara. O orquestrador combina agentes pequenos para formar workflows maiores. Essa abordagem torna o sistema mais modular, testável e escalável.

---

### 11. Habilite Gatilhos de Múltiplas Fontes
Deixe seus agentes acessíveis permitindo acionamento por Slack, e-mail ou sistemas de eventos — encontrando os usuários onde eles já trabalham.

Implemente APIs que iniciam agentes a partir de diversos canais e respondem pelo mesmo meio. Isso permite:

- Melhor acessibilidade ao integrar com plataformas preferidas dos usuários
- Suporte a fluxos de trabalho de automação orientados a eventos
- Workflows de aprovação humana para operações críticas

---

!["Texto alternativo da imagem"](https://github.com/humanlayer/12-factor-agents/raw/main/img/1b0-trigger-from-anywhere.png)

---

```python
from datetime import datetime

class Evento:
    def __init__(self, tipo, dados, origem):
        self.tipo = tipo
        self.dados = dados
        self.origem = origem
        self.timestamp = datetime.now()

class Agente:
    def processar_evento(self, evento: Evento):
        print(f"[{evento.timestamp}] Evento de {evento.origem} ({evento.tipo}): {evento.dados}")

# Instanciando agente
agente = Agente()

# Eventos vindos de diferentes fontes
evento_api = Evento(tipo="pedido", dados={"pedido_id": 123}, origem="API")
evento_slack = Evento(tipo="aprovacao", dados={"pedido_id": 124}, origem="Slack")
evento_sqs = Evento(tipo="notificacao", dados={"mensagem": "Pagamento pendente"}, origem="SQS")

# Processando eventos
agente.processar_evento(evento_api)
agente.processar_evento(evento_slack)
agente.processar_evento(evento_sqs)
```

---

> O agente se torna reativo a múltiplas fontes de evento. Facilita integração com sistemas heterogêneos, aumento de confiabilidade e modularidade no fluxo de decisões.

---

### 12. Projete Agentes como Redutores Sem Estado
Trate agentes como funções sem estado que transformam o contexto de entrada em ações de saída, simplificando a gestão de estado e tornando-os previsíveis e mais fáceis de depurar.

---

!["Texto alteranativo da imamge"](https://github.com/humanlayer/12-factor-agents/raw/main/img/1c0-stateless-reducer.png)

---

Um agente não deve armazenar estado interno de longo prazo. Ele deve receber entradas, processá-las e emitir saídas, sem depender de memória própria para decisões futuras.

O estado de negócio ou histórico deve ser armazenado externamente (DB, cache, eventos).

Essa abordagem permite escala horizontal, reexecução e consistência.

---

✅ Essa abordagem traz algumas vantagens:

**Escalabilidade horizontal**: agentes podem ser replicados facilmente.
**Resiliência:** crash do agente não perde informações.
**Reprodutibilidade:** inputs determinam outputs de forma consistente.
**Simplicidade:** código do agente é mais previsível e testável.

---

> O agente não mantém estado interno, atuando como um reducer: transforma inputs em outputs. Todo estado relevante é persistido externamente, permitindo escala, reexecução e confiabilidade.

---

### Referencias:

[Twelve Factor Agents](https://github.com/humanlayer/12-factor-agents/tree/main)
[Twelve Factor Agents for Building Effective AI Systems at Scale](https://www.flowhunt.io/pt/blog/the-12-factor-ai-agent-building-effective-ai-systems-that-scale/)
[Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)
[Show me the Prompts](https://hamel.dev/blog/posts/prompt/)

---

![w:740 center "Texto alternativo da imagem"](https://clampettstudio.com/cdn/shop/files/That_sallFolks_1_web.jpg?v=1752108572&width=1445)
