---
title: Performance testing for microservices
description: How to load test distributed systems, identify inter-service bottlenecks, and test resilience.
pubDate: 2025-02-24
tags: [microservices, process, advanced]
---

## Why microservices are different

In a monolith, one slow function affects one service. In microservices, one slow service affects every service that calls it — through the entire call chain. Performance testing for microservices means testing **individual services**, **service-to-service calls**, and the **system as a whole**.

---

## Three levels of testing

| Level | What you test | Tool |
|-------|--------------|------|
| **Unit / service** | One service in isolation | k6, JMeter, Postman |
| **Integration** | Service A → Service B chain | k6, JMeter |
| **End-to-end** | Full user flow across all services | k6, Gatling, JMeter |

---

## Step-by-step

### 1. Map your service dependencies
Before testing, draw the call chain:
- Which services does each API call touch?
- Where are the synchronous calls (slow = blocking)?
- Where are async calls (queues, events)?

### 2. Load test each service in isolation
- Start with individual services to set a baseline
- Identify the slowest service — it's the weak link
- Use [k6](/tools/) or [JMeter](/tools/) to hit each service's API directly

### 3. Test the full chain
- Run load through your API gateway or front-facing service
- Watch latency accumulate across the chain (tracing tools like Jaeger or Zipkin help here)

### 4. Test resilience
- What happens when Service B is slow? Does Service A timeout gracefully or cascade?
- Test circuit breakers, retries, and fallbacks under load

### 5. Monitor distributed metrics
- **Distributed tracing**: Jaeger, Zipkin, AWS X-Ray — trace a single request through all services
- **Per-service latency**: p95 for each service in the chain
- **Queue depth**: if you use Kafka or RabbitMQ, watch lag under load
- **Resource usage per service**: CPU, memory, connections for each instance

---

## Common microservices performance problems

- **Cascading slowdowns** — one slow service delays all callers
- **Chatty services** — too many small inter-service calls instead of batching
- **No timeout / circuit breaker** — slow dependency hangs the caller
- **Shared database** — two services competing for the same DB connections
- **Service discovery overhead** — DNS or sidecar proxy latency under load
- **Cold starts** — serverless or auto-scaled services that haven't warmed up

---

## Related

- [API performance testing](/blog/api-performance-testing/) — testing individual services
- [Performance testing in CI/CD](/blog/performance-testing-cicd/) — automate per-service tests in pipelines
- [Common mistakes in performance testing](/blog/common-mistakes-performance-testing/) — what to avoid
