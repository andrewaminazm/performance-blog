---
title: Getting started with Apache JMeter
description: Install JMeter and run your first load test.
pubDate: 2025-02-22
tags: [jmeter, tools]
draft: true
---

## What is JMeter?

Apache JMeter is an open-source load and performance testing tool. You build test plans with a GUI (or XML) using **Thread Groups**, **Samplers**, and **Listeners**. Good for HTTP, APIs, DB, and more.

## Install

1. Install Java (JDK 11+).
2. Download JMeter from [jmeter.apache.org](https://jmeter.apache.org/).
3. Unzip and run `bin/jmeter` (or `jmeter.bat` on Windows).

## First test

1. Add a **Thread Group**: right-click Test Plan → Add → Threads → Thread Group. Set number of threads (users) and ramp-up.
2. Add an **HTTP Request**: right-click Thread Group → Add → Sampler → HTTP Request. Set server name and path.
3. Add a **Listener**: e.g. View Results Tree or Summary Report. Run the test (green play button).

## Key concepts

- **Thread Group**: virtual users and how they ramp up.
- **Samplers**: the actual requests (HTTP, JDBC, etc.).
- **Listeners**: reports and graphs (use sparingly when running large tests; they use memory).
- **Assertions**: validate response (status code, body).
- **Controllers**: loops, conditions, transaction controllers.

## Running without GUI (recommended for real load)

```bash
jmeter -n -t myplan.jmx -l results.jtl
```

Use **Backend Listener** (e.g. InfluxDB + Grafana) for live dashboards during long runs.

Expand this post with parameterization (CSV), assertions, and reporting in later articles.
