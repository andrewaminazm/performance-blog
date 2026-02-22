---
title: What is performance testing?
description: Introduction to performance testing, types, and why it matters.
pubDate: 2025-02-22
tags: [getting-started, basics]
---

## What is performance testing?

Performance testing checks how your system behaves under load: speed, stability, and scalability. You simulate many users or requests and measure response times, throughput, and errors.

## Why it matters

- **Find bottlenecks** before users do (slow DB, memory leaks, weak APIs).
- **Validate SLAs** (e.g. “p95 &lt; 200 ms”).
- **Plan capacity** for traffic spikes and growth.
- **Avoid outages** by testing limits in a controlled way.

## Types of performance testing

| Type | Goal |
|------|------|
| **Load testing** | Normal or expected load; check if system meets targets. |
| **Stress testing** | Beyond normal load; find breaking point and recovery. |
| **Spike testing** | Sudden traffic increase; see how system reacts. |
| **Endurance / soak** | Sustained load over hours; find leaks and degradation. |
| **Scalability** | Increase load step by step; see how capacity scales. |

## How to start

1. Define **objectives** (e.g. 1000 RPS, p95 &lt; 300 ms).
2. Choose a **tool** (JMeter, k6, or Postman for lighter tests).
3. **Script** your main user flows or API calls.
4. **Run** tests in a staging or production-like environment.
5. **Analyze** metrics and fix bottlenecks, then retest.

Next: [Performance testing in 9 steps](/blog/performance-testing-steps).
