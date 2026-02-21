# 5. System Architecture Design - Target Microservices (TO-BE)

## Purpose
Break down Klarna’s core capabilities into domain-based services that can scale independently, be deployed separately, and support faster product innovation.

## Microservices Overview (Target)

### 1) Orders / Checkout Service
**Owns:** checkout session, order lifecycle, payment method selection, merchant checkout configuration.  
**Why it matters:** keeps the user experience stable while allowing payment capabilities to evolve behind the scenes.

**Key interactions:**
- Calls **Payments Service** to execute a transaction
- Publishes events like `CheckoutStarted` and `OrderCreated`

---

### 2) Payments Service (Core Orchestrator)
**Owns:** payment execution, payment orchestration, settlement initiation, refunds/chargebacks triggers, and transaction state.  
**Why it matters:** this is the core system that must scale and stay reliable during peak traffic.

**Key interactions:**
- Requests decisioning from **Risk & Credit Service**
- Requests screening from **Fraud Service**
- Publishes events like `PaymentAuthorized`, `PaymentRejected`, `RefundCreated`

---

### 3) Risk & Credit Service
**Owns:** credit decisioning and affordability logic for BNPL (approve/decline, limits, conditions).  
**Why it matters:** supports global expansion with market-specific risk models and policies.

**Key interactions:**
- Receives a credit assessment request from **Payments Service**
- Returns `Approved / Declined` + decision reasons (where possible)

---

### 4) Fraud Service
**Owns:** fraud screening, risk signals, rules/models, and real-time scoring for suspicious behavior.  
**Why it matters:** fraud protection must evolve continuously without slowing down the checkout.

**Key interactions:**
- Receives fraud screening requests from **Payments Service**
- Publishes signals/events like `FraudFlagged` (optional)

---

### 5) Merchant Service
**Owns:** merchant profile, onboarding status, configuration, enabled products per market, technical integration metadata.  
**Why it matters:** enables self-service onboarding and faster merchant activation across countries.

**Key interactions:**
- Used by **Orders/Checkout Service** to apply merchant configuration
- Used by **Developer Portal / Integration Hub** for onboarding workflows

---

### 6) User Identity & Accounts
**Owns:** identity, authentication/authorization, customer accounts, account-level preferences.  
**Why it matters:** security and compliance depend on strong identity foundations.

**Key interactions:**
- Provides identity context to other services (via tokens/claims)
- Supports auditing and access control

---

### 7) Notifications Service
**Owns:** customer/merchant notifications (email/SMS/in-app), event-triggered messaging, templates.  
**Why it matters:** keeps communication consistent while services publish events.

**Key interactions:**
- Subscribes to events from the **Event Bus**
- Sends confirmations like payment receipt, order update, onboarding completion

---

## Why this microservices setup enables the strategy
- **Scalability:** Payments, Risk, and Fraud can scale independently during peaks.
- **Speed:** teams can ship improvements without coordinating a monolithic release.
- **Reliability:** failures are isolated (smaller blast radius).
- **Global expansion:** merchant and risk policies can be localized per market.

## Minimal dependency model (clean & stable)
- Checkout → Payments
- Payments → Risk/Credit
- Payments → Fraud
- Payments → Event Bus
- Notifications → Event Bus
