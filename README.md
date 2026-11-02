# SignalPatch

**Know what needs attention.**

SignalPatch is an original security-review workspace for small IT teams. It is designed around a practical question: *what should we verify first, and where is the evidence?* It brings severity, known exploitation status, affected-version context, suggested next steps, and primary-source references into one calm review surface.

This repository contains a front-end product prototype and a deliberately inactive n8n intake starter. It does not copy code from the repository that motivated the problem exploration.

## What is here

- Searchable advisory stream with severity filtering, exploited-only and saved views.
- Selected advisory brief with affected versions, source confidence, primary link, and a copyable broadcast draft.
- A responsive dark interface designed for an operator to review evidence quickly.
- A clear **demo mode**: sample records are illustrative, dates are sample values, and no live feed is connected.
- An inactive [n8n workflow starter](workflows/signalpatch-review-intake.json) showing NVD + CISA KEV intake, deterministic prioritization, and an explicit human-review rollout path.

## Start

```sh
npm install
npm run dev
```

Create a production build with `npm run build`.

## Workflow rollout

Import the workflow JSON into n8n and keep it inactive while validating it. The starter fetches public NVD and CISA KEV data and normalizes a review list. It does not call an LLM, send Telegram messages, or contain credentials. Before use, validate the current response shapes and time-window syntax in your n8n version, add durable deduplication, and connect an authenticated review queue.

Telegram should be a separate, approval-triggered workflow: an authorized reviewer approves and previews a specific draft, an idempotency key prevents duplicate posts, and n8n credential storage holds the bot token. Use a private staging channel first and grant the bot only the channel posting permission it needs. Never route the scheduler directly to public publishing.

## Trust boundaries

- **No live security data yet.** Demo advisories are not operational intelligence; do not use them to make patch or incident decisions.
- **Primary sources win.** Verify version ranges, fixes, and exploitation claims at the linked vendor advisory/NVD/CISA source.
- **Severity is not asset risk.** No organization-specific inventory is configured. Unknown product match must remain unknown, not “not affected.”
- **Human judgment stays in the loop.** Future generated summaries should be source-linked and reviewed. No model should approve or publish.
- **No exploit instructions.** This project is for defensive triage, not payloads, exploit code, or offensive testing.

## Next product milestones

1. Replace demo rows with a small, source-provenance-preserving ingestion service.
2. Add an optional local product/version inventory and conservative affected-range matching.
3. Add review states, audit history, duplicate suppression, preview, and authorized Telegram publishing.
4. Add feed-health/error visibility and operational monitoring before claiming coverage.
