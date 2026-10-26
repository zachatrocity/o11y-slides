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

<div class="mt-6 text-sm opacity-70">2026 refresh · Reliability for services and AI workflows</div>

---
layout: about-me
helloMsg: Hello!
name: Zach Russell
imageSrc: https://github.com/zachatrocity/o11y-slides/blob/3d3f7e786674d16ff131e8aab9899d8e83a2163d/images/PXL_20231029_010441145.MP.jpg?raw=true
position: left
job: Lead Software Engineer
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

Better approach: page on user-impacting symptoms or fast error-budget burn; use CPU as diagnostic context. Zero signups needs expected traffic and telemetry freshness checks.

Consider if there's an automated remediation that could be tried first

-->
---
layout: section
---

# Why Are These Things Important?

---

# The Real Cost of Downtime

<div grid="~ cols-2 gap-8">
<div>

## Start with your user journey
- Failed signups and abandoned checkouts
- Delayed work, support load, lost trust
- Recovery effort and contractual impact

## Estimate, don't borrow a headline
At-risk revenue ≈ affected attempts × baseline conversion × value per conversion

</div>
<div class="bg-blue-100 color-black p-4 rounded text-sm">

**Historical example · October 20, 2025**

AWS traced the US-EAST-1 DynamoDB disruption to a race condition in DNS management. Recovery also involved downstream EC2 and Network Load Balancer problems.

**Discussion:** Can your synthetic checkout detect a dependency failure when your own hosts look healthy?

[Source: AWS incident report](https://aws.amazon.com/message/101925/)

</div>
</div>

<!--
The formula is an illustrative planning estimate, not realized loss: some users retry,
some transactions are deferred, and conversion varies by cohort. Avoid universal
cost-per-minute figures. This AWS example was already present in the 2025 deck;
it is retained as historical context, not presented as a new 2026 incident.
-->

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
layout: section
---

# Best Practices and Tools in Practice

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
  "timestamp": "2026-10-25T13:45:30.123Z",
  "level": "ERROR",
  "service": "payment-processor",
  "message": "Payment transaction failed",
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
  "timestamp": "2026-10-25T13:45:30.123Z",
  "level": "ERROR",
  "service": "payment-processor",
  "trace_id": "4bf92f3577b34da6a3ce929d0e0e4736",
  "span_id": "00f067aa0ba902b7",
  "service_version": "checkout-2026.10.25",
  "release_cohort": "canary",
  "region": "LATAM",
  "message": "Payment transaction failed",
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
    <h3 class="text-blue-700">Metrics</h3>
    <ul class="text-sm mt-2">
      <li>Prometheus</li>
      <li>Datadog</li>
      <li>Grafana</li>
      <li>New Relic</li>
    </ul>
  </div>
  <div class="bg-green-50 p-3 rounded">
    <h3 class="text-green-700">Logging</h3>
    <ul class="text-sm mt-2">
      <li>Sentry</li>
      <li>Loki</li>
      <li>Splunk</li>
      <li>Kibana</li>
    </ul>
  </div>
  <div class="bg-purple-50 p-3 rounded">
    <h3 class="text-purple-700">Tracing</h3>
    <ul class="text-sm mt-2">
      <li>Jaeger</li>
      <li>Zipkin</li>
      <li>AWS X-Ray</li>
      <li>Grafana Tempo</li>
    </ul>
  </div>
</div>

---

# OpenTelemetry: Instrument Once

<div grid="~ cols-2 gap-8">
<div>

```mermaid
flowchart LR
  App[Services and AI workflows] --> SDK[OTel instrumentation]
  SDK --> Collector[Collector]
  Collector --> Traces[Trace backend]
  Collector --> Metrics[Metrics backend]
  Collector --> Logs[Log backend]
```

</div>
<div class="text-sm">

- **OpenTelemetry is instrumentation and transport**, not a storage/query UI.
- Correlate logs and spans; carry context across HTTP, queues, and tool calls.
- Attach service version and release cohort to make regressions explainable.
- Control cardinality, sampling, retention, and sensitive data before export.

</div>
</div>

<div class="text-xs mt-4">
<a href="https://opentelemetry.io/docs/">OpenTelemetry documentation</a> · GenAI conventions are evolving: check the status of the specific conventions you adopt.
</div>

---
layout: section
---

# AI Changes What “Healthy” Means
## Observability of AI ≠ AI for observability

---

# HTTP 200 Is Not a Successful AI Task

<div grid="~ cols-2 gap-8">
<div>

## Keep the golden signals
- Request and end-to-end latency
- Time to first token for streaming UX
- Errors, timeouts, retries, rate limits
- Token usage and estimated cost per task

</div>
<div>

## Add outcome signals
- Did the task actually complete?
- Was the answer supported by evidence?
- Did the tool perform the intended action?
- Did the user correct or abandon it?

</div>
</div>

<div class="bg-blue-100 color-black p-3 rounded mt-5 text-sm">
A fast, inexpensive answer can still be wrong. Track quality, latency, and cost together; segment by model, prompt version, and release cohort.
</div>

<div class="text-xs mt-4">
<a href="https://opentelemetry.io/blog/2026/genai-observability/">OTel: Inside the LLM Call (May 2026)</a> · <a href="https://www.anthropic.com/research/demystifying-evals-for-ai-agents">Anthropic: Agent evals (January 2026)</a>
</div>

<!--
Outcome measures depend on the task; these are suggested signals, not a universal
standard. Model-graded quality is an estimate: calibrate against human-reviewed
examples and deterministic outcome checks. Token-derived cost needs provider
pricing and cache accounting; it is not an invoice.
-->

---

# Trace the Workflow, Not Just the Model Call

```mermaid
flowchart LR
  Request[User task] --> Retrieve[Retrieve context]
  Retrieve --> Model[Model call]
  Model --> Tool[Tool execution]
  Tool --> Verify[Verify outcome]
  Verify --> Response[Response]
```

- One trace connects retrieval, inference, tool calls, retries, and verification.
- Record model identity, prompt/config version, durations, and usage.
- Link sampled eval outcomes back to traces; compare release cohorts.
- Don't capture prompts, responses, or tool payloads by default: they may contain secrets or personal data.

<div class="text-xs mt-5">
<a href="https://opentelemetry.io/docs/specs/semconv/gen-ai/">OpenTelemetry GenAI semantic conventions</a> · Check per-convention maturity and pin versions; do not assume every SDK emits the same fields.
</div>

<!--
This is a conceptual workflow, not a claim that all agents follow a linear graph.
Use child spans or links as appropriate for asynchronous or multi-agent work.
Prompt/config version and outcome fields may be application-specific attributes.
Keep high-cardinality task identifiers in traces, not metric labels. If content
capture is needed, use explicit access controls, redaction, sampling, and retention.
-->

---

# AI for On-Call: A Copilot with Evidence

<div grid="~ cols-2 gap-8">
<div>

## Useful assistance
- Summarize an incident timeline
- Draft queries and find relevant runbooks
- Compare deployments and trace cohorts
- Propose hypotheses with source links

</div>
<div>

## Earn the right to automate
- Start with read-only access
- Test against known, injected failures
- Measure diagnosis and recovery outcomes
- Bound actions; require approval for risky changes

</div>
</div>

<div class="bg-yellow-100 color-black p-3 rounded mt-5 text-sm">
A plausible root-cause story is not proof. Require telemetry evidence, an audit trail, and a rollback path.
</div>

<div class="text-xs mt-4">
Research reference: <a href="https://www.microsoft.com/en-us/research/blog/aiopslab-building-ai-agents-for-autonomous-clouds/">Microsoft Research: AIOpsLab (December 2024)</a> — a fault-injection/evaluation framework, not proof of production autonomy. Guardrails above are recommendations for this talk.
</div>

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

# Page on User Impact: An SLO Example

- **SLI:** successful eligible checkout attempts / all eligible checkout attempts
- **SLO:** 99.9% over 30 days → **0.1% error budget**
- **Burn rate:** observed bad-event fraction / allowed bad-event fraction
- At **1% failures**, burn rate = 0.01 / 0.001 = **10×**

<div class="bg-blue-100 color-black p-4 rounded mt-5 text-sm">
Example fast-burn page: burn rate above <b>14.4× in both 1h and 5m windows</b>.<br>
The longer window establishes impact; the shorter confirms it is still happening.
</div>

Define eligibility, low-traffic behavior, missing-data alerts, ownership, and a runbook before paging.

<div class="text-xs mt-4">
Foundational reference (not new in 2026): <a href="https://sre.google/workbook/alerting-on-slos/">Google SRE Workbook: Alerting on SLOs</a>. This is one fast-burn rule, not a complete paging policy.
</div>

<!--
For a 30-day window, a sustained 14.4x burn over one hour spends about 2% of the
budget under steady traffic. Pair with slower-burn rules and ticket routing.
Define eligibility explicitly (e.g. distinguish expected payment declines from
service failures). Low-volume journeys need synthetics and careful denominators.
-->

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
    <h3 class="text-red-600 text-center border-b pb-1">Page now</h3>
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
    <h3 class="text-orange-500 text-center border-b pb-1">Ticket / investigate</h3>
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
    <h3 class="text-blue-500 text-center border-b pb-1">Informational</h3>
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

# Alerting: Tools and Frameworks

<div class="mb-4">
Effective alerting systems typically combine multiple tools:
</div>

<div class="grid grid-cols-3 gap-4">
  <div class="border p-3 rounded">
    <h3 class="text-center border-b pb-1">Alert Generation</h3>
    <ul class="text-sm mt-2">
      <li>Prometheus alerting rules</li>
      <li>Grafana Alerting</li>
      <li>Datadog Monitors</li>
      <li>New Relic Alerts</li>
      <li>CloudWatch Alarms</li>
    </ul>
  </div>
  <div class="border p-3 rounded">
    <h3 class="text-center border-b pb-1">Alert Delivery</h3>
    <ul class="text-sm mt-2">
      <li>PagerDuty</li>
      <li>Jira Service Management</li>
      <li>Grafana IRM</li>
      <li>Slack/Teams/Discord</li>
      <li>Email/SMS</li>
    </ul>
  </div>
  <div class="border p-3 rounded">
    <h3 class="text-center border-b pb-1">Alert Management</h3>
    <ul class="text-sm mt-2">
      <li>Alertmanager grouping/routing</li>
      <li>On-call rotation tools</li>
      <li>Runbook automation</li>
      <li>Post-mortem systems</li>
      <li>Synthetic testing</li>
    </ul>
  </div>
</div>

---
layout: section
---

# Case Studies and Real-World Examples

---

# Case Study 1: The Netflix Chaos Monkey

<div grid="~ cols-1 gap-4">
<div>

## The Challenge
- Ensuring resilience in a cloud infrastructure
- Detecting failures before they impact customers
- Building a culture of observability

</div>
<div>

```mermaid
---
config:
  look: handDrawn
  layout: elk
---
graph LR
  ChaosMonkey[Chaos Monkey] -->|Introduces Failures| System[System Under Test]
  System -->|Monitored By| ObservabilityTools[Observability Tools]
  ObservabilityTools -->|Detect Issues| Engineers[Engineers]
  Engineers -->|Improve Resilience| System
```

</div>
</div>

<!--

- Deliberately introducing failures
- Chaos Engineering as a practice
- "Break things on purpose"

-->

---

# Case Study 2: AI Quality Regressions in 2026

<div grid="~ cols-2 gap-8">
<div>

## What Anthropic reported
- Reasoning-effort defaults changed
- A caching bug repeatedly cleared prior reasoning
- A prompt change hurt coding quality
- The API and inference layer were unaffected

</div>
<div>

## What we can learn
- Observe the product, not just the endpoint
- Track configuration and prompt changes
- Test long-lived sessions, not only fresh ones
- Segment results: aggregates hide affected cohorts

</div>
</div>

<div class="text-xs mt-5">
<a href="https://www.anthropic.com/research/april-23-postmortem">Anthropic postmortem · April 23, 2026</a>. Vendor's incident account; lessons on the right are our interpretation, not measured claims of improvement.
</div>

<!--
Anthropic describes three changes affecting Claude Code, the Agent SDK, and Cowork,
resolved by April 20. Use this to distinguish application/harness failures from
model-serving availability. Ask: which of our current dashboards would notice?
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
  - a supported LTS release to the next supported LTS release
- Validate and ensure no negative user impact
</div>

<div>

## Approach
1. Upgrade on a feature branch
2. Run E2E tests in staging; inspect telemetry
3. Start a small, representative canary
4. Compare user SLIs; expand or roll back
5. For AI, compare task quality and cost

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

# Hands-On Exercise: Checkout Is Slow

<div class="text-center mb-4">
Hypothetical training scenario (all numbers are illustrative): checkout is slow.
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
layout: section
---

# Conclusion

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
      <li>For AI: verify outcomes, not just HTTP success</li>
    </ul>
  </div>
</div>

---

# Resources: Keep Learning

<div grid="~ cols-2 gap-8" class="text-sm">
<div>

## New for this refresh
- [OTel: Inside the LLM Call (May 2026)](https://opentelemetry.io/blog/2026/genai-observability/)
- [Anthropic: Agent evals (January 2026)](https://www.anthropic.com/research/demystifying-evals-for-ai-agents)
- [Anthropic: Quality postmortem (April 2026)](https://www.anthropic.com/research/april-23-postmortem)
- [Microsoft: AIOpsLab (2024 foundations)](https://www.microsoft.com/en-us/research/blog/aiopslab-building-ai-agents-for-autonomous-clouds/)

</div>
<div>

## Foundations and living references
- [OpenTelemetry documentation](https://opentelemetry.io/docs/)
- [GenAI semantic conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/)
- [Google SRE: Alerting on SLOs](https://sre.google/workbook/alerting-on-slos/)
- [AWS: October 2025 incident report](https://aws.amazon.com/message/101925/)

</div>
</div>

<div class="text-xs mt-6">Reviewed October 25, 2026 · Source notes and presentation recommendations: docs/2026-refresh.md in the repository.</div>

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
