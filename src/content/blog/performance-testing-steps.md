---
title: Performance testing in 9 steps
description: End-to-end process from objectives to reporting.
pubDate: 2025-02-22
tags: [getting-started, process]
---

## The full process

### 1. Define objectives

- Target **throughput** (e.g. 500 requests/second).
- **Latency** targets (e.g. p95 &lt; 200 ms).
- **Error rate** (e.g. &lt; 0.1%).
- **Concurrency** (e.g. 1000 simultaneous users).

### 2. Choose environment

- Use **staging** or a production-like environment.
- Same (or scaled) DB, caches, and network.
- Isolate tests so they don’t affect real users.

### 3. Identify scenarios

- Critical **user flows** (login, search, checkout).
- **API endpoints** that must perform.
- **Traffic mix** (e.g. 70% read, 30% write).

### 4. Design workload

- **Virtual users** (VUs) or threads.
- **Ramp-up**: how fast to reach target load.
- **Duration**: how long to hold load.
- **Think time** between actions (optional).

### 5. Implement tests

- Script in **JMeter**, **k6**, or **Postman** (Newman).
- Use variables, data files, and assertions.
- Keep scripts maintainable and versioned.

### 6. Execute

- Run **baseline** at low load.
- Run **load** test at target load.
- Optionally run **stress** (increase until failure).

### 7. Monitor

- **Application** metrics (response time, errors).
- **Infrastructure**: CPU, memory, disk, network.
- **Databases**: connections, slow queries.

### 8. Analyze

- Compare to objectives (throughput, latency, errors).
- Find **bottlenecks** (which component degrades first).
- Correlate with infrastructure and code.

### 9. Report

- Summary, graphs, and recommendations.
- **Retest** after fixes and track trends.

Next: [Performance metrics explained](/blog/performance-metrics-explained).
