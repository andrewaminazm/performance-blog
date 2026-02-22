---
title: Performance metrics explained
description: Definitions of latency, throughput, error rate, and more.
pubDate: 2025-02-22
tags: [metrics, reference]
---

## Core metrics

### Latency / response time

Time from sending a request to receiving the response.

- **TTFB** (Time to First Byte): time until first byte of response; often used for “time to start” feel.
- **Total response time**: until last byte (or “time to last byte”); what users perceive as “how long the request took.”
- **Percentiles**: p50 (median), p90, p95, p99, p99.9 — e.g. “p95 = 150 ms” means 95% of requests finished in ≤ 150 ms. p99 and p99.9 show tail latency (worst-case experience).
- **Min / max / average**: simple stats; average can hide spikes, so prefer percentiles for SLAs.
- **Standard deviation**: spread of response times; high value means inconsistent performance.
- **Connection time**: time to establish TCP (and optionally TLS) before the first byte of the request.
- **Processing time**: server-side duration only (if reported separately from network time).

### Throughput

- **RPS** (requests per second): number of completed requests per second.
- **TPS** (transactions per second): business transactions per second (e.g. orders, checkouts).
- **Bytes per second (B/s)**: data transferred per second; useful for bandwidth and large payloads.
- **Hits per second**: often used for cache or CDN (successful cache hits per second).
- **Peak vs sustained throughput**: peak = max RPS in a short window; sustained = stable RPS over the test; both matter for capacity.

### Concurrency

- **Virtual users (VUs)** or **concurrent users**: number of simulated users/connections at a time.
- **Open connections**: active TCP/HTTP connections to the system.

### Error rate

- **Failed requests / total requests** (e.g. 4xx, 5xx, timeouts).
- Often expressed as **%** (e.g. 0.1% error rate).
- **Error rate by type**: separate 4xx, 5xx, timeouts, connection errors to find root cause.
- **Success rate**: (1 − error rate); sometimes used in SLAs (e.g. 99.9% success rate).

### Availability

- **Uptime** or **success rate**: % of requests that succeed (or health checks that pass).
- **MTBF** (Mean Time Between Failures): average time between failures in endurance tests.
- **MTTR** (Mean Time To Recovery): average time to recover after a failure or stress event.

---

## Infrastructure & saturation

### Resource utilization

- **CPU %**: processor usage on app and DB servers.
- **Memory**: used vs available (watch for leaks in long tests).
- **Disk I/O**: read/write throughput and latency.
- **Network**: bandwidth and packet loss.

### Saturation

- How “full” a resource is; high saturation often means queuing and slower response times.

### Load generator metrics

- **CPU and memory on the load generator**: if these are high, the load box may be the bottleneck, not the system under test.
- **Requests per VU/thread**: how many requests each virtual user completed; helps interpret throughput.

---

## Application & server metrics

- **Thread pool utilization**: % of app-server threads in use; high value can mean queuing.
- **Database connection pool**: active vs idle connections; exhaustion causes timeouts or slow queries.
- **Cache hit ratio**: hits / (hits + misses); higher is better for performance.
- **Queue depth / backlog**: items waiting (e.g. in a thread queue or message queue); growing queue often means overload.
- **GC (garbage collection)**: for JVM-based apps, pause time and frequency; long or frequent GC can spike latency.
- **Slow query count**: number of DB queries above a threshold (e.g. &gt; 1 s); indicates DB bottlenecks.

---

## Other useful metrics

- **Scalability**: how throughput and latency change as load (e.g. VUs) increases; “linear” vs “flattening” curve.
- **Baseline**: normal performance under expected load; use it to compare after changes or releases.
- **Recovery time**: how long the system takes to return to normal after stress or a spike.
- **Degradation**: % or factor by which latency or throughput worsens under higher load (e.g. “p95 2× under 2× load”).
- **Think time / pacing**: delay between requests per user; affects RPS and realism of the scenario.
- **Response size**: bytes per response; useful for bandwidth and payload optimization.
- **Connections per second**: rate of new TCP/TLS connections; high rate can stress the server even at moderate RPS.

Use these definitions in your test reports and SLAs. For tool-specific metrics, see the [Tools](/tools/) page (JMeter, k6, Postman).
