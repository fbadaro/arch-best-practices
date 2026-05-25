# Domain Identification Example

This document shows a practical example of how to use the Domain Identification Guidelines to analyze a codebase.

## Example: Analyzing the Monolith Module

### Step 1: Extract Concepts

**Entities Found:**
- `User`, `Subscription`, `Plan`, `Invoice`, `Payment`, `Charge`, `Credit`, `Discount`
- `Movie`, `TVShow`, `Episode`, `Video`, `Content`, `Thumbnail`, `VideoMetadata`
- `UsageRecord`, `AddOn`, `DunningAttempt`, `TaxCalculation`

**Services Found:**
- `SubscriptionService`, `InvoiceGeneratorService`, `PaymentProcessorService`
- `VideoProcessorService`, `ContentDistributionService`
- `AuthService`, `UserManagementService`

**Use Cases Found:**
- `CreateMovieUseCase`, `GetStreamingURLUseCase`
- `TranscribeVideoUseCase`, `GenerateSummaryForVideoUseCase`

**Controllers Found:**
- `SubscriptionController`, `InvoiceController`
- `AdminMovieController`, `MediaPlayerController`
- `AuthResolver`, `UserResolver`

### Step 2: Group by Ubiquitous Language

#### Group 1: Billing Language
**Terms**: subscription, plan, invoice, payment, billing, credit, discount, usage, charge, dunning, tax

**Concepts**:
- Entities: `Subscription`, `Plan`, `Invoice`, `Payment`, `Charge`, `Credit`, `Discount`, `UsageRecord`, `DunningAttempt`, `TaxCalculation`
- Services: `SubscriptionService`, `InvoiceGeneratorService`, `PaymentProcessorService`, `CreditManagerService`, `DiscountEngineService`, `TaxCalculatorService`, `UsageBillingService`, `DunningManagerService`
- Controllers: `SubscriptionController`, `InvoiceController`, `CreditController`, `UsageController`

**Analysis**: High linguistic cohesion - all terms relate to billing/financial operations

#### Group 2: Content Language
**Terms**: movie, tv show, episode, video, content, streaming, catalog, media

**Concepts**:
- Entities: `Movie`, `TVShow`, `Episode`, `Video`, `Content`, `Thumbnail`, `VideoMetadata`
- Services: `ContentDistributionService`, `EpisodeLifecycleService`, `ContentAgeRecommendationService`
- Use Cases: `CreateMovieUseCase`, `CreateTvShowUseCase`, `GetStreamingURLUseCase`
- Controllers: `AdminMovieController`, `AdminTvShowController`, `MediaPlayerController`

**Analysis**: High linguistic cohesion - all terms relate to content/media management

#### Group 3: Identity Language
**Terms**: user, authentication, authorization, access, identity

**Concepts**:
- Entities: `User`
- Services: `AuthService`, `UserManagementService`
- Controllers: `AuthResolver`, `UserResolver`

**Analysis**: High linguistic cohesion - all terms relate to identity/access management

#### Group 4: Video Processing Language
**Terms**: transcription, summary, recommendation, processing, moderation

**Concepts**:
- Services: `VideoProcessorService`
- Use Cases: `TranscribeVideoUseCase`, `GenerateSummaryForVideoUseCase`, `SetAgeRecommendationUseCase`

**Analysis**: Medium linguistic cohesion - processing operations, but could be part of Content domain

### Step 3: Identify Domains

#### Domain 1: Billing
**Type**: Core Domain (if subscription is key business) or Supporting Subdomain

**Ubiquitous Language**: subscription, plan, invoice, payment, billing, credit, discount, usage, charge, dunning, tax

**Key Concepts**:
- Subscription Management: `Subscription`, `Plan`, `SubscriptionService`
- Invoice Generation: `Invoice`, `InvoiceLineItem`, `InvoiceGeneratorService`
- Payment Processing: `Payment`, `PaymentGatewayClient`, `PaymentProcessorService`
- Usage Billing: `UsageRecord`, `UsageBillingService`
- Financial Management: `Credit`, `Discount`, `TaxCalculation`

**Subdomains**:
1. **Subscription Management** (Core/Supporting)
   - Concepts: `Subscription`, `Plan`, `SubscriptionService`
   - Cohesion: High (9/10)
   
2. **Invoice Generation** (Supporting)
   - Concepts: `Invoice`, `InvoiceLineItem`, `InvoiceGeneratorService`
   - Cohesion: High (8/10)
   - Dependencies: → Subscription Management
   
3. **Payment Processing** (Generic)
   - Concepts: `Payment`, `PaymentGatewayClient`
   - Cohesion: Medium (7/10)
   - Dependencies: → Invoice Generation
   
4. **Usage Billing** (Supporting)
   - Concepts: `UsageRecord`, `UsageBillingService`
   - Cohesion: Medium (6/10)
   - Dependencies: → Subscription Management, → Content (for usage tracking)

**Dependencies**:
- → Identity (User) - via `userId` reference
- ← Content (UsageRecord) - tracks content usage

**Cohesion Score**: 8/10 (High)

#### Domain 2: Content
**Type**: Core Domain (if content is key business)

**Ubiquitous Language**: movie, tv show, episode, video, content, streaming, catalog, media, thumbnail

**Key Concepts**:
- Content Management: `Movie`, `TVShow`, `Episode`, `Content`
- Content Delivery: `Video`, `GetStreamingURLUseCase`, `ContentDistributionService`
- Content Metadata: `VideoMetadata`, `Thumbnail`, `ContentAgeRecommendationService`

**Subdomains**:
1. **Content Management** (Core)
   - Concepts: `Movie`, `TVShow`, `Episode`, `Content`, `CreateMovieUseCase`
   - Cohesion: High (9/10)
   
2. **Content Delivery** (Supporting)
   - Concepts: `Video`, `GetStreamingURLUseCase`, `ContentDistributionService`
   - Cohesion: High (8/10)
   - Dependencies: → Content Management
   
3. **Content Processing** (Generic/Supporting)
   - Concepts: `VideoProcessorService`, `TranscribeVideoUseCase`, `GenerateSummaryForVideoUseCase`
   - Cohesion: Medium (7/10)
   - Dependencies: → Content Management

**Dependencies**:
- → Identity (User) - content ownership/access
- → Billing (UsageRecord) - content usage tracking

**Cohesion Score**: 8/10 (High)

#### Domain 3: Identity
**Type**: Generic Subdomain (common functionality)

**Ubiquitous Language**: user, authentication, authorization, access, identity

**Key Concepts**:
- User Management: `User`, `UserManagementService`
- Authentication: `AuthService`, `AuthResolver`

**Subdomains**:
1. **User Management** (Generic)
   - Concepts: `User`, `UserManagementService`, `UserResolver`
   - Cohesion: High (9/10)
   
2. **Authentication** (Generic)
   - Concepts: `AuthService`, `AuthResolver`
   - Cohesion: High (8/10)
   - Dependencies: → User Management

**Dependencies**:
- ← Billing (Subscription) - references `User`
- ← Content (Content) - references `User`

**Cohesion Score**: 9/10 (High)

### Step 4: Cohesion Analysis

#### Internal Cohesion (Within Domains)

| Domain | Cohesion Score | Status |
|--------|----------------|--------|
| Billing | 8/10 | ✅ High |
| Content | 8/10 | ✅ High |
| Identity | 9/10 | ✅ High |

#### Cross-Domain Cohesion

| Domain A | Domain B | Cohesion | Relationship | Issue? |
|----------|----------|----------|--------------|--------|
| Billing | Identity | 3/10 | `Subscription.userId` → `User.id` | ⚠️ Should use interface |
| Content | Identity | 3/10 | `Content.userId` → `User.id` | ⚠️ Should use interface |
| Content | Billing | 6/10 | `UsageRecord` tracks content usage | ✅ Appropriate (eventual consistency) |
| Billing | Content | 2/10 | `AuthService` directly calls `SubscriptionService` | ❌ Low cohesion - tight coupling |

### Step 5: Low Cohesion Issues

#### Issue #1: Cross-Domain Tight Coupling
**Location**: `AuthService.signIn()`
```typescript
const isActive = await this.subscriptionService.isUserSubscriptionActive(user.id);
```

**Problem**: 
- Authentication (Identity domain) directly depends on Subscription (Billing domain)
- Linguistic mismatch: Auth language mixed with Billing language
- Tight coupling prevents independent evolution

**Cohesion**: 2/10 (Very Low)

**Suggested Fix**: 
- Use interface/API to check subscription status
- Don't import `SubscriptionService` directly
- Use `BillingSubscriptionStatusApi` interface (already exists!)

#### Issue #2: Mixed Domain Responsibilities
**Location**: `AuthService`
**Problem**: 
- Service handles both authentication (Identity) and subscription validation (Billing)
- Should delegate subscription check to a separate concern

**Cohesion**: 3/10 (Low)

**Suggested Fix**:
- Keep authentication logic in Identity domain
- Use interface to check subscription status
- Consider: Is subscription check part of authentication or authorization?

#### Issue #3: Unclear Boundary
**Location**: `VideoProcessorService`
**Problem**:
- Video processing could be:
  - Part of Content domain (manages content)
  - Separate Generic Subdomain (utility service)
- Currently mixed with content management

**Cohesion**: 6/10 (Medium)

**Suggested Fix**:
- Clarify: Is video processing core to content business?
- If Generic: Extract to separate subdomain
- If Supporting: Keep in Content but separate from management

### Step 6: Cohesion Map

```
┌─────────────────────────────────────────────────────────────┐
│                    DOMAIN COHESION MAP                      │
└─────────────────────────────────────────────────────────────┘

┌─────────────────┐
│  IDENTITY       │  Cohesion: 9/10 ✅
│  (Generic)      │
│                 │
│  • User         │
│  • AuthService  │
│  • UserMgmt     │
└────────┬────────┘
         │
         │ userId reference (3/10 cohesion)
         │ ⚠️ Should use interface
         │
         ▼
┌─────────────────┐         ┌─────────────────┐
│  BILLING        │ 6/10    │  CONTENT        │  Cohesion: 8/10 ✅
│  (Core/Support) │◄────────┤  (Core)         │
│                 │         │                 │
│  • Subscription │         │  • Movie        │
│  • Invoice      │         │  • TVShow       │
│  • Payment      │         │  • Video        │
│  • UsageRecord  │         │  • Processor    │
└─────────────────┘         └─────────────────┘
         ▲
         │
         │ Direct service call (2/10 cohesion)
         │ ❌ Tight coupling
         │
┌────────┴────────┐
│  AuthService    │
│  (Identity)     │
└─────────────────┘

LEGEND:
✅ High Cohesion (8-10)
⚠️ Medium Cohesion (5-7)
❌ Low Cohesion (0-4)
```

### Step 7: Recommendations

#### High Priority
1. **Decouple AuthService from SubscriptionService**
   - Use `BillingSubscriptionStatusApi` interface
   - Remove direct import of `SubscriptionService`
   - Cohesion improvement: 2/10 → 6/10

#### Medium Priority
2. **Clarify Video Processing Boundary**
   - Decide: Core, Supporting, or Generic?
   - If Generic: Extract to separate subdomain
   - If Supporting: Keep in Content but organize better

3. **Improve Cross-Domain Interfaces**
   - All cross-domain references should use interfaces
   - No direct entity/service imports between domains
   - Use events or APIs for communication

#### Low Priority
4. **Document Ubiquitous Language**
   - Create dictionary of terms per domain
   - Ensure team uses consistent language
   - Review naming conventions

### Step 8: Final Domain Map

```markdown
## Domain: Billing
**Type**: Core Domain / Supporting Subdomain
**Cohesion**: 8/10 ✅
**Ubiquitous Language**: subscription, plan, invoice, payment, billing, credit, discount

**Subdomains**:
1. Subscription Management (Core/Supporting) - 9/10
2. Invoice Generation (Supporting) - 8/10
3. Payment Processing (Generic) - 7/10
4. Usage Billing (Supporting) - 6/10

**Dependencies**:
- → Identity (User) - 3/10 cohesion ⚠️
- ← Content (UsageRecord) - 6/10 cohesion ✅

---

## Domain: Content
**Type**: Core Domain
**Cohesion**: 8/10 ✅
**Ubiquitous Language**: movie, tv show, episode, video, content, streaming

**Subdomains**:
1. Content Management (Core) - 9/10
2. Content Delivery (Supporting) - 8/10
3. Content Processing (Generic/Supporting) - 7/10

**Dependencies**:
- → Identity (User) - 3/10 cohesion ⚠️
- → Billing (UsageRecord) - 6/10 cohesion ✅

---

## Domain: Identity
**Type**: Generic Subdomain
**Cohesion**: 9/10 ✅
**Ubiquitous Language**: user, authentication, authorization

**Subdomains**:
1. User Management (Generic) - 9/10
2. Authentication (Generic) - 8/10

**Dependencies**:
- ← Billing (Subscription) - 3/10 cohesion ⚠️
- ← Content (Content) - 3/10 cohesion ⚠️
```

---

## Summary

**Domains Identified**: 3
- Billing (Core/Supporting)
- Content (Core)
- Identity (Generic)

**Subdomains Identified**: 9
- 4 in Billing
- 3 in Content
- 2 in Identity

**Cohesion Issues**: 3
- 1 High Priority (AuthService coupling)
- 1 Medium Priority (Video Processing boundary)
- 1 Low Priority (Cross-domain interfaces)

**Overall Assessment**: 
- ✅ Good domain boundaries identified
- ⚠️ Some cross-domain coupling issues
- ✅ High internal cohesion within domains
- ⚠️ Need to improve cross-domain interfaces
