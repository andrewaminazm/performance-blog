---
title: How to set performance SLAs and goals
description: Define measurable targets for latency, throughput, and availability.
pubDate: 2025-02-23
tags: [metrics, process]
---

## What is a performance SLA?

A **Service Level Agreement** (or internal goal) defines **numeric targets** the system must meet. Common ones:

- **Latency:** e.g. p95 response time &lt; 200 ms.
- **Throughput:** e.g. support 1000 requests/second.
- **Availability / success rate:** e.g. 99.9% of requests succeed.
- **Error rate:** e.g. &lt; 0.1% failed requests.

Without clear targets, you can’t say if a test “passed” or if a release is safe.

---

## Step 1: Choose the right metrics

- **User-facing:** Response time (p50, p95, p99), time to first byte (TTFB), Core Web Vitals for web.
- **System:** Throughput (RPS/TPS), error rate, success rate.
- **Infrastructure:** CPU, memory, DB connections (to avoid saturation).

Pick a **small set** (e.g. p95 latency + error rate + RPS) so everyone can focus.

---

## Step 2: Set realistic targets

- Use **current production** or **staging** data: what is p95 today? That’s your baseline.
- Add headroom: e.g. “p95 &lt; 200 ms” when baseline is 120 ms, so you have room for growth.
- Align with **business:** support expected peak traffic (e.g. Black Friday) and a safety margin.

**Examples:**

- API: p95 &lt; 300 ms, error rate &lt; 0.1%, 500 RPS per instance.
- Web page: LCP &lt; 2.5 s, INP &lt; 200 ms, CLS &lt; 0.1.
- Mobile app: cold start &lt; 3 s, 60 FPS on key screens.

---

## Step 3: Define pass/fail for tests

- In [k6](/tools/): use **thresholds** (e.g. `http_req_duration: ['p(95)<300']`, `http_req_failed: ['rate<0.01']`).
- In [JMeter](/tools/): use **assertions** (e.g. Duration Assertion &lt; 500 ms) and review Summary Report.
- In CI: fail the build or block deploy if thresholds are not met.

---

## Step 4: Review and update

- SLAs should be **revisited** when traffic patterns or architecture change.
- Use [load and stress tests](/blog/load-vs-stress-vs-spike-testing/) to see how close you are to limits.
- Document targets in a single place (wiki, SLA doc) so dev and ops share the same goals.

Start with **one or two** metrics (e.g. p95 + error rate), set targets from baseline data, and run [performance tests](/blog/performance-testing-steps/) regularly to stay within SLA.
