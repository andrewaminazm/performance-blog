---
title: Performance testing for web
description: Topics, metrics, and tools for web and front-end performance testing.
pubDate: 2025-02-22
tags: [web, topics]
---

## Why web performance matters

Slow pages hurt conversions, SEO, and user trust. Web performance testing covers **front-end** (page load, Core Web Vitals, assets) and **back-end** (APIs, server load). Both matter for the full user experience.

---

## Topics for web performance testing

### 1. Page load and Core Web Vitals

- **LCP** (Largest Contentful Paint): when the main content is visible; target &lt; 2.5 s.
- **INP / FID** (Interaction to Next Paint): responsiveness to clicks/taps; target &lt; 200 ms.
- **CLS** (Cumulative Layout Shift): visual stability; target &lt; 0.1.
- **Tools**: Lighthouse (Chrome DevTools or CI), PageSpeed Insights, WebPageTest, real-user monitoring (RUM).

### 2. Front-end load testing

- **Simulating many users** opening the same page: use k6 or JMeter to hit the full page URL and measure response time and throughput.
- **Static assets**: CDN and caching; test cache hit ratio and TTFB for assets.
- **SPAs**: first load vs route change; API calls triggered by the page are load-tested separately (see APIs below).

### 3. API and backend load (web apps)

- **REST/GraphQL APIs** that feed the page: load test with [JMeter](/tools/), [k6](/tools/), or [Postman](/tools/) (light checks).
- **Throughput (RPS)** and **latency (p95, p99)** for login, search, checkout, etc.
- Same [metrics](/blog/performance-metrics-explained/) and [steps](/blog/performance-testing-steps/) as for any API.

### 4. Server and infrastructure

- **CPU, memory, I/O** on web and API servers under load.
- **Database** connection pool, slow queries, and replication lag.
- **CDN**: origin load, cache behavior, and edge latency.
- **Tools**: server metrics (Prometheus, Grafana, cloud dashboards); APM (e.g. Datadog, New Relic).

### 5. Stress and scalability (web)

- **Ramp-up** virtual users until the site or API degrades (response time, errors).
- **Spike test**: sudden traffic (e.g. flash sale) to see if the stack holds.
- **Endurance**: run at steady load for hours to catch memory leaks and slow degradation.

### 6. Mobile web

- **Same Core Web Vitals** and metrics, but measured on **mobile** (throttled CPU/network or real devices).
- Lighthouse in **mobile mode**; WebPageTest with mobile device and 4G.
- See [Performance testing for mobile](/blog/performance-testing-mobile/) for more.

### 7. Synthetic vs real-user monitoring

- **Synthetic**: scripted runs (e.g. k6, Lighthouse in CI) from fixed locations; consistent, repeatable.
- **RUM** (Real User Monitoring): metrics from real browsers; shows actual user experience by device and region.
- Use both: synthetic for SLAs and regression; RUM for reality check.

---

## Key metrics (web)

| Metric | What it tells you |
|--------|-------------------|
| LCP | When main content is visible |
| INP / FID | How fast the page responds to input |
| CLS | Layout stability (no unexpected jumps) |
| TTFB | Time to first byte (server + network) |
| RPS / throughput | How many requests the server handles |
| Error rate | Stability under load |
| p95 / p99 latency | Tail latency for APIs and pages |

---

## Tools quick reference

- **Core Web Vitals / page load**: Lighthouse, PageSpeed Insights, WebPageTest.
- **API and server load**: [JMeter](/tools/), [k6](/tools/), [Postman](/tools/) (see Tools page).
- **RUM**: Google Analytics 4, CrUX, or APM with RUM (e.g. Datadog, New Relic).
- **CI**: Lighthouse CI, k6 in pipelines, or custom scripts.

Start with **Core Web Vitals** and **TTFB** for the main pages, then add **API load tests** with k6 or JMeter for the endpoints that power the app. Use the same [performance metrics](/blog/performance-metrics-explained/) and [9-step process](/blog/performance-testing-steps/) for a consistent approach.
