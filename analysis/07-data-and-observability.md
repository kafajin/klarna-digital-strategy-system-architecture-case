# 5. System Architecture Design - Data Platform & Observability (TO-BE)

## Purpose
Enable real-time decision-making, monitoring, and continuous improvement by building a scalable data and observability layer.

---

## Data Platform Overview

### 1) Operational Datastores
Each microservice owns its own database (no shared database).

**Examples:**
- Payments Service → transaction database  
- Risk Service → credit decision data  
- Merchant Service → merchant configuration data  

**Why this matters:**
- Avoids tight coupling between services  
- Improves scalability and performance  
- Enables independent deployments  

---

### 2) Event Store (Event Streaming Platform)

All critical system events are published to an event streaming platform (e.g., Kafka).

**Examples of events:**
- PaymentAuthorized  
- PaymentRejected  
- OrderCreated  
- RefundCreated  

**Why this matters:**
- Enables real-time data flow across the platform  
- Decouples services from each other  
- Supports analytics and monitoring  

---

### 3) Analytics Data Warehouse

All events and transactional data are aggregated into a central analytics layer.

**Used for:**
- Business intelligence (BI dashboards)  
- Revenue tracking  
- Customer behavior analysis  
- Risk and fraud model improvements  

**Why this matters:**
- Enables data-driven decision making  
- Supports strategic planning  
- Creates a single source of truth  

---

## Observability Layer

### 4) Monitoring

Tracks system health in real time.

**Examples:**
- API latency  
- Error rates  
- Service uptime  
- Transaction throughput  

**Tools (example):**
- Prometheus / Grafana  

---

### 5) Logging

Captures detailed logs from all services.

**Used for:**
- Debugging issues  
- Auditing transactions  
- Incident investigation  

---

### 6) Distributed Tracing

Tracks a single request across multiple services.

**Example:**
Customer checkout → Payments → Risk → Fraud → Banking  

**Why this matters:**
- Helps identify bottlenecks  
- Improves debugging across microservices  

---

### 7) Security & Compliance Monitoring

Ensures compliance with regulations (e.g., GDPR, financial regulations).

**Includes:**
- Access control monitoring  
- Audit logs  
- Data protection tracking  

---

## Real-Time Analytics Flow

1. User performs an action (e.g., checkout)  
2. Event is published to Event Bus  
3. Event is stored in Event Store  
4. Data flows into Analytics Warehouse  
5. Dashboards and models are updated in near real-time  

---

## Why this layer is critical for Klarna

- **Real-time insights:** instant visibility into payments and risk  
- **Better decisions:** data-driven pricing, fraud detection, and UX improvements  
- **Scalability:** supports global transaction volume growth  
- **Reliability:** issues detected early through monitoring  

---

## Link to TO-BE architecture

This layer supports:
- Event-driven architecture (Event Bus)  
- Microservices (independent data ownership)  
- Continuous improvement through analytics  
