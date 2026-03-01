---

# High-Throughput Order Processing System

A distributed, event-driven order processing platform designed to handle **10,000–50,000+ orders per minute** with high reliability, scalability, and observability.

This project demonstrates advanced **system design, performance optimization, distributed architecture, and production-readiness** using modern Java.

---

## 🚀 Project Goals

* Design a horizontally scalable microservices system
* Compare synchronous vs asynchronous processing performance
* Implement resilience and fault tolerance patterns
* Measure throughput, latency, and resource usage
* Demonstrate production-grade observability and tuning

---

# 🏗 Architecture Overview

## High-Level Flow

Client → API Gateway → Order Service → Kafka → Inventory / Payment → Database → Notification

### Architecture Patterns Used

* Event-Driven Architecture
* Saga Pattern (Choreography-based)
* Outbox Pattern
* Circuit Breaker
* Idempotency Handling
* Cache-Aside Pattern
* Read/Write Separation

---

# 🧩 Services

## 1. API Gateway

Built with Spring Cloud Gateway (or custom lightweight gateway)

Responsibilities:

* Request routing
* JWT validation
* Rate limiting
* Request logging

---

## 2. Order Service

Handles order creation.

Responsibilities:

* Validate order
* Persist order (PostgreSQL)
* Publish `OrderCreatedEvent` to Kafka
* Implement idempotency key handling
* Use Outbox pattern for reliable publishing

---

## 3. Inventory Service

* Listens to `OrderCreatedEvent`
* Reserves inventory
* Publishes `InventoryReservedEvent`

---

## 4. Payment Service

* Listens to `InventoryReservedEvent`
* Simulates payment processing
* Publishes `PaymentCompletedEvent` or failure event

---

## 5. Notification Service

* Sends confirmation (simulated async email)

---

# ⚙️ Tech Stack

* Java 21
* Spring Boot
* Spring Data JPA
* PostgreSQL
* Redis
* Apache Kafka
* Docker & Docker Compose
* Micrometer
* Prometheus + Grafana
* Resilience4j
* JMH (Microbenchmarking)
* k6 (Load Testing)

---

# 📊 Performance Engineering

## Benchmark Scenarios

### Scenario 1 – Synchronous Flow

Order → Inventory → Payment (REST calls)

### Scenario 2 – Asynchronous Flow

Order → Kafka → Event-driven processing

---

## Metrics Collected

* Throughput (Requests/sec)
* p50 / p95 / p99 latency
* CPU usage
* Memory usage
* DB connection pool utilization
* Kafka consumer lag

---

## Sample Results (Example)

| Scenario | TPS   | p95 Latency | CPU Usage |
| -------- | ----- | ----------- | --------- |
| Sync     | 1,200 | 320ms       | 85%       |
| Async    | 8,500 | 90ms        | 65%       |

*(Replace with your actual benchmark data)*

---

# 🔥 Scalability Strategy

* Stateless services
* Horizontal scaling via Docker Compose replicas
* Kafka partitions for parallel processing
* Optimized HikariCP pool tuning
* Read replicas for reporting queries
* Redis caching for product lookups

---

# 🧠 Reliability Mechanisms

## Idempotency

Each order request includes an Idempotency-Key header.

Prevents duplicate order processing during retries.

---

## Circuit Breaker

Using Resilience4j:

* Protect downstream service calls
* Automatic fallback
* Failure threshold monitoring

---

## Outbox Pattern

Ensures:

* No lost events
* DB + Event consistency
* Reliable message publishing

---

# 📈 Observability

## Metrics

* Exposed via `/actuator/prometheus`
* Scraped by Prometheus
* Visualized in Grafana dashboards

Tracked metrics:

* HTTP request duration
* Kafka processing time
* DB query time
* JVM memory
* GC pause time
* Thread pool usage

---

## Logging

* Structured JSON logs
* Correlation IDs
* Distributed trace IDs

---

# 🧪 Testing Strategy

* Unit tests (JUnit + Mockito)
* Integration tests (Testcontainers)
* Kafka integration tests
* Performance tests (k6)
* JMH benchmarks for critical code paths

---

# 🐳 Running the Project

## Prerequisites

* Docker
* Docker Compose
* Java 21

## Start All Services

```bash
docker-compose up --build
```

Services:

* PostgreSQL
* Redis
* Kafka
* Zookeeper
* All microservices
* Prometheus
* Grafana

---

# 📂 Repository Structure

```
order-processing-system/
 ├── api-gateway/
 ├── order-service/
 ├── inventory-service/
 ├── payment-service/
 ├── notification-service/
 ├── load-tests/
 ├── jmh-benchmarks/
 ├── docker-compose.yml
 └── architecture/
```

---

# 🧭 Architecture Decision Records (ADR)

Located in:

```
/architecture/adr/
```

Example decisions:

* Why Kafka over RabbitMQ
* Why choreography over orchestration
* Why Redis cache-aside strategy
* Why PostgreSQL instead of NoSQL

---

# 📌 Key Engineering Learnings

* Async processing drastically improves throughput
* DB connection pool tuning is critical under load
* Idempotency is essential in distributed systems
* Kafka partition count impacts scaling ceiling
* Observability is mandatory for performance tuning

---

# 🎯 What This Project Demonstrates

* Distributed systems design
* Event-driven architecture
* Performance benchmarking
* Production-grade observability
* Scalability engineering
* Clean architecture principles
* Resilience patterns
* Senior-level backend thinking

---

# 📎 Future Improvements

* Kubernetes deployment
* Auto-scaling policies
* Chaos testing
* Blue/green deployment
* Multi-region support

