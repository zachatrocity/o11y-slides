---
theme: the-unnamed
highlighter: shiki
info: |
  ## Observability, Monitoring and Alerting
  A practical guide to work for the weekend instead of on the weekend.
transition: slide-left
title: Observability, Monitoring and Alerting
layout: cover
background: https://images.unsplash.com/photo-1741986947217-d1a0ecc39149?q=80&w=1466&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
---

# Observability, Monitoring and Alerting
A practical guide to work for the weekend instead of on the weekend.

---
layout: about-me
helloMsg: Hello!
name: Zach Russell
imageSrc: https://github.com/zachatrocity/o11y-slides/blob/3d3f7e786674d16ff131e8aab9899d8e83a2163d/images/PXL_20231029_010441145.MP.jpg?raw=true
position: left
job: Principal Engineer
line1: Disney Streaming - Web Commerce
line2: Sign up, conversion, account management
social1: "email: zach@zachs.io"
social2: "@zach:matrix.zachs.io"
---

---

# What is Observability?

<div grid="~ cols-2 gap-4">
<div>

## Definition
The ability to understand a system's internal state by examining its outputs

## Key Components
- **Metrics**: Quantitative measurements over time
- **Logs**: Timestamped records of events
- **Traces**: Records of request paths across services

</div>
<div>
```mermaid
---
config:
  look: handDrawn
  layout: elk
---
graph TD
  WebApp[Web Application] -->|HTTP Request| BackendAPI[Backend API]
  BackendAPI -->|Metrics, Logs, Traces| Datadog[Datadog]
  WebApp -->|Metrics, Logs, Traces| Datadog
```
</div>
</div>

---

# What is Monitoring?

<div grid="~ cols-2 gap-4">
<div>

## Definition
The continuous collection and analysis of system data

## Purpose
- Verify system health
- Detect anomalies
- Track performance
- Ensure SLAs are met

</div>
<div>
```mermaid
---
config:
  look: handDrawn
  layout: elk
---
graph TD
  System[Observability] -->|Metrics, Logs| Datadog[Monitoring]
  Datadog -->|Dashboards| Users[Users]
  Datadog -->|Alerts| OnCall[On-Call Engineers]
```

</div>
</div>
<div class="text-sm italic mt-10">
🤔 Discussion point: Why can't we just check our systems manually once a day?
</div>


<!--
By the time a human notices, the damage is often already done

Problems can develop and impact users rapidly, resulting in lost revenue or reputation damage

Intermittent issues may not be evident during a manual check

Patterns and trends over time aren't visible with periodic manual checks
-->

---

# What is Alerting?

<div grid="~ cols-2 gap-4">
<div>

## Definition
The process of notifying humans about significant events requiring attention

## Requirements for Good Alerting
- Actionable
- Timely
- Relevant
- Clear
- Prioritized

</div>
<div>

<div v-click> 
  <div class="bg-yellow-100 p-2 color-black rounded mt-4">
  ⚠️ Alert: High CPU usage detected on development environment web-server-01. 95% utilization for >5 minutes.
  </div>
  
  <div class="text-sm italic mt-2">
  Activity: Would you wake someone up at 3am for this alert? Why or why not?
  </div>
</div>

<div v-click>
  <div class="bg-red-100 p-2 color-black rounded mt-4">
  ⚠️ Alert: 0 User sign ups detected on production environment web-server-01 for >1hr
  </div>
  
  <div class="text-sm italic mt-2">
  What about this one?
  </div>
</div>

</div>
</div>

<!-- 

It depends on context (is this a critical production system?)

Need more information to determine criticality (is this affecting users?)

High CPU could lead to outages, but could also be temporary

Better approach: Set escalating thresholds (e.g., alert at 85% as warning, wake someone at 95% for >15 minutes if impacting users)

Consider if there's an automated remediation that could be tried first

-->
---

# The Real Cost of Downtime

<div grid="~ cols-2 gap-4">
<div>

## Financial Impact
- Lost sales while customers cannot check out
- Support and recovery costs
- At Disney this is our main motivator
  - if users can't sign up, we don't make money

## Reputation Damage
- Customer trust
- Brand perception
- Social media amplification

</div>
<div>

<div class="bg-red-100 p-2 color-black rounded mt-4 text-sm">
<b>Case Study:</b> Facebook's six-hour outage in October 2021 showed how one infrastructure failure can affect many products at once.
</div>

<div v-click class="bg-red-100 p-2 color-black rounded mt-4 text-sm">
<b>Case Study:</b> In July 2024, a faulty CrowdStrike update disrupted Windows systems worldwide. A software update became a business-continuity problem.
</div>

<div v-click class="bg-red-100 p-2 color-black rounded mt-4 text-sm">
<b>Case Study:</b> During the <a href="https://aws.amazon.com/message/101925/">October 2025 AWS outage</a>, services could not find a key database. Even after that was fixed, other services needed hours to recover. Fixing the cause did not immediately fix the customer experience.
</div>

</div>

<div class="text-xs"><a href="https://engineering.fb.com/2021/10/05/networking-traffic/outage-details/">Meta outage report</a> · <a href="https://www.crowdstrike.com/falcon-content-update-remediation-and-guidance-hub/">CrowdStrike incident hub</a> · <a href="https://aws.amazon.com/message/101925/">AWS outage report</a></div>
</div>

---

# Observability vs. Monitoring: A Key Distinction

<div grid="~ cols-2 gap-4">
<div>

## Monitoring
- **Known unknowns**
- Asks: "Is the system working?"
- Predefined checks and thresholds
- Limited to what you thought to monitor

</div>
<div>

## Observability
- **Unknown unknowns**
- Asks: "Why is the system not working?"
- Flexible exploration of state
- Helps with novel failure modes

</div>
</div>

<div class="bg-blue-100 color-black p-4 rounded mt-4">
<b>Interactive scenario:</b> Your web application is slow but CPU, memory, and database metrics look normal. How would monitoring vs. observability tools help you diagnose this?
</div>

<!-- 
Monitoring would:

Tell you the application is slow but working
Show that traditional metrics look normal

Observability would help by:

Providing request tracing to identify slow components
Enabling detailed query analysis in the database
Allowing correlation between different services
Giving access to detailed logs for specific transactions
Supporting exploratory analysis to find non-obvious connections
-->

---

# The Developer Feedback Loop

<div class="mb-5">
Proper observability, monitoring and alerting significantly shortens the path from:
</div>

```mermaid
---
config:
  look: handDrawn
  layout: elk
---
graph LR
    A[Code Change] --> B[Deployment]
    B --> C[Issue Occurs]
    C --> D[Issue Detection]
    D --> E[Root Cause Analysis]
    E --> F[Implement Fix]
    F --> A
```

<div v-click class="bg-green-100 color-black p-3 rounded mt-4">
<b>Pop Quiz:</b> In which parts of this loop do our three topics (observability, monitoring, alerting) play critical roles?
</div>

<!--
Monitoring: Helps during "Issue Occurs" to "Issue Detection" by continuously checking system health

Alerting: Critical in "Issue Detection" phase by notifying the right people at the right time

Observability: Essential during "Root Cause Analysis" to understand why the issue happened

All three help shorten the entire loop by identifying issues faster and providing context for fixes
-->

---

# Observability: Key Metrics and Logs

<div grid="~ cols-2 gap-4">
<div>

## The Four Golden Signals
1. **Latency**: How long it takes to serve a request
2. **Traffic**: How much demand is on your system
3. **Errors**: Rate of failed requests
4. **Saturation**: How "full" your system is

## Effective Logging
- Structured logs (JSON)
- Consistent formatting
- Appropriate detail levels
- Correlation IDs

</div>
<div>

```json
{
  "timestamp": "2023-09-15T13:45:30.123Z",
  "level": "ERROR",
  "service": "payment-processor",
  "message": "Payment transaction failed",
  "user_id": "user_592",
  "error_code": "CARD_DECLINED",
  "latency_ms": 432
}
```

<div class="text-sm italic mt-2">
Workshop task: What information would you add to this log entry?
</div>

</div>
</div>

---

# Observability: Key Metrics and Logs

<div grid="~ cols-2 gap-4">
<div>

```json
{
  "timestamp": "2023-09-15T13:45:30.123Z",
  "level": "ERROR",
  "service": "payment-processor",
  "trace_id": "abc123xyz789",
  "session_id": "f09802flkj234",
  "geo": "LATAM",
  "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 15_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/15.0 Mobile/15E148 Safari/604.1",
  "message": "Payment transaction failed",
  "user_id": "user_592",
  "error_code": "CARD_DECLINED",
  "latency_ms": 432
}
```

</div>
</div>

---

# Observability: Distributed Tracing

<div grid="~ cols-2 gap-4">
<div>

## What is Distributed Tracing?
A method to track request flows across multiple services

## Core Concepts
- **Spans**: Individual operations within a distributed trace
- **Traces**: A collection of spans representing the lifecycle of a single request as it flows through multiple services
- **Propagation**: Passing context between services
- **Sampling**: Capturing subset of traces

## When You Need It
- Microservices architecture
- Complex request paths
- Performance bottlenecks

</div>
<div>

<div class="bg-purple-100 color-black p-2 rounded mt-2 text-sm">
<b>Real-world example:</b> A single user checkout request touches 15+ different microservices. How can you identify which one is causing a 2-second delay?
</div>

</div>
</div>

---

# Observability: Distributed Tracing

<div grid="~ cols-2 gap-4">
<div>

## Flamegraph
Enabled by spans and traces

![Flamegraph Example](https://imgix.datadoghq.com/img/knowledge-center/distributed-tracing/example-flamegraph-with-code-level-data.png?auto=format&fit=max&dpr=2)

</div>
</div>
---

# Observability: Tools and Techniques

<div class="grid color-black grid-cols-3 gap-4">
  <div class="bg-blue-50 p-3 rounded">
    <h3 class="text-blue-700">Collect and Explore</h3>
    <ul class="text-sm mt-2">
      <li>OpenTelemetry: instrumentation</li>
      <li>Datadog: metrics, logs, traces</li>
      <li>Prometheus + Grafana: metrics and dashboards</li>
    </ul>
  </div>
  <div class="bg-green-50 p-3 rounded">
    <h3 class="text-green-700">Investigate</h3>
    <ul class="text-sm mt-2">
      <li>Search logs and follow traces</li>
      <li>Compare before and after a change</li>
      <li>pup: query Datadog from the command line</li>
    </ul>
  </div>
  <div class="bg-purple-50 p-3 rounded">
    <h3 class="text-purple-700">Notify</h3>
    <ul class="text-sm mt-2">
      <li>Datadog Monitors / Grafana Alerting</li>
      <li>PagerDuty: on-call routing</li>
      <li>Slack / Teams: coordination</li>
    </ul>
  </div>
</div>

<!--
This is a short list of examples, not a shopping list. Combines the original
observability and alerting tool lists to save time.
-->

---

# pup: Datadog from Your Terminal

<div grid="~ cols-2 gap-4">
<div>

## Find recent errors

```bash
pup logs search \
  --query="service:checkout status:error" \
  --from="30m"
```

<div class="text-sm mt-3">Show checkout errors from the last 30 minutes.</div>

</div>
<div>

## Check CPU usage

```bash
pup metrics query \
  --query="avg:system.cpu.user{env:prod}" \
  --from="30m"
```

<div class="text-sm mt-3">Look at production CPU usage during the same period.</div>

</div>
</div>

<div class="text-sm mt-6">Useful for you—or an AI assistant helping you investigate. You still need to check what the data actually says.</div>

<div class="text-xs mt-5"><a href="https://docs.datadoghq.com/cli/">Datadog pup CLI</a> · Authenticate first with <code>pup auth login</code>. Replace the example service and environment with yours.</div>

<!--
Keep this to about a minute, not a live installation tutorial. These are read
queries. Examples checked against DataDog/pup documentation at commit
6c3a4707f5ab87688566020d868a50810db08166; no live Datadog account queries run.
https://github.com/DataDog/pup/blob/6c3a4707f5ab87688566020d868a50810db08166/docs/EXAMPLES.md
Use scoped credentials; read-only mode is also available. Empty results do not
prove health: the filters, permissions, or telemetry might be wrong.
-->

---

# Monitoring: Types and Approaches

<div class="grid grid-cols-3 gap-2">
  <div class="border p-3 rounded">
    <h3 class="text-center border-b pb-1">Infrastructure</h3>
    <ul class="text-sm mt-2">
      <li>CPU, Memory, Disk</li>
      <li>Network I/O</li>
      <li>Virtual machines</li>
      <li>Containers</li>
    </ul>
  </div>
  <div class="border p-3 rounded">
    <h3 class="text-center border-b pb-1">Application</h3>
    <ul class="text-sm mt-2">
      <li>Error rates</li>
      <li>Request rates</li>
      <li>Response times</li>
      <li>Success/failure</li>
    </ul>
  </div>
  <div class="border p-3 rounded">
    <h3 class="text-center border-b pb-1">Business</h3>
    <ul class="text-sm mt-2">
      <li>Conversions</li>
      <li>User activity</li>
      <li>Revenue</li>
      <li>Feature usage</li>
    </ul>
  </div>
</div>

<div class="mt-6 grid grid-cols-2 gap-4">
  <div>
    <h3 class="mb-2">White-box vs. Black-box</h3>
    <ul class="text-sm">
      <li><b>White-box:</b> Internal system metrics (with instrumentation)</li>
      <li><b>Black-box:</b> External behavior monitoring (as users see it)</li>
    </ul>
  </div>
  <div>
    <h3 class="mb-2">Push vs. Pull</h3>
    <ul class="text-sm">
      <li><b>Pull:</b> Monitoring system collects data from targets</li>
      <li><b>Push:</b> Systems send data to monitoring platform</li>
    </ul>
  </div>
</div>

---

# Monitoring: Best Practices

<div grid="~ cols-2 gap-2">
<div>

## The Goldilocks Principle
- Not too many metrics
- Not too few metrics
- Just right to understand system health

## USE Method (for resources)
- **Utilization**: Percentage of time busy
- **Saturation**: Extra work queued
- **Errors**: Error events

</div>
<div>

## Implementation Tips
- Define clear SLIs and SLOs
- Aggregate similar metrics
- Use anomaly detection
- Implement dashboards by use case:
  - Executive view
  - Operational view
  - Debugging view
- Test your monitoring
- Document alert meanings

</div>
</div>

<div class="text-xs mt-4">
<b>SLI (Service Level Indicator):</b> A metric that measures a specific aspect of a service<br>
<b>SLO (Service Level Objective):</b> A target value for an SLI
</div>


---

# Monitoring: Common Pitfalls

<div v-click.hide class="absolute">
  <img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExemN6Nmo2cWx1MXY1Zm8xcXhkY2Ric3R6MTdmZzl6d2tlZno4cHdpdSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/ASM4IvHzkop00e6sUN/giphy.gif" alt="Burning Money" class="h-60 mx-auto">
  <p class="text-sm mt-2 italic">The cost of over monitoring.</p>
</div>

<div v-click class="grid grid-cols-2 gap-4 mb-6">
  <div>
    <h3 class="text-red-600">Excessive Data Collection</h3>
    <p class="text-sm">Collecting everything "just in case" leads to:</p>
    <ul class="text-sm">
      <li>💰💰💰 storage/processing</li>
      <li>Query performance issues</li>
    </ul>
  </div>
  <div>
    <h3 class="text-red-600">Incorrect Thresholds</h3>
    <p class="text-sm">Poor threshold configuration causes:</p>
    <ul class="text-sm">
      <li>False positives</li>
      <li>Missed incidents</li>
      <li>Alert fatigue</li>
    </ul>
  </div>
</div>

<div v-click class="grid grid-cols-2 gap-4">
  <div>
    <h3 class="text-red-600">Focusing on Firefighting</h3>
    <p class="text-sm">Only reacting to alerts without:</p>
    <ul class="text-sm">
      <li>Trend analysis</li>
      <li>Capacity planning</li>
      <li>Preventative improvements</li>
    </ul>
  </div>
  <div>
    <h3 class="text-red-600">Siloed Monitoring</h3>
    <p class="text-sm">Separate tools without correlation:</p>
    <ul class="text-sm">
      <li>Infrastructure team can't see application metrics</li>
      <li>Developers can't see infrastructure issues</li>
      <li>No end-to-end view of user experience</li>
    </ul>
  </div>
</div>

---

# Alerting: Types of Alerts

<div class="grid grid-cols-3 gap-2">
  <div class="border p-3 rounded">
    <h3 class="text-red-600 text-center border-b pb-1">Critical/P0</h3>
    <div class="text-sm mt-2">
      <p><b>Characteristics:</b></p>
      <ul class="text-xs">
        <li>Service is down</li>
        <li>Data loss occurring</li>
        <li>Revenue impact</li>
      </ul>
      <p class="mt-2"><b>Response:</b></p>
      <p class="text-xs">Immediate action required, 24/7</p>
    </div>
  </div>
  <div class="border p-3 rounded">
    <h3 class="text-orange-500 text-center border-b pb-1">Warning/P1</h3>
    <div class="text-sm mt-2">
      <p><b>Characteristics:</b></p>
      <ul class="text-xs">
        <li>Degraded performance</li>
        <li>Approaching capacity</li>
        <li>Partial functionality loss</li>
      </ul>
      <p class="mt-2"><b>Response:</b></p>
      <p class="text-xs">Action required during business hours</p>
    </div>
  </div>
  <div class="border p-3 rounded">
    <h3 class="text-blue-500 text-center border-b pb-1">Info/P2</h3>
    <div class="text-sm mt-2">
      <p><b>Characteristics:</b></p>
      <ul class="text-xs">
        <li>Automated recovery succeeded</li>
        <li>Expected state changes</li>
        <li>Non-urgent maintenance needs</li>
      </ul>
      <p class="mt-2"><b>Response:</b></p>
      <p class="text-xs">No immediate action required</p>
    </div>
  </div>
</div>

<div class="mt-6">
  <h3>Alert Design Principles</h3>
  <div class="grid grid-cols-2 gap-4">
    <div>
      <ul class="text-sm">
        <li><b>Actionable:</b> Every alert should have a clear action to take</li>
        <li><b>Unique:</b> Don't send multiple alerts for the same issue</li>
        <li><b>Contextual:</b> Include sufficient information to diagnose</li>
      </ul>
    </div>
    <div>
      <ul class="text-sm">
        <li><b>Right Level:</b> Alert at the appropriate severity</li>
        <li><b>Accurate:</b> Minimize false positives/negatives</li>
        <li><b>Documented:</b> Include runbooks or troubleshooting steps</li>
      </ul>
    </div>
  </div>
</div>

---

# Case Study 1: The Slow Leak

<div class="bg-yellow-100 color-black p-3 rounded mt-2 mb-6 text-sm">
<b>Discussion:</b> A metric grows 2% a month for three years. Nothing is broken. When does it become an incident?
</div>

<div v-click grid="~ cols-2 gap-4">
<div>

## The Setup
- Consider 3 web apps all of its UI copy (every string, every language, every brand) into the main JavaScript file as a fallback
- Every feature adds strings. No process exists to remove them or audit their usage.
- The bundles grow for years. Each release, a little bigger. Each diff, too small to notice

</div>
<div>

## Hidden Cliffs
- Many CDNs compress files automatically... **up to 10 MB**.
- One PR unknowingly crossed this boundary. Compression silently switched off
- Overnight: **~2 MB on the wire → 11 MB on the wire**, on every page load, for every user
- No errors. No alerts. No failed deploy

</div>
<div>

## The Lesson
- **Baselines aren't enough.** "Is it normal?" alerts would never fire on a slow trend. Alert on *trend* and on *thresholds*
- **You don't own all your thresholds.** Find out where the cliffs are (CDN, browser, API quotas) and monitor the distance to them
- **Growth is a signal.** A metric that only ever goes up is a countdown to something

</div>
</div>

---
layout: center
---
<img src="https://github.com/user-attachments/assets/c47d3a5d-e270-4925-acc3-844548164dc9" />

---

# Case Study 2: The AI Is Responding, but Is It Working?

<div grid="~ cols-2 gap-4">
<div>

## What Happened
- In April 2026, Anthropic explained why some users were getting worse results from Claude Code
- Changes to settings, conversation history, and instructions affected answer quality
- The API itself was still working normally

</div>
<div>

## What We Can Learn
- A successful request doesn't always mean a useful answer
- Test realistic tasks, not just whether the service responds
- Pay attention to user feedback after changes

</div>
</div>

<div class="bg-blue-100 color-black p-3 rounded mt-4 text-sm">
<b>Discussion:</b> If a chatbot responds quickly but gives the wrong answer, would your current monitoring notice?
</div>

<div class="text-xs mt-4"><a href="https://www.anthropic.com/research/april-23-postmortem">Anthropic's April 23, 2026 incident report</a></div>

<!--
This replaces the unsourced holiday-rush success figures, rather than adding time.
Anthropic reported product-layer issues, not an API/inference outage. The lessons
are discussion points, not claims that monitoring can automatically judge quality.
For deeper reading, not an extra slide:
https://www.anthropic.com/research/demystifying-evals-for-ai-agents
https://opentelemetry.io/blog/2026/genai-observability/
-->

---

# Case Study 3: Canary Releases

<div class="bg-yellow-100 color-black p-3 rounded mt-2 mb-8 text-sm">
<b>Discussion:</b> Does anyone know what canary releases are and how observability can help us accomplish them?
</div>

<div v-click grid="~ cols-2 gap-4">

<div>

## The Challenge
- Upgrade from legacy Next.js 
- Upgrade Node.js from:
  - v18 -> v19 -> v20 -> v21 -> v22 (LTS)
- Validate and ensure no negative user impact
</div>

<div>

## Approach
1. Perform upgrades in feature branch
2. Deploy feature branch to staging environment
  a. Perform automated QA (E2E test suites)
  b. Monitor dashboards during test suites
3. Deploy feature branch as a canary to 25% of traffic
4. Monitor alerts and dashboards of canary traffic

</div>
</div>

---
layout: center
---
<img src="https://github.com/zachatrocity/o11y-slides/blob/main/images/image2.png?raw=true" />

---
layout: center
---
<img src="https://github.com/zachatrocity/o11y-slides/blob/main/images/image.png?raw=true" />

---

# Hands-On Exercise:

<div class="text-center mb-4">
You've received reports that the checkout system is slow. Here's what you can see:
</div>

<div class="grid grid-cols-3 gap-2">
  <div class="border p-1">
    <div class="text-center font-bold">Infrastructure Metrics</div>
    <div class="text-sm">
      ✅ CPU: 30% utilization<br/>
      ✅ Memory: 45% utilization<br/>
      ✅ Network: Normal traffic<br/>
      ✅ Disk: 60% capacity<br/>
    </div>
  </div>
  <div class="border p-1">
    <div class="text-center font-bold">Application Metrics</div>
    <div class="text-sm">
      ❌ Response time: 3.5s (up from 0.8s)<br/>
      ✅ Error rate: 0.1% (normal)<br/>
      ✅ Request rate: Normal<br/>
      ❓ Cache hit rate: 45% (down from 95%)<br/>
    </div>
  </div>
  <div class="border p-1">
    <div class="text-center font-bold">Database Metrics</div>
    <div class="text-sm">
      ❌ Query time: 2.9s (up from 0.3s)<br/>
      ❌ Connection count: 95/100<br/>
      ❓ Index usage: 20% (down from 90%)<br/>
      ✅ Disk usage: Normal<br/>
    </div>
  </div>
</div>

<div class="mt-6 bg-blue-100 color-black p-4 rounded">
<div class="font-bold mb-2">Group Task:</div>
<ol class="text-xs">
  <li>What additional information would you want to look for?</li>
  <li>What do you think is the root cause?</li>
  <li>What immediate actions would you take?</li>
  <li>What alerts should have triggered but didn't?</li>
</ol>
</div>

<!-- 
Database-specific metrics: Query plans, table scans vs. index scans, recent schema changes
Recent deployments: Was there a code or schema change recently?
Cache invalidation events: When and why did the cache hit rate drop?

Likely Causes:
Missing/unused index: The dramatic drop in index usage (90% → 20%) combined with slow queries strongly suggests an index problem
- Recent schema change that invalidated indexes
- New query pattern not covered by existing indexes
- Database statistics out of date
- Index corruption

-->

---

# Key Takeaways

<div class="grid grid-cols-2 gap-x-10 gap-y-6">
  <div>
    <h3 class="border-b pb-2 mb-2">Observability</h3>
    <ul>
      <li>Not just monitoring, but understanding</li>
      <li>Combine metrics, logs, and traces</li>
      <li>Design for unknown-unknowns</li>
      <li>Structured, correlated data is key</li>
    </ul>
  </div>
  <div>
    <h3 class="border-b pb-2 mb-2">Monitoring</h3>
    <ul>
      <li>The Four Golden Signals</li>
      <li>USE and RED methodologies</li>
      <li>Balance coverage with signal-to-noise</li>
      <li>Test your monitoring together with your code</li>
    </ul>
  </div>
  
  <div>
    <h3 class="border-b pb-2 mb-2">Alerting</h3>
    <ul>
      <li>Actionable, accurate, properly prioritized</li>
      <li>Fight alert fatigue aggressively</li>
      <li>Include context and next steps</li>
      <li>Regular review and refinement</li>
    </ul>
  </div>
  <div>
    <h3 class="border-b pb-2 mb-2">Implementation</h3>
    <ul>
      <li>Start small, iterate often</li>
      <li>Observability is a team culture, not just tools</li>
      <li>Learn from incidents</li>
      <li>Continuously improve your visibility</li>
    </ul>
  </div>
</div>

---

# Resources

<div class="grid grid-cols-3 gap-4">
  <div>
    <h3 class="mb-2">Books</h3>
    <ul class="text-sm">
      <li>The Phoenix Project</li>
      <li>Observability Engineering</li>
    </ul>
  </div>
  <div>
    <h3 class="mb-2">Online Resources</h3>
    <ul class="text-sm">
      <li><a href="https://opentelemetry.io/blog/2026/genai-observability/">OpenTelemetry: observing AI calls (2026)</a></li>
      <li>Prometheus & Grafana Tutorials</li>
      <li><a href="https://docs.datadoghq.com/cli/">Datadog pup CLI</a></li>
    </ul>
  </div>
  <div>
    <h3 class="mb-2">Communities</h3>
    <ul class="text-sm">
      <li>SRE Weekly Newsletter</li>
      <li>/r/devops on Reddit</li>
      <li>DevOps & SRE Stack Exchange</li>
    </ul>
  </div>
</div>

---

# Q&A

<div class="text-center">
  <img src="https://github.com/zachatrocity/o11y-slides/blob/3d3f7e786674d16ff131e8aab9899d8e83a2163d/images/PXL_20240928_150524524.jpg?raw=true" class="h-60 mx-auto">
  <p class="text-xl mt-4">What would you like to know more about?</p>
  <div class="mt-4 text-sm">
    Contact: zach@zachs.io | Slides available at: https://zachatrocity.github.io/o11y-slides/
  </div>
</div>

---

# Glossary of Terms

<div class="text-sm grid grid-cols-2 gap-x-10 gap-y-2">
  <div>
    <p><b>SLI (Service Level Indicator):</b> A metric that measures a specific aspect of service</p>
    <p><b>SLO (Service Level Objective):</b> A target value for an SLI</p>
    <p><b>SLA (Service Level Agreement):</b> A contract that includes consequences of meeting/missing SLOs</p>
    <p><b>Latency:</b> The time it takes to service a request</p>
    <p><b>Cardinality:</b> The number of unique values in a set of data</p>
    <p><b>RCA (Root Cause Analysis):</b> Process to identify the primary cause of an incident</p>
  </div>
  <div>
    <p><b>MTTD (Mean Time to Detect):</b> Average time to detect an incident</p>
    <p><b>MTTR (Mean Time to Recovery):</b> Average time to resolve an incident</p>
    <p><b>Golden Signals:</b> Latency, traffic, errors, and saturation</p>
    <p><b>Span:</b> An operation within a distributed trace</p>
    <p><b>Structured Logging:</b> Logging in a consistent, parseable format (often JSON)</p>
    <p><b>Instrumentation:</b> Code added to collect metrics or traces</p>
  </div>
</div>
