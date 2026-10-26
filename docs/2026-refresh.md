# 2026 presentation refresh

Reviewed October 25, 2026. Preserve the practical, interactive introduction; add AI without turning the talk into a vendor pitch.

## Source ledger

| Reference | Date / type | Use and caveat |
| --- | --- | --- |
| [OTel: Inside the LLM Call](https://opentelemetry.io/blog/2026/genai-observability/) | May 14, 2026; project article | GenAI instrumentation, usage and latency. Not a claim of universal SDK support. |
| [Anthropic: Demystifying evals for AI agents](https://www.anthropic.com/research/demystifying-evals-for-ai-agents) | January 9, 2026; vendor engineering guidance | Outcome evaluation complements traces. Graders need validation. |
| [Anthropic: Claude Code quality reports](https://www.anthropic.com/research/april-23-postmortem) | April 23, 2026; vendor incident report | Product/configuration regressions despite an unaffected API. Lessons in the deck are interpretation. |
| [OpenTelemetry GenAI conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/) | Living specification | Check individual convention maturity and instrumentation versions. |
| [AIOpsLab](https://www.microsoft.com/en-us/research/blog/aiopslab-building-ai-agents-for-autonomous-clouds/) | December 20, 2024; foundational research | Fault injection and agent evaluation; not evidence that autonomous production remediation is solved. |
| [Google SRE Workbook](https://sre.google/workbook/alerting-on-slos/) | Foundational guidance | Multiwindow burn-rate alerting; the slide shows only one fast-burn rule. |
| [AWS DynamoDB disruption](https://aws.amazon.com/message/101925/) | October 20, 2025 incident | Retained historical example, clarified DNS race and downstream recovery. |
| [Atlassian Opsgenie licensing](https://www.atlassian.com/licensing/opsgenie) | Lifecycle notice | End of sale June 4, 2025; end of support April 5, 2027. Removed from the suggested-tool list in favor of Jira Service Management. |

| [Grafana Cloud IRM](https://grafana.com/products/cloud/irm/) | Current product documentation | On-call and incident response example; not an endorsement or benchmark. |

## Editorial corrections

- Removed old universal downtime dollar figures and unsourced outage-loss estimates.
- Replaced the unattributed holiday-rush success statistics with the sourced 2026 AI incident.
- Marked the checkout exercise as hypothetical; no fabricated industry results.
- Replaced invalid JSON with one concise structured-log example, a valid trace ID, and release context. Removed unnecessary user/session identifiers.
- Distinguished OTel instrumentation/transport from trace storage and query backends.
- Replaced organization-specific P0/P1/P2 assumptions with page/ticket/info actions.
- Replaced long alerting product lists with a concise role-based toolkit and practical pup examples.
- Avoided recommending upgrades through every intervening Node release; use supported LTS releases and a measured canary.

## Consolidation and delivery plan

The original `main` deck had **32 slides**. The first refresh had **38**. The consolidated deck has **22 total: 21 through Q&A, plus one resources appendix**. That is 10 fewer than the original, including the AI material and a dedicated pup examples slide.

| Previous material | Consolidation |
| --- | --- |
| Three definitions and monitoring/observability distinction | One three-question framing slide; alert activity moves to alert routing |
| Five section-divider slides | Removed; use the content headings as transitions |
| Before/after log examples | One readable structured-log example alongside the golden signals |
| Tracing definition and flamegraph image | One request-path diagram and diagnostic example |
| Two tool inventories | One four-row toolkit, including pup |
| Monitoring types, best practices, and pitfalls | One journey-coverage and telemetry-budget slide |
| Alert severity cards and design principles | One page/ticket/event routing slide |
| Chaos Monkey and two standalone canary screenshots | Removed from the speaking path; retained in Git history |
| Glossary | Removed; essential terms explained where used |
| AI outcomes, tracing, copilot guidance, real incident | Retained, with pup queries connecting concepts to practice |

### Rehearsal budget (proposal, not a measured duration)

- Opening, bio, framing, impact, feedback loop (1–5): 3 minutes.
- Signals, tracing, compact toolkit, OTel, pup (6–10): 5 minutes.
- AI outcomes, workflow tracing, on-call assistance (11–13): 3 minutes.
- Monitoring, SLOs, alert routing (14–16): 3 minutes.
- AI incident, canary, checkout exercise (17–19): 4 minutes.
- Takeaways (20): 1 minute; Q&A (21): reserve 5 minutes.
- Resources (22): appendix, do not present line by line.

This suggests about 19 minutes of speaking plus 5 minutes of Q&A. Actual delivery must be rehearsed against the event's time limit. Cap pup at 90 seconds; do not log in live. Take one audience response per discussion prompt. For a tighter slot, skip the feedback-loop and monitoring-budget slides and make the checkout exercise a take-home activity.

### pup snippet verification

The slide uses Datadog's `pup`, not the unrelated HTML-processing utility of the same name. Two query examples: checkout error logs and production CPU metrics, using explicit 30-minute windows and `--read-only`. Authentication is a prerequisite, not part of the demo.

Checked against [upstream examples at commit 6c3a470](https://github.com/DataDog/pup/blob/6c3a4707f5ab87688566020d868a50810db08166/docs/EXAMPLES.md) and [the command reference](https://github.com/DataDog/pup/blob/6c3a4707f5ab87688566020d868a50810db08166/docs/COMMANDS.md) on October 25, 2026. No authenticated Datadog calls were run; service names, tags, and available metrics depend on the account. Read-only mode complements scoped permissions, not a replacement for them. No sample results are invented.

## Recommended follow-ups

1. **A five-minute recorded demo:** show one checkout trace, add a slow dependency or bad retrieval result, and follow the signal to a fix. Keep a recording as the conference-Wi-Fi fallback.
2. **Rehearse for the allotted time:** use the budget above and tighten discussion time first. The deck is now shorter than the original; slide count alone is not a timing guarantee.
3. **Confirm personal details:** the existing employer, title, and Matrix handle were preserved. Review for current accuracy before presenting publicly. The old standalone dashboard screenshots were removed from the talk.
4. **Make media self-contained:** the existing remote cover art and personal photos still depend on network access. Check reuse permissions before vendoring third-party assets. The remote GIF and flamegraph were removed in the consolidation.
5. **Reproducible publishing:** pin the runtime/package-manager/theme versions and add a build-only PR workflow. Current Pages workflow deploys only from main; dependency/CI modernization is intentionally outside this content PR.
6. **Audience participation:** use the AI case study to ask “which dashboard would notice?” Then ask how to prove a task succeeded without storing raw user prompts.

## Scope

No employer facts were invented, no live incident demo was deployed, and no production changes were made. Sources are primary project/vendor publications, not independent validation of vendor claims. No commercial product benchmark or adoption statistic is asserted.
