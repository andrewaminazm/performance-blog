---
title: How to analyze performance test results
description: Interpret metrics, find bottlenecks, and report findings.
pubDate: 2025-02-23
tags: [metrics, process]
---

## What to look at first

1. **Error rate** — If &gt; 0%, find why (timeouts, 5xx, connection errors).
2. **Throughput (RPS)** — Did you hit the target? Flat or dropping over time?
3. **Latency percentiles** — p95, p99: are they within [SLA](/blog/setting-performance-slas/)? Any spike in the middle of the run?
4. **Trend over time** — Response time and RPS over the test duration (e.g. from [JMeter](/tools/) HTML report or [k6](/tools/) output).

---

## Interpreting common patterns

### High error rate

- **Timeouts:** Server or DB too slow; increase timeout or fix the bottleneck.
- **5xx:** Application or infra failure; check logs and resource usage.
- **Connection refused / reset:** Server or load generator limit (e.g. file descriptors, connection pool).

### Latency increasing over time

- **Memory leak:** Server memory growing; run an [endurance test](/blog/load-vs-stress-vs-spike-testing/).
- **Resource exhaustion:** DB connections, thread pool; check server metrics.

### Throughput flat while increasing load

- **Saturation:** One resource (CPU, DB, disk) is maxed; that’s often the bottleneck.
- **Contention:** Locks or serialization; profile the app.

### Big gap between p50 and p99

- **Tail latency:** A subset of requests is slow (e.g. cold caches, slow DB queries). Optimize the slow path or add capacity.

---

## Correlate with server metrics

- **CPU, memory, disk I/O** on app and DB servers during the test.
- **Database:** slow queries, connection pool usage, replication lag.
- **Network:** packet loss, bandwidth.

When latency or errors spike, check **what changed** on the server at that time (e.g. CPU spike, GC pause).

---

## Reporting

- **Summary:** Pass/fail vs [SLAs](/blog/setting-performance-slas/); test config (VUs, duration, scenario).
- **Graphs:** Response time over time, throughput, error rate; use [JMeter](/tools/) HTML report, k6 output, or Grafana.
- **Findings:** What’s the bottleneck? What’s the recommended action (scale, optimize query, add cache)?
- **Baseline comparison:** “p95 was 120 ms last week, 180 ms this week” — document and track.

Use the same [metrics](/blog/performance-metrics-explained/) and [steps](/blog/performance-testing-steps/) every time so results are comparable across runs.
