---
title: Performance testing in the cloud (AWS, Azure, GCP)
description: How to run load tests in cloud environments, use cloud-native tools, and test auto-scaling.
pubDate: 2025-02-24
tags: [cloud, advanced, process]
---

## Why cloud changes performance testing

Cloud infrastructure can scale up and down automatically — but auto-scaling has limits, cold starts, and costs. Performance testing in the cloud means testing your **app under load** and testing whether your **cloud setup scales correctly**.

---

## Key cloud performance topics

### 1. Auto-scaling
- Does your Auto Scaling Group (AWS), VMSS (Azure), or MIG (GCP) scale in time?
- What is the cold start time when new instances spin up?
- Is there a latency spike during scale-out?

**How to test:** Ramp VUs gradually and watch if new instances appear before latency crosses SLA.

### 2. Load balancer behavior
- Are requests distributed evenly across instances?
- Does the load balancer drop connections during scale events?
- What is the health check interval? Unhealthy instances may still receive traffic briefly.

### 3. Cloud-native databases
- **RDS / Aurora** — connection pool per instance; test with [PgBouncer](https://www.pgbouncer.org/) or RDS Proxy under high concurrency
- **DynamoDB** — throughput capacity units (RCU/WCU); test for throttling
- **Cosmos DB / Firestore** — request units and partition key distribution

### 4. CDN and edge
- Test origin load vs CDN cache hit ratio
- Measure TTFB at origin vs edge
- Spike test to see how CDN handles sudden traffic

### 5. Serverless (Lambda, Azure Functions)
- Cold starts add 100 ms–5 s to first request
- Concurrent invocation limits (default 1000 for Lambda)
- Cost at load: test with realistic traffic, not just "does it work?"

---

## Cloud-native performance testing tools

| Tool | Cloud | Notes |
|------|-------|-------|
| **AWS Load Testing** | AWS | Distributed JMeter on ECS (Fargate) |
| **Azure Load Testing** | Azure | Managed k6 and JMeter in Azure |
| **k6 Cloud** | Any | Run k6 from distributed cloud agents |
| **Gatling Enterprise** | Any | Managed load testing SaaS |
| **Artillery Cloud** | Any | Node-based, free tier available |

You can also run [JMeter](/tools/) or [k6](/tools/) from cloud VMs in multiple regions.

---

## Step-by-step: cloud load test

1. **Set up your environment** — staging or a dedicated perf environment in the cloud
2. **Use cloud-based load generators** — run JMeter/k6 from VMs in the same region to avoid extra network latency
3. **Enable CloudWatch / Azure Monitor / Cloud Monitoring** — watch CPU, memory, connections, scaling events
4. **Ramp load slowly** — let auto-scaling respond; note the latency spike during scale-out
5. **Test scale-in too** — drop load and check that instances terminate cleanly without dropping requests

---

## Common cloud performance mistakes

- **Testing from your laptop** — network latency to the cloud distorts results
- **Ignoring cold starts** — first request to a Lambda or new instance is much slower
- **Not testing scale-in** — terminating instances can drop in-flight requests
- **Over-provisioning to hide bottlenecks** — scale up doesn't fix a slow DB query
- **Not checking costs** — a poorly optimized app at scale costs more

---

## Related

- [Load vs stress vs spike testing](/blog/load-vs-stress-vs-spike-testing/) — which test to run and when
- [Performance testing in CI/CD](/blog/performance-testing-cicd/) — automate cloud load tests in pipelines
- [Setting performance SLAs](/blog/setting-performance-slas/) — define pass/fail for cloud tests
