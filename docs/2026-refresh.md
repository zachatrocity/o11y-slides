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
- Fixed invalid JSON in the enriched-log example and added valid trace/span IDs and deployment context. Removed unnecessary user/session identifiers.
- Distinguished OTel instrumentation/transport from trace storage and query backends.
- Replaced organization-specific P0/P1/P2 assumptions with page/ticket/info actions.
- Corrected Prometheus rule evaluation versus Alertmanager grouping/routing.
- Avoided recommending upgrades through every intervening Node release; use supported LTS releases and a measured canary.

## Recommended follow-ups

1. **A five-minute recorded demo:** show one checkout trace, add a slow dependency or bad retrieval result, and follow the signal to a fix. Keep a recording as the conference-Wi-Fi fallback.
2. **Rehearse for the allotted time:** this refresh adds six slides. For a short talk, skip the historical Chaos Monkey slide and the tool inventories, keeping the new AI case study and SLO example.
3. **Confirm personal details:** the existing employer, title, Matrix handle, and dashboard screenshots were preserved. Review for current accuracy and suitability before presenting publicly.
4. **Make media self-contained:** existing remote cover art, GIF, and screenshots still depend on network access. Check reuse permissions before vendoring third-party assets; add captions/alt text to the historical screenshots.
5. **Reproducible publishing:** pin the runtime/package-manager/theme versions and add a build-only PR workflow. Current Pages workflow deploys only from main; dependency/CI modernization is intentionally outside this content PR.
6. **Audience participation:** use the AI case study to ask “which dashboard would notice?” Then ask how to prove a task succeeded without storing raw user prompts.

## Scope

No employer facts were invented, no live incident demo was deployed, and no production changes were made. Sources are primary project/vendor publications, not independent validation of vendor claims. No commercial product benchmark or adoption statistic is asserted.
