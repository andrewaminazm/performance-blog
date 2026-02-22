---
title: Performance testing in CI/CD
description: Run load and performance checks in your pipeline.
pubDate: 2025-02-23
tags: [cicd, best-practices]
---

## Why run performance tests in CI/CD?

- **Catch regressions** before production (e.g. a slow query or new dependency).
- **Enforce SLAs** with thresholds: fail the build if p95 or error rate is out of range.
- **Consistent runs** on every commit or nightly, not only when someone remembers.

---

## What to run in the pipeline

- **Smoke / sanity:** Short run (e.g. 1–2 min, 10–20 VUs) to confirm the app responds and basic [metrics](/blog/performance-metrics-explained/) are in range.
- **Thresholds:** e.g. p95 &lt; 500 ms, error rate &lt; 1%. If exceeded, the job fails.
- **Optional:** Heavier load or stress in a **scheduled** job (e.g. nightly) so the main pipeline stays fast.

---

## Tools that fit CI

### k6

- **Native:** Single binary, no JVM; easy in Docker or GitHub Actions.
- **Thresholds:** `k6 run --threshold "http_req_failed<0.01" script.js` — exit code non-zero on failure.
- **Example (GitHub Actions):** Install k6, run script, fail job on exit code.

### JMeter

- **CLI:** `jmeter -n -t plan.jmx -l results.jtl`; then check results or use a plugin to fail on criteria.
- **Needs Java** in the runner; heavier than k6 but works in CI.

### Postman / Newman

- **Light checks:** `newman run collection.json -n 20` for API sanity; add a test that fails if response time &gt; 5 s.
- **Not** for real load; use for functional + simple perf gate.

---

## Pipeline design tips

1. **Keep main pipeline fast:** Short duration (e.g. 1–3 min), low VUs; save long stress tests for nightly.
2. **Use staging or a dedicated env:** Don’t load test production from CI unless you have a separate, safe strategy.
3. **Store artifacts:** Save HTML report (JMeter) or JSON output (k6) so you can inspect failures.
4. **Baseline comparison (optional):** Compare current run to a baseline and fail if metrics regress by X%.
5. **Flaky tests:** If the environment is unstable, use a few retries or relax thresholds so CI doesn’t cry wolf.

---

## Example: k6 in GitHub Actions

```yaml
- name: Run k6
  run: |
    k6 run --out json=results.json script.js
  continue-on-error: true
- name: Check thresholds
  run: |
    if [ $? -ne 0 ]; then exit 1; fi
```

Or use the **k6 GitHub Action** (e.g. `grafana/k6-action`) to run the script and fail on threshold failure.

Start with a **short k6 (or JMeter) run** and **one or two thresholds** in CI; then extend to nightly or longer tests as needed.
