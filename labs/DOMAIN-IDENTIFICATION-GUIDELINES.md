# Domain and Subdomain Identification Guidelines

## Purpose

This document provides guidelines for an AI agent to analyze codebases and identify:
- **Domains** and **Subdomains** following DDD Strategic Design principles
- **Low cohesion** indicators between concepts
- **Cohesion maps** showing relationships between domains and subdomains

**Important**: This agent focuses on **strategic design** (problem space), not implementation details (solution space). It identifies *what* domains exist, not *how* to structure modules.

---

## Core DDD Strategic Design Principles

### 1. Domain vs Subdomain

- **Domain**: The entire business area an organization operates in
- **Subdomain**: A distinct area of business functionality within the Domain
  - **Core Domain**: The primary business differentiator (highest priority)
  - **Supporting Subdomain**: Essential but not differentiating (specialized to business)
  - **Generic Subdomain**: Common functionality, not business-specific

### 2. Bounded Context

- A **linguistic boundary** where domain terms have explicit, unambiguous meaning
- Each Bounded Context has its own **Ubiquitous Language**
- Concepts may have the same name but different meanings across contexts

### 3. Problem Space vs Solution Space

- **Problem Space**: Business challenges and required capabilities (Subdomains)
- **Solution Space**: Technical implementation (Bounded Contexts)
- **Goal**: Align one Subdomain to one Bounded Context when possible

---

## Identification Process

### Step 1: Extract Concepts from Code

Analyze the codebase to identify:

1. **Entities** (classes with identity)
   - Look for: `@Entity`, `class`, domain models
   - Focus on: Business concepts, not technical infrastructure

2. **Services** (business operations)
   - Look for: `Service`, `Manager`, `Handler` classes
   - Focus on: Business logic, not technical utilities

3. **Use Cases** (business workflows)
   - Look for: `UseCase`, `Command`, `Handler` classes
   - Focus on: Business processes, not CRUD operations

4. **Controllers/Resolvers** (entry points)
   - Look for: `Controller`, `Resolver`, API endpoints
   - Focus on: Business capabilities exposed, not technical routes

### Step 2: Group Concepts by Ubiquitous Language

For each concept, identify:

1. **Primary Language Context**
   - What business language does this concept belong to?
   - Example: `Subscription`, `Invoice`, `Payment` → Billing language
   - Example: `Movie`, `Video`, `Episode` → Content language

2. **Concept Relationships**
   - Which concepts naturally belong together?
   - Which concepts reference each other?
   - Which concepts share the same business vocabulary?

3. **Linguistic Boundaries**
   - Where does the meaning of terms change?
   - Example: `User` in Identity context vs `Customer` in Billing context

### Step 3: Identify Domains

A **Domain** is identified by:

- **Distinct Business Capability**: A major business function
- **Independent Business Value**: Can be understood separately
- **Unique Vocabulary**: Has its own terminology

**Indicators of a Domain:**
- ✅ Multiple related entities working together
- ✅ Services that operate on a cohesive set of concepts
- ✅ Use cases that solve a specific business problem
- ✅ Controllers that expose a distinct business capability

**Common Domains:**
- **Billing/Subscription**: Payments, invoices, plans, usage
- **Content/Catalog**: Media, products, inventory
- **Identity/Access**: Users, authentication, authorization
- **Analytics/Reporting**: Metrics, dashboards, insights
- **Notifications**: Messages, alerts, communications

### Step 4: Identify Subdomains

A **Subdomain** is identified by:

- **Focused Business Function**: A specific aspect within a domain
- **Distinct Responsibilities**: Different from other subdomains
- **Separate Lifecycle**: Can evolve independently

**Types of Subdomains:**

#### Core Domain
- **Indicators:**
  - Highest business value
  - Competitive advantage
  - Complex business logic
  - Requires domain experts
  - Frequently changing

#### Supporting Subdomain
- **Indicators:**
  - Essential but not differentiating
  - Business-specific (not generic)
  - Supports Core Domain
  - Moderate complexity

#### Generic Subdomain
- **Indicators:**
  - Common functionality
  - Could be bought/outsourced
  - Well-understood problem
  - Low business differentiation

**Example: Content Domain**
- **Core**: Content recommendation algorithm (unique to business)
- **Supporting**: Content moderation (business-specific rules)
- **Generic**: Video transcoding (standard functionality)

### Step 5: Measure Cohesion

Cohesion measures how closely related concepts are within a domain/subdomain.

#### High Cohesion Indicators ✅
- Concepts share the same Ubiquitous Language
- Concepts are frequently used together
- Concepts have direct business relationships
- Changes to one concept affect others in the same group
- Concepts solve the same business problem

#### Low Cohesion Indicators ❌
- Concepts from different business vocabularies mixed together
- Concepts rarely used together
- Concepts have no direct business relationship
- Changes to one concept don't affect others
- Concepts solve different business problems

#### Cohesion Metrics

1. **Linguistic Cohesion**
   - Count shared terms in Ubiquitous Language
   - Higher shared vocabulary = higher cohesion

2. **Usage Cohesion**
   - Analyze method calls, dependencies, imports
   - Concepts frequently used together = higher cohesion

3. **Data Cohesion**
   - Analyze entity relationships, foreign keys
   - Direct relationships = higher cohesion

4. **Change Cohesion**
   - Analyze git history, co-changes
   - Files changed together = higher cohesion

---

## Low Cohesion Detection Rules

### Rule 1: Linguistic Mismatch
**Problem**: Concepts use different business vocabularies

**Detection**:
- `User` (identity) mixed with `Subscription` (billing)
- `Video` (content) mixed with `Invoice` (billing)
- `Payment` (billing) mixed with `Movie` (content)

**Action**: Flag as low cohesion, suggest separation

### Rule 2: Cross-Domain Dependencies
**Problem**: Concepts from different domains have tight coupling

**Detection**:
- Service from Domain A directly instantiates entities from Domain B
- Service from Domain A has business logic for Domain B
- Controller handles concepts from multiple domains

**Action**: Flag as low cohesion, suggest interface-based integration

### Rule 3: Mixed Responsibilities
**Problem**: Single class/service handles multiple business concerns

**Detection**:
- Service that handles both billing and content
- Entity that represents multiple business concepts
- Use case that orchestrates multiple domains

**Action**: Flag as low cohesion, suggest splitting

### Rule 4: Generic Concepts in Core Domain
**Problem**: Generic functionality mixed with core business logic

**Detection**:
- Authentication logic in content management
- Email sending in billing service
- File storage in domain service

**Action**: Flag as low cohesion, suggest extraction to Generic Subdomain

### Rule 5: Unclear Boundaries
**Problem**: Cannot clearly identify which domain a concept belongs to

**Detection**:
- Concept used in multiple contexts with different meanings
- Service that could belong to multiple domains
- Entity with relationships to multiple domains

**Action**: Flag as low cohesion, suggest clarifying boundaries

---

## Cohesion Map Structure

### Domain Cohesion Map

Create a map showing:

```
Domain: Billing
├── Subdomain: Subscription Management (Core)
│   ├── Concepts: Subscription, Plan, SubscriptionStatus
│   ├── Cohesion: High (9/10)
│   └── Dependencies: → Identity (User)
│
├── Subdomain: Invoice Generation (Supporting)
│   ├── Concepts: Invoice, InvoiceLineItem, InvoiceStatus
│   ├── Cohesion: High (8/10)
│   └── Dependencies: → Subscription Management
│
└── Subdomain: Payment Processing (Generic)
    ├── Concepts: Payment, PaymentGateway, PaymentStatus
    ├── Cohesion: Medium (6/10)
    └── Dependencies: → Invoice Generation
```

### Cross-Domain Cohesion Map

Show relationships between domains:

```
Billing Domain
├── High Cohesion (9/10)
│   └── Subscription, Invoice, Payment
│
├── Medium Cohesion (6/10)
│   └── UsageRecord (billing) ↔ Content (content)
│
└── Low Cohesion (2/10)
    └── User (identity) ↔ Subscription (billing) [should use interface]
```

### Cohesion Score Calculation

```
Cohesion Score = (
  Linguistic Cohesion (0-3) +
  Usage Cohesion (0-3) +
  Data Cohesion (0-2) +
  Change Cohesion (0-2)
) / 10

- 8-10: High Cohesion ✅
- 5-7: Medium Cohesion ⚠️
- 0-4: Low Cohesion ❌
```

---

## Analysis Checklist

### For Each Concept Found:

- [ ] What business language does it belong to?
- [ ] What domain does it belong to?
- [ ] What subdomain does it belong to?
- [ ] Is it Core, Supporting, or Generic?
- [ ] What other concepts does it relate to?
- [ ] What concepts does it depend on?
- [ ] Are dependencies within the same domain?
- [ ] Are there linguistic mismatches?

### For Each Domain Identified:

- [ ] What is the Ubiquitous Language?
- [ ] What are the key concepts?
- [ ] What are the subdomains?
- [ ] What is the Core Domain?
- [ ] What are the dependencies on other domains?
- [ ] Is cohesion high within the domain?
- [ ] Are boundaries clear?

### For Cohesion Analysis:

- [ ] Calculate cohesion scores for each group
- [ ] Identify low cohesion areas
- [ ] Map dependencies between domains
- [ ] Identify linguistic mismatches
- [ ] Flag cross-domain tight coupling
- [ ] Suggest boundary clarifications

---

## Output Format

### Domain Map

```markdown
## Domain: {Domain Name}

**Type**: Core Domain | Supporting Subdomain | Generic Subdomain

**Ubiquitous Language**: {key terms}

**Concepts**:
- {Concept1} (Entity/Service/UseCase)
- {Concept2} (Entity/Service/UseCase)
- ...

**Subdomains**:
1. {Subdomain1} (Core/Supporting/Generic)
   - Concepts: {list}
   - Cohesion: {score}/10
   
2. {Subdomain2} (Core/Supporting/Generic)
   - Concepts: {list}
   - Cohesion: {score}/10

**Dependencies**:
- → {OtherDomain} (via {interface/concept})
- ← {OtherDomain} (via {interface/concept})

**Cohesion Issues**:
- ❌ {Issue description}
- ⚠️ {Warning description}
```

### Cohesion Matrix

```markdown
## Cohesion Matrix

| Domain A | Domain B | Cohesion | Relationship Type |
|----------|-----------|----------|-------------------|
| Billing  | Identity  | 2/10     | Low - Should use interface |
| Content  | Billing   | 6/10     | Medium - Usage tracking |
| Content  | Identity  | 3/10     | Low - Should use interface |
```

### Low Cohesion Report

```markdown
## Low Cohesion Issues

### Issue #1: Linguistic Mismatch
**Location**: {file/class}
**Problem**: {description}
**Concepts Involved**: {list}
**Suggested Fix**: {recommendation}

### Issue #2: Cross-Domain Coupling
**Location**: {file/class}
**Problem**: {description}
**Concepts Involved**: {list}
**Suggested Fix**: {recommendation}
```

---

## Examples

### Example 1: Identifying Billing Domain

**Code Found**:
```typescript
// Entities
- Subscription
- Plan
- Invoice
- Payment
- Credit

// Services
- SubscriptionService
- InvoiceGeneratorService
- PaymentProcessorService

// Controllers
- SubscriptionController
- InvoiceController
```

**Analysis**:
- **Ubiquitous Language**: subscription, plan, invoice, payment, billing, credit
- **Domain**: Billing
- **Subdomains**:
  - Subscription Management (Core)
  - Invoice Generation (Supporting)
  - Payment Processing (Generic)
- **Cohesion**: High (8/10) - all concepts share billing vocabulary

### Example 2: Low Cohesion Detection

**Code Found**:
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

**Analysis**:
- **Problem**: Authentication service directly depends on subscription service
- **Linguistic Mismatch**: Auth (identity) mixed with Subscription (billing)
- **Cohesion**: Low (3/10)
- **Suggested Fix**: Use interface/API to check subscription status, don't import billing service directly

### Example 3: Content Domain with Subdomains

**Code Found**:
```typescript
// Entities
- Movie, TVShow, Episode, Video, Content

// Use Cases
- CreateMovieUseCase
- GetStreamingURLUseCase
- TranscribeVideoUseCase

// Services
- VideoProcessorService
- ContentDistributionService
```

**Analysis**:
- **Domain**: Content
- **Subdomains**:
  - Content Management (Core) - CreateMovie, Movie, TVShow
  - Content Delivery (Supporting) - GetStreamingURL, ContentDistribution
  - Video Processing (Generic) - TranscribeVideo, VideoProcessor
- **Cohesion**: 
  - Within subdomains: High (8-9/10)
  - Between subdomains: Medium (6/10) - appropriate for subdomains

---

## Key Principles to Remember

1. **Focus on Business Language, Not Code Structure**
   - Don't group by technical layers (controllers, services)
   - Group by business meaning and vocabulary

2. **Ubiquitous Language is the Primary Guide**
   - If concepts use different business terms, they're likely different domains
   - If concepts share business vocabulary, they're likely the same domain/subdomain

3. **Cohesion is About Business Relationships**
   - High cohesion = concepts solve the same business problem
   - Low cohesion = concepts solve different business problems

4. **Dependencies Should Be Explicit**
   - Cross-domain dependencies should use interfaces/APIs
   - Direct imports between domains indicate low cohesion

5. **Subdomains Can Have Different Cohesion Levels**
   - Core Domain should have high cohesion
   - Generic Subdomains may have lower cohesion (they're often utilities)

6. **Don't Over-Optimize**
   - Some cross-domain communication is necessary
   - Focus on identifying clear boundaries, not eliminating all dependencies

---

## Tools and Techniques

### Code Analysis Techniques

1. **Static Analysis**
   - Parse imports/dependencies
   - Analyze class relationships
   - Map entity relationships

2. **Semantic Analysis**
   - Extract business terms from class/method names
   - Identify Ubiquitous Language patterns
   - Group by vocabulary similarity

3. **Dependency Analysis**
   - Build dependency graph
   - Identify tight coupling
   - Find circular dependencies

4. **Change Analysis** (if git history available)
   - Analyze co-changes
   - Identify files that change together
   - Find change patterns

### Output Artifacts

1. **Domain Map**: Visual representation of domains and subdomains
2. **Cohesion Matrix**: Table showing cohesion scores
3. **Dependency Graph**: Graph showing relationships
4. **Low Cohesion Report**: List of issues with recommendations
5. **Ubiquitous Language Dictionary**: Terms per domain

---

## Validation Criteria

A good domain identification should:

✅ **Clear Boundaries**: Each domain has distinct Ubiquitous Language
✅ **High Internal Cohesion**: Concepts within domain are closely related
✅ **Explicit Dependencies**: Cross-domain dependencies are clear
✅ **Business Alignment**: Domains match business capabilities
✅ **Actionable Insights**: Low cohesion issues have clear recommendations

---

## Notes

- This analysis is **strategic**, not tactical
- Focus on **what** domains exist, not **how** to implement them
- **Cohesion maps** help visualize relationships, not prescribe architecture
- **Low cohesion** doesn't always mean "bad" - it means "needs attention"
- Some domains naturally have lower cohesion (e.g., Generic Subdomains)

---

## References

- **MODULAR-ARCHITECTURE-GUIDELINES.md**: Implementation patterns (for reference only)
