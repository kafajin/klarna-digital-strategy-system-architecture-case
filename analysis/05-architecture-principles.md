# 5. System Architecture Design — Principles (TO-BE)

## Goal
Design a target architecture that supports faster merchant onboarding, global scalability, and faster product innovation — while improving reliability and compliance.

## Target Architecture Principles

### 1) API-first & Developer Experience
All external and internal clients integrate through an API Gateway. A Developer Portal enables self-service onboarding (docs, sandbox, keys), reducing integration lead time.

### 2) Domain-based Microservices
Core capabilities are separated into domain services (Payments, Risk, Fraud, Orders/Checkout, Merchant, Identity, Notifications). This reduces coupling and enables independent scaling and deployment.

### 3) Event-driven Communication
Critical business events (payment authorized, risk approved, refund created) are published to an Event Bus. This enables real-time analytics and reduces point-to-point integrations.

### 4) Resilience by Design
Services are built with timeouts, retries, circuit breakers and graceful degradation to reduce customer impact during partial outages.

### 5) Security & Compliance Built-in
Identity, access control, encryption, auditing and compliance requirements are treated as platform capabilities — not afterthoughts.

## How this maps to the TO-BE diagram
- API-first: API Gateway + Developer Portal
- Event-driven: Event Bus feeding analytics and services
- Scalability: microservices + per-service datastores
- Reliability: monitoring + tracing across services
