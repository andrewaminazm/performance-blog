---
title: API performance testing — a complete guide
description: How to load test REST and GraphQL APIs, what metrics matter, and how to interpret results.
pubDate: 2025-02-24
tags: [api, getting-started, process]
---

## Why API performance testing matters

Most modern apps are API-first. Slow or fragile APIs mean slow UIs, mobile apps, and integrations. Load testing your APIs directly gives you fast feedback without the overhead of browser-based testing.

---

## Key metrics for APIs

| Metric | What it tells you |
|--------|------------------|
| **Response time (p95, p99)** | Tail latency — the worst experience for most users |
| **Throughput (RPS)** | How many requests per second the API handles |
| **Error rate** | % of 4xx/5xx/timeouts under load |
| **Concurrency** | How many parallel connections the API supports |
| **Time to First Byte (TTFB)** | Server processing speed |

---

## Step-by-step: load testing an API

### 1. Pick your endpoints
Focus on:
- **Authentication** (login, token refresh)
- **Read-heavy** endpoints (search, list, get)
- **Write-heavy** endpoints (create, update, delete)
- **High-risk** endpoints (checkout, payment, report generation)

### 2. Set your targets (SLA)
- p95 response time < 200 ms
- Error rate < 0.1%
- Sustain 500 RPS per instance

### 3. Script with your tool
- **k6** — JavaScript, lightweight, great for APIs
- **JMeter** — GUI-based, supports complex flows
- **Postman (Newman)** — good for quick contract + light load checks

### 4. Run scenarios
- **Baseline**: 10 VUs, 2 min — confirm everything works
- **Load**: target VUs, 10–30 min — check SLAs
- **Stress**: ramp beyond target — find breaking point
- **Spike**: sudden jump in users — test flash traffic

### 5. Analyze
- Are p95 and p99 within SLA?
- Does error rate increase with load?
- Which endpoint is slowest?
- Correlate with DB slow queries, CPU, memory

---

## Common API performance issues

- **N+1 queries** — one API call triggers many DB queries
- **Missing pagination** — large payloads slow everything down
- **No caching** — repeated expensive queries
- **Synchronous blocking** — long operations block the thread pool
- **No rate limiting** — system can be overwhelmed by a single client

---

## Tools

See the [Tools page](/tools/) for step-by-step guides on [JMeter](/tools/), [k6](/tools/), and [Postman](/tools/) — including copy-paste scripts, parameter handling, and token auth for API tests.
