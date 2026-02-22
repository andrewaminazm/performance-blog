---
title: Performance testing for mobile
description: Topics, metrics, and tools for mobile app and mobile web performance testing.
pubDate: 2025-02-22
tags: [mobile, topics]
---

## Why mobile performance matters

Mobile users expect fast, smooth apps. Slow startup, janky scrolling, or high battery drain lead to uninstalls and poor reviews. Performance testing for mobile covers **native apps** (iOS/Android), **mobile web** (browsers on phones), and **hybrid** apps.

---

## Topics for mobile performance testing

### 1. App launch and startup time

- **Cold start**: time from tap to first interactive frame (app was not in memory).
- **Warm start**: app was in background; measure time to resume.
- **Tools**: Xcode Instruments (iOS), Android Studio Profiler, Firebase Performance Monitoring, custom timers in code.

### 2. Frame rate and smoothness (jank)

- **FPS** (frames per second): target 60 FPS (or 120 on capable devices) for smooth UI.
- **Dropped frames / jank**: frames that miss the vsync window; users see stutter.
- **Tools**: GPU profiling in Android Studio, Xcode Instruments (Time Profiler, Core Animation), Perfetto, GameBench.

### 3. Memory and battery

- **Memory usage**: heap size, leaks (growth over time), OOM (out-of-memory) crashes.
- **Battery drain**: CPU wake locks, network usage, background activity.
- **Tools**: Memory profilers in IDE, LeakCanary (Android), Xcode Memory Graph; battery stats and Battery Historian.

### 4. Network and API on mobile

- **Latency over cellular**: 3G/4G/5G and variable networks; test with throttling.
- **Offline and weak network**: how the app behaves with slow or no connectivity.
- **Tools**: k6 or JMeter for API load; Charles Proxy, Proxyman, or browser DevTools for throttling; Network Link Conditioner (iOS).

### 5. Mobile web performance

- **Page load on mobile**: LCP, FID/INP, CLS (Core Web Vitals) on real or emulated devices.
- **Lighthouse (mobile)**: run in Chrome DevTools or CI with mobile emulation.
- **Real devices vs emulators**: always validate on real devices for accurate battery and CPU.

### 6. Load testing mobile backends

- The **backend** and **APIs** that the app calls are load-tested with JMeter, k6, or similar (same as web).
- Simulate many concurrent mobile clients (e.g. thousands of virtual users hitting the same API).

### 7. Installation and update size

- **Download size** (APK/IPA or app store): affects install time and abandonment.
- **Update size**: delta updates and asset delivery (e.g. Play Asset Delivery, App Clip).

---

## Key metrics (mobile)

| Metric | What it tells you |
|--------|-------------------|
| Cold start time | First impression; keep under 2–3 s where possible |
| FPS / jank | Smoothness of scrolling and animations |
| Memory (heap) | Risk of OOM and background kills |
| API latency (p95) | Responsiveness of data in the app |
| Battery impact | User experience over a session or day |
| Crash rate | Stability under load or long runs |

---

## Tools quick reference

- **Native profiling**: Xcode Instruments, Android Studio Profiler, Perfetto.
- **APIs / backend load**: JMeter, k6, Postman (see [Tools](/tools/)).
- **Mobile web**: Lighthouse (mobile mode), Chrome DevTools, WebPageTest (mobile).
- **Monitoring in production**: Firebase Performance, New Relic, Datadog, custom APM.

Start with **startup time** and **frame rate** on a few key flows, then add **network** and **memory** tests. Use the same [performance metrics](/blog/performance-metrics-explained/) and [step-by-step process](/blog/performance-testing-steps/) as for web and APIs.
