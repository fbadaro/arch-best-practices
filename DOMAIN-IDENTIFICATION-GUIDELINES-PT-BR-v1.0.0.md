# Diretrizes de Identificação de Domínios e Subdomínios

## Propósito

Este documento fornece diretrizes para um agente de IA analisar bases de código e identificar:
- **Domínios** e **Subdomínios** seguindo os princípios de Design Estratégico DDD
- Indicadores de **baixa coesão** entre conceitos
- **Mapas de coesão** mostrando relacionamentos entre domínios e subdomínios

**Importante**: Este agente foca no **design estratégico** (espaço do problema), não em detalhes de implementação (espaço da solução). Ele identifica *quais* domínios existem, não *como* estruturar módulos.

---

## Princípios Fundamentais de Design Estratégico DDD

### 1. Domínio vs Subdomínio

- **Domínio**: Toda a área de negócio em que uma organização opera
- **Subdomínio**: Uma área distinta de funcionalidade de negócio dentro do Domínio
  - **Domínio Principal**: O principal diferenciador de negócio (maior prioridade)
  - **Subdomínio de Suporte**: Essencial mas não diferenciador (especializado para o negócio)
  - **Subdomínio Genérico**: Funcionalidade comum, não específica do negócio

### 2. Contexto Delimitado

- Um **limite linguístico** onde termos do domínio têm significado explícito e inequívoco
- Cada Contexto Delimitado tem sua própria **Linguagem Ubíqua**
- Conceitos podem ter o mesmo nome mas significados diferentes entre contextos

### 3. Espaço do Problema vs Espaço da Solução

- **Espaço do Problema**: Desafios de negócio e capacidades requeridas (Subdomínios)
- **Espaço da Solução**: Implementação técnica (Contextos Delimitados)
- **Objetivo**: Alinhar um Subdomínio a um Contexto Delimitado quando possível

---

## Processo de Identificação

### Passo 1: Extrair Conceitos do Código

Analise a base de código para identificar:

1. **Entidades** (classes com identidade)
   - Procure por: `@Entity`, `class`, modelos de domínio
   - Foque em: Conceitos de negócio, não infraestrutura técnica

2. **Serviços** (operações de negócio)
   - Procure por: classes `Service`, `Manager`, `Handler`
   - Foque em: Lógica de negócio, não utilitários técnicos

3. **Casos de Uso** (fluxos de trabalho de negócio)
   - Procure por: classes `UseCase`, `Command`, `Handler`
   - Foque em: Processos de negócio, não operações CRUD

4. **Controllers/Resolvers** (pontos de entrada)
   - Procure por: `Controller`, `Resolver`, endpoints de API
   - Foque em: Capacidades de negócio expostas, não rotas técnicas

### Passo 2: Agrupar Conceitos por Linguagem Ubíqua

Para cada conceito, identifique:

1. **Contexto Primário de Linguagem**
   - A qual linguagem de negócio este conceito pertence?
   - Exemplo: `Subscription`, `Invoice`, `Payment` → linguagem de Faturamento
   - Exemplo: `Movie`, `Video`, `Episode` → linguagem de Conteúdo

2. **Relacionamentos de Conceitos**
   - Quais conceitos naturalmente pertencem juntos?
   - Quais conceitos referenciam uns aos outros?
   - Quais conceitos compartilham o mesmo vocabulário de negócio?

3. **Limites Linguísticos**
   - Onde o significado dos termos muda?
   - Exemplo: `User` no contexto de Identidade vs `Customer` no contexto de Faturamento

### Passo 3: Identificar Domínios

Um **Domínio** é identificado por:

- **Capacidade de Negócio Distinta**: Uma função principal de negócio
- **Valor de Negócio Independente**: Pode ser compreendido separadamente
- **Vocabulário Único (Linguagem Ubíqua)**: Tem sua própria terminologia

**Indicadores de um Domínio:**
- ✅ Múltiplas entidades relacionadas trabalhando juntas
- ✅ Serviços que operam em um conjunto coeso de conceitos
- ✅ Casos de uso que resolvem um problema específico de negócio
- ✅ Controllers que expõem uma capacidade distinta de negócio

**Domínios Comuns:**
- **Faturamento/Assinatura**: Pagamentos, faturas, planos, uso
- **Conteúdo/Catálogo**: Mídia, produtos, inventário
- **Identidade/Acesso**: Usuários, autenticação, autorização
- **Analytics/Relatórios**: Métricas, dashboards, insights
- **Notificações**: Mensagens, alertas, comunicações

### Passo 4: Identificar Subdomínios

Um **Subdomínio** é identificado por:

- **Função de Negócio Focada**: Um aspecto específico dentro de um domínio
- **Responsabilidades Distintas**: Diferentes de outros subdomínios
- **Ciclo de Vida Separado**: Pode evoluir independentemente

**Tipos de Subdomínios:**

#### Domínio Principal
- **Indicadores:**
  - Maior valor de negócio
  - Vantagem competitiva
  - Lógica de negócio complexa
  - Requer especialistas do domínio
  - Mudanças frequentes

#### Subdomínio de Suporte
- **Indicadores:**
  - Essencial mas não diferenciador
  - Específico do negócio (não genérico)
  - Apoia o Domínio Principal
  - Complexidade moderada

#### Subdomínio Genérico
- **Indicadores:**
  - Funcionalidade comum
  - Poderia ser comprado/terceirizado
  - Problema bem compreendido
  - Baixa diferenciação de negócio

**Exemplo: Domínio de Conteúdo**
- **Principal**: Algoritmo de recomendação de conteúdo (único para o negócio)
- **Suporte**: Moderação de conteúdo (regras específicas do negócio)
- **Genérico**: Transcodificação de vídeo (funcionalidade padrão)

### Passo 5: Medir Coesão

Coesão mede quão relacionados os conceitos estão dentro de um domínio/subdomínio.

#### Indicadores de Alta Coesão ✅
- Conceitos compartilham a mesma Linguagem Ubíqua
- Conceitos são frequentemente usados juntos
- Conceitos têm relacionamentos diretos de negócio
- Mudanças em um conceito afetam outros no mesmo grupo
- Conceitos resolvem o mesmo problema de negócio

#### Indicadores de Baixa Coesão ❌
- Conceitos de diferentes vocabulários de negócio misturados
- Conceitos raramente usados juntos
- Conceitos não têm relacionamento direto de negócio
- Mudanças em um conceito não afetam outros
- Conceitos resolvem diferentes problemas de negócio

#### Métricas de Coesão

1. **Coesão Linguística**
   - Conte termos compartilhados na Linguagem Ubíqua
   - Maior vocabulário compartilhado = maior coesão

2. **Coesão de Uso**
   - Analise chamadas de métodos, dependências, imports
   - Conceitos frequentemente usados juntos = maior coesão

3. **Coesão de Dados**
   - Analise relacionamentos de entidades, chaves estrangeiras
   - Relacionamentos diretos = maior coesão

4. **Coesão de Mudanças**
   - Analise histórico git, co-mudanças
   - Arquivos alterados juntos = maior coesão

---

## Regras de Detecção de Baixa Coesão

### Regra 1: Incompatibilidade Linguística
**Problema**: Conceitos usam diferentes vocabulários de negócio

**Detecção**:
- `User` (identidade) misturado com `Subscription` (faturamento)
- `Video` (conteúdo) misturado com `Invoice` (faturamento)
- `Payment` (faturamento) misturado com `Movie` (conteúdo)

**Ação**: Marcar como baixa coesão, sugerir separação

### Regra 2: Dependências Entre Domínios
**Problema**: Conceitos de diferentes domínios têm acoplamento forte

**Detecção**:
- Serviço do Domínio A instancia diretamente entidades do Domínio B
- Serviço do Domínio A tem lógica de negócio para o Domínio B
- Controller manipula conceitos de múltiplos domínios

**Ação**: Marcar como baixa coesão, sugerir integração baseada em interface

### Regra 3: Responsabilidades Misturadas
**Problema**: Única classe/serviço manipula múltiplas preocupações de negócio

**Detecção**:
- Serviço que manipula tanto faturamento quanto conteúdo
- Entidade que representa múltiplos conceitos de negócio
- Caso de uso que orquestra múltiplos domínios

**Ação**: Marcar como baixa coesão, sugerir divisão

### Regra 4: Conceitos Genéricos no Domínio Principal
**Problema**: Funcionalidade genérica misturada com lógica principal de negócio

**Detecção**:
- Lógica de autenticação no gerenciamento de conteúdo
- Envio de email no serviço de faturamento
- Armazenamento de arquivo no serviço de domínio

**Ação**: Marcar como baixa coesão, sugerir extração para Subdomínio Genérico

### Regra 5: Limites Pouco Claros
**Problema**: Não é possível identificar claramente a qual domínio um conceito pertence

**Detecção**:
- Conceito usado em múltiplos contextos com significados diferentes
- Serviço que poderia pertencer a múltiplos domínios
- Entidade com relacionamentos a múltiplos domínios

**Ação**: Marcar como baixa coesão, sugerir clarificação de limites

---

## Estrutura do Mapa de Coesão

### Mapa de Coesão de Domínio

Crie um mapa mostrando:

```
Domínio: Faturamento
├── Subdomínio: Gerenciamento de Assinaturas (Principal)
│   ├── Conceitos: Subscription, Plan, SubscriptionStatus
│   ├── Coesão: Alta (9/10)
│   └── Dependências: → Identidade (User)
│
├── Subdomínio: Geração de Faturas (Suporte)
│   ├── Conceitos: Invoice, InvoiceLineItem, InvoiceStatus
│   ├── Coesão: Alta (8/10)
│   └── Dependências: → Gerenciamento de Assinaturas
│
└── Subdomínio: Processamento de Pagamentos (Genérico)
    ├── Conceitos: Payment, PaymentGateway, PaymentStatus
    ├── Coesão: Média (6/10)
    └── Dependências: → Geração de Faturas
```

### Mapa de Coesão Entre Domínios

Mostre relacionamentos entre domínios:

```
Domínio de Faturamento
├── Alta Coesão (9/10)
│   └── Subscription, Invoice, Payment
│
├── Média Coesão (6/10)
│   └── UsageRecord (faturamento) ↔ Content (conteúdo)
│
└── Baixa Coesão (2/10)
    └── User (identidade) ↔ Subscription (faturamento) [deveria usar interface]
```

### Cálculo de Pontuação de Coesão

```
Pontuação de Coesão = (
  Coesão Linguística (0-3) +
  Coesão de Uso (0-3) +
  Coesão de Dados (0-2) +
  Coesão de Mudanças (0-2)
) / 10

- 8-10: Alta Coesão ✅
- 5-7: Média Coesão ⚠️
- 0-4: Baixa Coesão ❌
```

---

## Lista de Verificação de Análise

### Para Cada Conceito Encontrado:

- [ ] A qual linguagem de negócio ele pertence?
- [ ] A qual domínio ele pertence?
- [ ] A qual subdomínio ele pertence?
- [ ] É Principal, Suporte ou Genérico?
- [ ] A quais outros conceitos ele se relaciona?
- [ ] De quais conceitos ele depende?
- [ ] As dependências estão dentro do mesmo domínio?
- [ ] Há incompatibilidades linguísticas?

### Para Cada Domínio Identificado:

- [ ] Qual é a Linguagem Ubíqua?
- [ ] Quais são os conceitos chave?
- [ ] Quais são os subdomínios?
- [ ] Qual é o Domínio Principal?
- [ ] Quais são as dependências de outros domínios?
- [ ] A coesão é alta dentro do domínio?
- [ ] Os limites estão claros?

### Para Análise de Coesão:

- [ ] Calcular pontuações de coesão para cada grupo
- [ ] Identificar áreas de baixa coesão
- [ ] Mapear dependências entre domínios
- [ ] Identificar incompatibilidades linguísticas
- [ ] Marcar acoplamento forte entre domínios
- [ ] Sugerir clarificações de limites

---

## Formato de Saída

### Mapa de Domínio

```markdown
## Domínio: {Nome do Domínio}

**Tipo**: Domínio Principal | Subdomínio de Suporte | Subdomínio Genérico

**Linguagem Ubíqua**: {termos chave}

**Conceitos**:
- {Conceito1} (Entidade/Serviço/Caso de Uso)
- {Conceito2} (Entidade/Serviço/Caso de Uso)
- ...

**Subdomínios**:
1. {Subdomínio1} (Principal/Suporte/Genérico)
   - Conceitos: {lista}
   - Coesão: {pontuação}/10
   
2. {Subdomínio2} (Principal/Suporte/Genérico)
   - Conceitos: {lista}
   - Coesão: {pontuação}/10

**Dependências**:
- → {OutroDomínio} (via {interface/conceito})
- ← {OutroDomínio} (via {interface/conceito})

**Problemas de Coesão**:
- ❌ {Descrição do problema}
- ⚠️ {Descrição do aviso}
```

### Matriz de Coesão

```markdown
## Matriz de Coesão

| Domínio A | Domínio B | Coesão | Tipo de Relacionamento |
|----------|-----------|----------|-------------------|
| Faturamento  | Identidade  | 2/10     | Baixa - Deveria usar interface |
| Conteúdo  | Faturamento   | 6/10     | Média - Rastreamento de uso |
| Conteúdo  | Identidade  | 3/10     | Baixa - Deveria usar interface |
```

### Relatório de Baixa Coesão

```markdown
## Problemas de Baixa Coesão

### Problema #1: Incompatibilidade Linguística
**Localização**: {arquivo/classe}
**Problema**: {descrição}
**Conceitos Envolvidos**: {lista}
**Correção Sugerida**: {recomendação}

### Problema #2: Acoplamento Entre Domínios
**Localização**: {arquivo/classe}
**Problema**: {descrição}
**Conceitos Envolvidos**: {lista}
**Correção Sugerida**: {recomendação}
```

---

## Exemplos

### Exemplo 1: Identificando Domínio de Faturamento

**Código Encontrado**:
```typescript
// Entidades
- Subscription
- Plan
- Invoice
- Payment
- Credit

// Serviços
- SubscriptionService
- InvoiceGeneratorService
- PaymentProcessorService

// Controllers
- SubscriptionController
- InvoiceController
```

**Análise**:
- **Linguagem Ubíqua**: subscription, plan, invoice, payment, billing, credit
- **Domínio**: Faturamento
- **Subdomínios**:
  - Gerenciamento de Assinaturas (Principal)
  - Geração de Faturas (Suporte)
  - Processamento de Pagamentos (Genérico)
- **Coesão**: Alta (8/10) - todos os conceitos compartilham vocabulário de faturamento

### Exemplo 2: Detecção de Baixa Coesão

**Código Encontrado**:
```typescript
// AuthService
class AuthService {
  async signIn(email, password) {
    const user = await this.userRepository.findOneByEmail(email);
    const isActive = await this.subscriptionService.isUserSubscriptionActive(user.id);
    // ...
  }
}
```

**Análise**:
- **Problema**: Serviço de autenticação depende diretamente do serviço de assinatura
- **Incompatibilidade Linguística**: Auth (identidade) misturado com Subscription (faturamento)
- **Coesão**: Baixa (3/10)
- **Correção Sugerida**: Use interface/API para verificar status de assinatura, não importe o serviço de faturamento diretamente

### Exemplo 3: Domínio de Conteúdo com Subdomínios

**Código Encontrado**:
```typescript
// Entidades
- Movie, TVShow, Episode, Video, Content

// Casos de Uso
- CreateMovieUseCase
- GetStreamingURLUseCase
- TranscribeVideoUseCase

// Serviços
- VideoProcessorService
- ContentDistributionService
```

**Análise**:
- **Domínio**: Conteúdo
- **Subdomínios**:
  - Gerenciamento de Conteúdo (Principal) - CreateMovie, Movie, TVShow
  - Entrega de Conteúdo (Suporte) - GetStreamingURL, ContentDistribution
  - Processamento de Vídeo (Genérico) - TranscribeVideo, VideoProcessor
- **Coesão**: 
  - Dentro dos subdomínios: Alta (8-9/10)
  - Entre subdomínios: Média (6/10) - apropriado para subdomínios

---

## Princípios Chave a Lembrar

1. **Foque na Linguagem de Negócio, Não na Estrutura do Código**
   - Não agrupe por camadas técnicas (controllers, serviços)
   - Agrupe por significado e vocabulário de negócio

2. **Linguagem Ubíqua é o Guia Principal**
   - Se conceitos usam termos diferentes de negócio, provavelmente são domínios diferentes
   - Se conceitos compartilham vocabulário de negócio, provavelmente são o mesmo domínio/subdomínio

3. **Coesão é Sobre Relacionamentos de Negócio**
   - Alta coesão = conceitos resolvem o mesmo problema de negócio
   - Baixa coesão = conceitos resolvem diferentes problemas de negócio

4. **Dependências Devem Ser Explícitas**
   - Dependências entre domínios devem usar interfaces/APIs
   - Imports diretos entre domínios indicam baixa coesão

5. **Subdomínios Podem Ter Diferentes Níveis de Coesão**
   - Domínio Principal deveria ter alta coesão
   - Subdomínios Genéricos podem ter coesão menor (frequentemente são utilitários)

6. **Não Sobre-Otimize**
   - Alguma comunicação entre domínios é necessária
   - Foque em identificar limites claros, não em eliminar todas as dependências

---

## Ferramentas e Técnicas

### Técnicas de Análise de Código

1. **Análise Estática**
   - Parse de imports/dependências
   - Análise de relacionamentos de classes
   - Mapeamento de relacionamentos de entidades

2. **Análise Semântica**
   - Extração de termos de negócio de nomes de classes/métodos
   - Identificação de padrões de Linguagem Ubíqua
   - Agrupamento por similaridade de vocabulário

3. **Análise de Dependências**
   - Construção de grafo de dependências
   - Identificação de acoplamento forte
   - Localização de dependências circulares

4. **Análise de Mudanças** (se histórico git disponível)
   - Análise de co-mudanças
   - Identificação de arquivos que mudam juntos
   - Localização de padrões de mudança

### Artefatos de Saída

1. **Mapa de Domínio**: Representação visual de domínios e subdomínios
2. **Matriz de Coesão**: Tabela mostrando pontuações de coesão
3. **Grafo de Dependências**: Grafo mostrando relacionamentos
4. **Relatório de Baixa Coesão**: Lista de problemas com recomendações
5. **Dicionário de Linguagem Ubíqua**: Termos por domínio

---

## Critérios de Validação

Uma boa identificação de domínio deveria ter:

✅ **Limites Claros**: Cada domínio tem Linguagem Ubíqua distinta
✅ **Alta Coesão Interna**: Conceitos dentro do domínio são intimamente relacionados
✅ **Dependências Explícitas**: Dependências entre domínios são claras
✅ **Alinhamento com Negócio**: Domínios correspondem a capacidades de negócio
✅ **Insights Acionáveis**: Problemas de baixa coesão têm recomendações claras

---

## Notas

- Esta análise é **estratégica**, não tática
- Foque em **quais** domínios existem, não **como** implementá-los
- **Mapas de coesão** ajudam a visualizar relacionamentos, não prescrevem arquitetura
- **Baixa coesão** nem sempre significa "ruim" - significa "precisa de atenção"
- Alguns domínios naturalmente têm coesão menor (ex: Subdomínios Genéricos)

---

## Referências

- **MODULAR-ARCHITECTURE-GUIDELINES.md**: Padrões de implementação (apenas para referência)
