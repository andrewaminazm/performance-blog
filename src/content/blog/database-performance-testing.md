---
title: Database performance testing
description: How to find and fix database bottlenecks during load tests — queries, connections, and indexing.
pubDate: 2025-02-24
tags: [database, metrics, process]
---

## Why the database is usually the bottleneck

When load tests show high latency or errors, the database is the most common culprit. Connection pool exhaustion, slow queries, missing indexes, and lock contention all show up under load — even when they're invisible at low traffic.

---

## What to monitor during a load test

| Metric | Healthy range | Warning sign |
|--------|--------------|--------------|
| **DB response time** | < 10 ms for simple queries | > 100 ms under load |
| **Connection pool usage** | < 70% | Near 100% → queuing |
| **Slow query count** | 0 or very low | Growing fast |
| **Lock waits** | Near 0 | Any significant value |
| **Replication lag** | < 1 s | Growing > a few seconds |
| **Cache hit ratio** | > 95% | Below 90% → index or cache issue |

---

## Step-by-step: database performance testing

### 1. Baseline your queries
Before running load tests, profile slow queries:
- **PostgreSQL**: `EXPLAIN ANALYZE` and `pg_stat_statements`
- **MySQL**: slow query log and `EXPLAIN`
- **MongoDB**: `explain()` and the Profiler

### 2. Enable monitoring during load tests
- Attach a monitoring tool (Prometheus + Grafana, Datadog, CloudWatch) to the DB
- Track: connections, query latency, cache hit ratio, lock waits

### 3. Run load tests against your APIs
Your API load tests (JMeter, k6) indirectly stress the DB. Watch:
- Does API latency increase as VUs go up?
- Does DB CPU spike?
- Are connection pool slots exhausted?

### 4. Simulate DB-heavy scenarios
- Many users running the same expensive report
- Concurrent writes to the same table
- Long-running transactions under load

### 5. Fix and retest
- Add or tune indexes
- Increase connection pool size (but don't just raise the limit — fix the root cause)
- Cache hot queries (Redis, Memcached)
- Break large queries into smaller ones

---

## Common database performance issues

- **Missing indexes** — full table scans at 10 users are fine; at 1000 users, they kill performance
- **Connection pool too small** — requests queue waiting for a connection
- **ORM N+1 queries** — one API call → hundreds of DB queries
- **Unbounded queries** — `SELECT *` with no `LIMIT` returns huge payloads
- **Lock contention** — concurrent writes on the same row or table
- **Replication lag** — reads from replicas return stale data under high write load

---

## Related

- [Performance metrics explained](/blog/performance-metrics-explained/) — cache hit ratio, connection pool, slow queries
- [How to analyze performance test results](/blog/analyzing-performance-results/) — correlating API latency with DB metrics
- [Common mistakes in performance testing](/blog/common-mistakes-performance-testing/) — what to avoid
