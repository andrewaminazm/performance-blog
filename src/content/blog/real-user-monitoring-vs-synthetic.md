---
title: Real user monitoring vs synthetic testing
description: When to use RUM and when to use synthetic load tests — and how to use both together.
pubDate: 2025-02-24
tags: [metrics, monitoring, advanced]
---

## The difference

| | **Synthetic testing** | **Real user monitoring (RUM)** |
|--|----------------------|-------------------------------|
| **What** | Simulated users (scripts) | Real users in production |
| **When** | Before release (CI/CD, staging) | After release, always-on |
| **Data** | Controlled, reproducible | Actual experience, all devices |
| **Coverage** | Any scenario you script | Only what users actually do |
| **Tools** | JMeter, k6, Postman | Datadog RUM, New Relic, Dynatrace, SpeedCurve |

Both are necessary. Neither replaces the other.

---

## Synthetic testing

You control everything: the script, the load, the timing. This is what [JMeter](/tools/), [k6](/tools/), and [Postman](/tools/) do.

**Use synthetic for:**
- Load and stress testing before release
- Regression testing (did this change break performance?)
- CI/CD gates (block deploy if p95 > 300 ms)
- Testing edge cases and failure scenarios
- Capacity planning (how many VUs can we handle?)

**Limitations:**
- Doesn't capture real device diversity (old phones, slow networks)
- Doesn't capture real user behavior (what pages they actually visit)
- You have to write and maintain scripts

---

## Real user monitoring (RUM)

RUM collects performance data from actual user browsers and apps. A small JavaScript snippet (or SDK) measures page load, Core Web Vitals, API calls, and errors for every real session.

**Use RUM for:**
- Understanding real user experience after release
- Segmenting by geography, device, browser, connection type
- Catching issues that synthetic tests don't cover (real user flows, real networks)
- Tracking Core Web Vitals (LCP, INP, CLS) over time

**Limitations:**
- No data before release
- Can't simulate high load
- Privacy considerations (user data)

---

## How to use both together

1. **Before release** — run synthetic [load tests](/blog/load-vs-stress-vs-spike-testing/) in CI/CD; block if [SLAs](/blog/setting-performance-slas/) are missed.
2. **After release** — RUM shows if real users are experiencing what synthetic tests predicted.
3. **If RUM shows a problem** — reproduce it with a synthetic test to fix and verify the fix.
4. **Use RUM baselines** — set synthetic test thresholds based on RUM p95 data from production.

---

## Popular RUM tools

| Tool | Type | Notes |
|------|------|-------|
| **Datadog RUM** | Full-stack APM + RUM | Session replay, Core Web Vitals |
| **New Relic Browser** | APM + RUM | Page load, AJAX, JS errors |
| **Dynatrace** | AI-powered APM | Automatic baselining |
| **SpeedCurve** | Web perf focus | Core Web Vitals, filmstrips |
| **Google CrUX** | Free, anonymous | Real Chrome user data for your URL |
| **Sentry** | Error + performance | Good for frontend JS errors |

---

## Related

- [Performance metrics explained](/blog/performance-metrics-explained/) — what to measure
- [Performance testing for web](/blog/performance-testing-web/) — LCP, INP, CLS
- [How to analyze performance test results](/blog/analyzing-performance-results/) — interpreting data
