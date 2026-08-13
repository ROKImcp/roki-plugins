---
description: Audit this project's ROKI Connect integration against the verified contract
---

Audit the ROKI Connect integration in this project.

$ARGUMENTS

Call `roki_audit_integration` for the checklist, then verify each item against the actual code.

Rules for this audit:

- **Report file and line for every finding.** A claim without a location is not a finding.
- **An item you cannot verify is a finding, not a pass.** Say what you could not confirm and why.
- **Check field names against `roki_get_schema`, not against your expectations.** This API accepts
  unknown fields silently, so code that looks correct can be doing nothing. Run any payload you find
  through `roki_validate_request`.
- **Do not report style opinions.** Only things that will fail, cost money, or leak.

Pay particular attention to the failures that are invisible until production:

- A wrong field name that returns `201` with the feature off (`service_fee` instead of
  `service_fee_enabled` is the classic).
- Void, refund or receipts called with the numeric payment id instead of the transaction UUID - that
  produces a routing 404 that looks like a missing endpoint.
- Webhook signatures computed over re-serialized JSON instead of the raw body.
- `success_url` treated as confirmation of payment.
- A missing or badly derived `Idempotency-Key`.
- Amounts handled as cents, or compared as floats.
- The secret key reachable from browser code, a mobile binary, the repository or a log line.

Order findings by how badly each one fails in production, worst first. If the integration is sound,
say so plainly rather than inventing findings.
