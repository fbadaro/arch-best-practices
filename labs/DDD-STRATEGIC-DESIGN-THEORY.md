# DDD Strategic Design Theory

> **Purpose**: This document provides a condensed, LLM-optimized summary of Domain-Driven Design Strategic Design principles. Use this as the theoretical foundation for domain identification and boundary analysis.

## Core Definitions

### Domain

A **Domain** is what an organization does and the world it operates in.

- **Business Definition**: The entire realm of business operations, know-how, and methods
- **Technical Definition**: The problem space that software is being built to address
- **Key Insight**: The term "domain" is overloaded - it can refer to the entire business domain OR a specific area (Core Domain, Subdomain)

```
Domain = Organization's business realm + How it operates + Its unique knowledge
```

### Subdomain

A **Subdomain** is a distinct area within the overall business domain.

- **Purpose**: Divide the complexity of the entire domain into manageable parts
- **Key Insight**: Every software domain has multiple subdomains representing different business functions
- **Rule**: Separate different business functions to avoid complexity accumulation

```
Domain = Subdomain₁ + Subdomain₂ + Subdomain₃ + ... + Subdomainₙ
```

#### Types of Subdomains

| Type | Description | Business Value | Team Priority | Example |
|------|-------------|----------------|---------------|---------|
| **Core Domain** | Primary importance to business success. Provides competitive advantage. | Highest | Best developers, domain experts | Forecasting algorithm for a retail company |
| **Supporting Subdomain** | Essential but not differentiating. Business-specific, supports Core. | Medium | Competent developers | Inventory management |
| **Generic Subdomain** | Common functionality. Could be bought/outsourced. Well-understood problem. | Lower | Can outsource | Authentication, Email sending |

**Decision Matrix for Subdomain Classification:**

```
Is it a competitive advantage? 
  YES → Core Domain
  NO → Does it require business-specific knowledge?
         YES → Supporting Subdomain
         NO → Generic Subdomain
```

### Bounded Context

A **Bounded Context** is an explicit boundary within which a domain model exists.

- **Primary Nature**: A **linguistic boundary** where terms have specific meanings
- **Key Rule**: Inside the boundary, all terms of the Ubiquitous Language have unambiguous meaning
- **Contains**: Domain model, database schema (if designed for the model), UI components, Application Services, Web services

> **Context is King**: The same term can have completely different meanings in different contexts. Example: "Account" in Banking Context vs "Account" in Literary Context.

```
Bounded Context = Explicit Linguistic Boundary + Domain Model + Supporting Infrastructure
```

## Problem Space vs Solution Space

### Problem Space

- **Definition**: The parts of the Domain that need to be developed/addressed
- **Composition**: Core Domain + Supporting Subdomains that it needs
- **Purpose**: Strategic assessment of business challenges
- **Tool**: Subdomains (useful for rapidly viewing different parts)

### Solution Space

- **Definition**: The specific software models that realize the solution
- **Composition**: One or more Bounded Contexts
- **Purpose**: Actual software implementation
- **Tool**: Bounded Contexts (specific software realization)

```
Problem Space (what to solve) → assessed using → Subdomains
                ↓ transforms into ↓
Solution Space (how to solve) → realized using → Bounded Contexts
```

**Ideal Goal**: Align Subdomains 1:1 with Bounded Contexts (expressly segregates domain models by business objective).

**Reality**: In legacy systems, Subdomains often intersect multiple Bounded Contexts, or one Bounded Context contains multiple implicit models.

## Ubiquitous Language

The **Ubiquitous Language** is the shared vocabulary between developers and domain experts.

### Key Principles

1. **Language Drives Boundaries**: Bounded Context boundaries are primarily linguistic
2. **Same Term, Different Meaning**: When terms have different meanings, they belong in different contexts
3. **No Global Definitions**: Don't attempt to create single global meanings for all concepts
4. **Embrace Differences**: Use Bounded Contexts to delineate where differences are explicit

### Linguistic Boundary Detection

**Signs that concepts belong in DIFFERENT Bounded Contexts:**

- Same term used with different properties/behaviors
- Same term used in different stages of a lifecycle
- Domain experts use different vocabulary for similar concepts
- Concepts have different relationships depending on context

**Example - "Customer" in E-Commerce:**

| Context | Customer Meaning |
|---------|------------------|
| Catalog Browsing | Previous purchases, loyalty, available products, discounts |
| Order Placement | Name, ship-to address, bill-to address, payment terms |

→ These are different concepts that share a name = different Bounded Contexts

**Example - "Book" in Publishing:**

| Lifecycle Stage | Book Definition |
|-----------------|-----------------|
| Conceptualization | Proposed idea, potential author |
| Contract | Tentative title, author agreement |
| Editorial | Drafts, comments, corrections, final draft |
| Production | Page layouts, press images, plates |
| Marketing | Cover art, descriptions |
| Shipping | Identity, inventory location, size, weight |

→ Each stage has a different "Book" model = separate Bounded Contexts

## Strategic Design Anti-Patterns

### Big Ball of Mud

**Definition**: A haphazard, spaghetti-code system where everything is connected to everything else.

**Causes:**

- Mixing different business vocabularies in one model
- Not separating linguistic boundaries
- Blending Core Domain with Generic concerns
- Lack of explicit Bounded Contexts

**Prevention:**

- Understand your Domain and Subdomains
- Create explicit Bounded Context boundaries
- Separate linguistic concerns

### All-Inclusive Model (Enterprise Model)

**Definition**: Attempting to create a single, cohesive model of an organization's entire business domain.

**Why It Fails:**

- Impossible to establish global agreement on all concept meanings
- Different stakeholders have different perspectives
- Enduring global definitions are unlikely
- Creates conflicts and contention

**Solution**: Embrace that differences always exist. Use Bounded Contexts to separately delineate each domain model.

### Mixed Linguistic Concepts

**Example from SaaSOvation:**

```
❌ WRONG: Forum, Post, Discussion coupled with User, Permission
   - User/Permission are identity/security concepts
   - Forum/Post/Discussion are collaboration concepts
   - These don't harmonize in the Ubiquitous Language of Collaboration

✅ CORRECT: Forum, Post, Discussion coupled with Author, Moderator, Participant
   - All concepts have linguistic association to collaboration
   - Identity concepts belong in Identity and Access Context
```

**Rule**: Every concept in a Bounded Context should have a linguistic association to that context's domain.

## Size of Bounded Contexts

### Guiding Principle

> A Bounded Context should be as big as it needs to be to fully express its complete Ubiquitous Language.

### Mozart Principle

> "There are just as many notes as I required, neither more nor less."

- Not too small (gaping holes from missing concepts)
- Not too big (muddy waters from extraneous concepts)

### What to Include

- ✅ Concepts that are part of the Ubiquitous Language
- ✅ Concepts that domain experts describe as related
- ✅ Components that naturally fit in a cohesive model

### What to Exclude

- ❌ Concepts not in your Ubiquitous Language
- ❌ Extraneous concepts not truly part of the Core Domain
- ❌ Generic functionality that belongs in Supporting/Generic Subdomains

### Wrong Reasons to Size Bounded Contexts

| Wrong Reason | Why It's Wrong |
|--------------|----------------|
| Architectural components | Technical components don't define linguistic boundaries |
| Developer task distribution | Enforcing boundaries for task management plays false to linguistic motivations |
| Platform/framework conventions | Infrastructure shouldn't drive domain boundaries |

**Alternative for Task Distribution**: Use Modules within a Bounded Context to divide developer responsibilities.

## Integration Between Bounded Contexts

### Key Insights

1. **Bounded Contexts rarely stand alone**: Even large systems can't do everything
2. **Integration is necessary**: Different models must work together
3. **Mapping is required**: When integrating, translation must occur between Bounded Contexts
4. **Shared identity, different models**: Objects may share identity across contexts but have different properties

### Integration Example

```
User (Identity Context) + Role (Identity Context)
           ↓ Integration + Translation ↓
      Moderator (Collaboration Context)
```

- User attributes are used to create a Moderator
- But Moderator is a different concept with different properties
- The translation respects both contexts' Ubiquitous Languages

## Assessment Questions

### Problem Space Assessment

1. What is the name of and vision for the strategic Core Domain?
2. What concepts should be considered part of the Core Domain?
3. What are the necessary Supporting Subdomains and Generic Subdomains?
4. Who should do the work in each area of the domain?
5. Can the right teams be assembled?

### Solution Space Assessment

1. What software assets already exist, and can they be reused?
2. What assets need to be acquired or created?
3. How are all of these connected to each other, or integrated?
4. What additional integration will be needed?
5. Where are the terms of the Ubiquitous Languages completely different?
6. Where is there overlap and sharing of concepts between Bounded Contexts?
7. How are shared terms mapped and translated between Bounded Contexts?
8. Which Bounded Context contains the Core Domain concepts?

## Practical Application for Code Analysis

### Identifying Domains in Code

1. **Look for Entity clusters**: Groups of related entities with shared vocabulary
2. **Analyze Service responsibilities**: What business operations do they perform?
3. **Check Use Case scope**: What business problems do they solve?
4. **Examine Controller groupings**: What capabilities do they expose?

### Detecting Wrong Boundaries

| Signal | Interpretation |
|--------|----------------|
| Same class used in multiple contexts with identical properties | Potential modeling error (unless Shared Kernel) |
| Concepts with different vocabularies in same module | Mixed linguistic boundaries |
| Service handling multiple business domains | Low cohesion, should be split |
| Entity with relationships to unrelated domains | Unclear boundaries |

### Validating Correct Boundaries

| Signal | Interpretation |
|--------|----------------|
| All concepts share business vocabulary | Good linguistic cohesion |
| Concepts have properties/operations specific to context | Proper separation |
| Clear integration points with other contexts | Explicit boundaries |
| Domain experts can describe concepts without confusion | Healthy Ubiquitous Language |

## Summary: Key Takeaways for LLMs

1. **Domain** = Business realm. **Subdomain** = Specific business function within it.
2. **Bounded Context** = Linguistic boundary where terms have specific, unambiguous meanings.
3. **Ubiquitous Language** drives boundaries, NOT technical architecture.
4. **Core Domain** = Competitive advantage. **Supporting** = Essential but not differentiating. **Generic** = Can outsource.
5. **Problem Space** (Subdomains) → **Solution Space** (Bounded Contexts).
6. **Anti-patterns**: Big Ball of Mud, All-Inclusive Model, Mixed Linguistic Concepts.
7. **Context size**: Not determined by architecture or task distribution, but by completeness of Ubiquitous Language.
8. **Integration**: Bounded Contexts must integrate; mapping/translation is required.
9. **Same term, different context** = Different concept = Different Bounded Context.
10. **Always ask domain experts** to understand linguistic boundaries.
