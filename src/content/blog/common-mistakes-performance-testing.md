---
title: Common mistakes in performance testing
description: What to avoid and how to get reliable results.
pubDate: 2025-02-23
tags: [best-practices, process]
---

## 1. Testing in the wrong environment

- **Problem:** Running load tests against a tiny staging box or localhost, then expecting production to behave the same.
- **Fix:** Use **staging** that mirrors production (same stack, scaled down if needed), or a dedicated perf environment. Document differences (e.g. “DB has 1/10 of prod data”).

---

## 2. No clear goals or SLAs

- **Problem:** “We ran 100 users for 10 minutes” with no pass/fail criteria. You can’t say if it’s good or bad.
- **Fix:** Define [SLAs](/blog/setting-performance-slas/) (e.g. p95 &lt; 300 ms, error rate &lt; 0.1%) and use [thresholds](/tools/) (k6) or assertions (JMeter) to fail the run when they’re not met.

---

## 3. Running heavy load from the GUI

- **Problem:** Running JMeter with 500 threads and View Results Tree open; the load generator runs out of memory or CPU and distorts results.
- **Fix:** Use **CLI** for real load ([JMeter](/tools/) `-n`, [k6](/tools/) always CLI). Use the GUI only for designing and debugging with few threads.

---

## 4. Ignoring the load generator

- **Problem:** Assuming all bottlenecks are on the server. If the **load generator** (the machine running JMeter/k6) is at 100% CPU or network, it can’t send more load and results are misleading.
- **Fix:** Monitor **CPU and memory on the load generator**. If they’re high, use a more powerful machine or distribute load (e.g. JMeter distributed, or multiple k6 runners).

---

## 5. No think time or pacing

- **Problem:** Scripts that fire requests as fast as possible. Unrealistic and can max out the load generator or the server in an artificial way.
- **Fix:** Add **think time** (e.g. [JMeter](/tools/) Constant Timer, [k6](/tools/) `sleep()`) or pacing so each user has a realistic delay between actions.

---

## 6. One run is enough

- **Problem:** A single run can be lucky or unlucky (e.g. GC pause, network blip). Basing decisions on one run is risky.
- **Fix:** Run **at least 2–3 times** for important tests; compare results. Use **trends** (e.g. weekly runs) to spot regressions.

---

## 7. Only testing the happy path

- **Problem:** Load testing only the main API or page; ignoring login, search, or error paths that might be slow or fragile.
- **Fix:** Include **critical user flows** ([steps](/blog/performance-testing-steps/) 3–4): login, main transaction, search, and any heavy or error paths.

---

## 8. Not correlating with app and infra

- **Problem:** Seeing “p95 is 2 s” but not checking **why** (DB? cache? GC?).
- **Fix:** **Correlate** test results with server metrics (CPU, memory, DB, logs). Use APM or Grafana to see what was slow during the test.

---

## Quick checklist

- [ ] Environment matches (or approximates) production.
- [ ] Clear SLAs and pass/fail (thresholds/assertions).
- [ ] Load run from CLI, not GUI; load generator has headroom.
- [ ] Think time or pacing in the script.
- [ ] Multiple runs for important decisions.
- [ ] Critical flows and key endpoints covered.
- [ ] Server and infra metrics reviewed when results are bad.

Avoiding these mistakes makes your [performance tests](/blog/performance-testing-steps/) and [analysis](/blog/analyzing-performance-results/) much more reliable.
