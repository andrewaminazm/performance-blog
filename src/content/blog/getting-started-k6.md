---
title: Getting started with k6
description: Install k6 and run your first load test with JavaScript.
pubDate: 2025-02-22
tags: [k6, tools]
draft: true
---

## What is k6?

k6 by Grafana Labs is a script-based load testing tool. You write tests in **JavaScript**; k6 runs them with many virtual users. Great for APIs and modern workflows (CI/CD, code review).

## Install

- **Windows**: `winget install k6` or download from [k6.io](https://k6.io/).
- **Mac**: `brew install k6`.
- **Linux**: see [k6.io/docs](https://k6.io/docs/).

## First script

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  vus: 10,
  duration: '30s',
};

export default function () {
  const res = http.get('https://httpbin.org/get');
  check(res, { 'status is 200': (r) => r.status === 200 });
  sleep(1);
}
```

Run: `k6 run script.js`.

## Key concepts

- **VUs** (virtual users): concurrency.
- **Stages**: ramp-up, steady load, ramp-down (in `options.scenarios` or `options.stages`).
- **Checks**: pass/fail per request (don’t stop the test).
- **Thresholds**: pass/fail for the whole run (e.g. `p95 < 500`, `error_rate < 0.01`).

## Outputs

- CLI summary (default).
- JSON/CSV for analysis.
- InfluxDB, Grafana Cloud, or other outputs for dashboards.

Expand with custom metrics, thresholds, and CI integration in later posts.
