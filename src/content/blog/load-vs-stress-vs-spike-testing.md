---
title: Load vs stress vs spike testing
description: When to use each type and how they differ.
pubDate: 2025-02-23
tags: [getting-started, types]
---

## Quick comparison

| Type | Goal | Load level | What you learn |
|------|------|------------|-----------------|
| **Load** | Validate under expected traffic | At or near target | Meets SLAs? Stable? |
| **Stress** | Find breaking point | Push beyond target | Capacity limit, recovery |
| **Spike** | Sudden traffic surge | Fast ramp to high load | Handles flash sales, viral spikes? |

---

## Load testing

**Goal:** Confirm the system meets performance targets under **normal or expected** load.

- Run at **target concurrency** (e.g. 100–500 users) and **target duration** (e.g. 30 min).
- Measure: throughput (RPS), latency (p50, p95, p99), error rate.
- **Pass criteria:** All within SLA (e.g. p95 &lt; 300 ms, error rate &lt; 0.1%).

**When to use:** Before release, after major changes, or regularly (e.g. weekly) as regression.

---

## Stress testing

**Goal:** Find **how much load** the system can handle before it degrades or fails, and how it **recovers**.

- **Ramp up** load (e.g. 50 → 100 → 200 → 500 users) until response time or errors spike.
- Or hold high load until something breaks (timeout, OOM, crash).
- Observe: at what point do latency and errors worsen? Does the system recover when load drops?

**When to use:** Capacity planning, finding bottlenecks, validating auto-scaling or failover.

---

## Spike testing

**Goal:** See how the system behaves when traffic **suddenly jumps** (e.g. flash sale, viral link).

- **Short ramp-up** (e.g. 0 to 500 users in 30 seconds) or two phases: low load, then sudden high load.
- Measure: does the system accept the spike, throttle, or crash? How long to stabilize?

**When to use:** When you expect sudden traffic (campaigns, product launches, events).

---

## Endurance (soak) testing

**Goal:** Run at **moderate load for hours** to find memory leaks, connection leaks, or slow degradation.

- Steady load (e.g. 50 users for 4–8 hours).
- Watch: memory growth, response time trend, error rate over time.

**When to use:** Before long-running or always-on systems; after fixing leaks.

---

## How to run them

- Use the same tools ([JMeter](/tools/), [k6](/tools/)) and the same [metrics](/blog/performance-metrics-explained/).
- **Load:** Set VUs/threads and duration to your target; add [thresholds](https://k6.io/docs/using-k6/thresholds/) or assertions.
- **Stress:** Use **stages** or stepping to ramp up; watch when metrics cross acceptable limits.
- **Spike:** Short ramp in stages (e.g. 10s at 10 VUs, then 30s at 500 VUs).
- **Endurance:** Long duration at fixed VUs; monitor server memory and GC.

Start with **load** to establish a baseline, then add **stress** and **spike** when you need capacity and resilience data.
