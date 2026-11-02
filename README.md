# SignalPatch

**Know what needs attention.**

SignalPatch is an original security-review workspace for small IT teams. It is designed around a practical question: *what should we verify first, and where is the evidence?* It brings CVSS severity, CISA known-exploitation status, NVD product scope, recommended verification steps, and primary-source links into one calm review surface.

## Live preview

[Open SignalPatch](https://goutham-valugula.github.io/SignalPatch/) · [Source repository](https://github.com/goutham-valugula/SignalPatch)

On first load, the dashboard uses clearly marked illustrative sample records. Press **Load live data** to fetch the latest 24 hours of NVD disclosures (up to 100 records) and cross-reference them with the CISA Known Exploited Vulnerabilities catalog. The KEV data is fetched from [CISA's official GitHub mirror](https://github.com/cisagov/kev-data), which mirrors the catalog CISA publishes. Requests run in the visitor's browser only when requested; there is no background polling, API key, user account, or app server.

## What it does

- Searchable advisory stream with severity filters, exploited-only and saved views.
- CVE brief with the NVD record, CVSS severity, CISA KEV match, listed CPE product/version scope, and a recommended verification step.
- One-click copy of a human-review broadcast draft. It does not send messages.
- Responsive dark interface with explicit demo/live state and feed errors.
- An inactive [n8n workflow starter](workflows/signalpatch-review-intake.json) for scheduled source intake and review-workflow design.

## Start locally

```sh
npm install
npm run dev
```

Create a production build with `npm run build`.

## n8n and Telegram roadmap

Import the workflow JSON into n8n and keep it inactive while validating it. The starter fetches public NVD and CISA KEV data and normalizes a review list. It does not call an LLM, send Telegram messages, or contain credentials. Before operational use, validate response shapes and date expressions against your n8n version, add durable deduplication, and connect a private review queue.

Telegram should be a separate, approval-triggered workflow: an authorized reviewer approves and previews a specific draft, an idempotency key prevents duplicate posts, and n8n credential storage holds the bot token. Use a private staging channel first and grant the bot only the posting permission it needs. Never route the scheduled intake directly to public publishing.

## Trust boundaries

- NVD and CISA records are public signals, not a complete view of your organization’s risk.
- The live browser fetch may fail because of source availability or rate limits; the app then keeps the last/current sample visible and explains the failure.
- Only the first 100 NVD CVEs published in the last 24 hours are requested. This is not a complete vulnerability feed or monitoring service.
- NVD CPE scope is shown when supplied; it is not proof that a local installation is affected. Unknown scope stays unknown.
- Severity is not asset risk. No organization-specific inventory is configured.
- Verify version ranges, fixes, and exploitation details at the linked vendor/CISA source before acting.
- No exploit instructions are run or published. This is not incident response, compliance certification, or a patch oracle.

## Next product milestones

1. Add source timestamps, pagination, and feed health/retry visibility.
2. Add an optional local product/version inventory and conservative affected-range matching.
3. Add review states, audit history, durable duplicate suppression, and preview.
4. Connect approved Telegram publishing only after a channel and private staging flow are configured.
