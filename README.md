# Observability, Monitoring and Alerting

A practical guide to work for the weekend instead of on the weekend.

**[View the published presentation](https://zachatrocity.github.io/o11y-slides/)** · [Slide source](slides.md) · [2026 references and recommendations](docs/2026-refresh.md)

The October 2026 refresh covers metrics, logs, traces, user-impact alerting, OpenTelemetry, and reliability for AI workflows. It distinguishes observing AI systems from using AI during incident response.

## Run locally

The repository includes a Bun lockfile:

```sh
bun install --frozen-lockfile
bun run dev
```

## Build for GitHub Pages

```sh
bun run build --base /o11y-slides/
```

The existing GitHub Pages workflow publishes changes pushed to `main`. A PR does not update the public deck until merged and deployed.

## Presenting

Edit `slides.md`; HTML comments contain speaker notes. Sources are linked on relevant slides and in the reference ledger. Illustrative scenarios are labeled, and historical references retain their dates.

Built with [Slidev](https://sli.dev/).
