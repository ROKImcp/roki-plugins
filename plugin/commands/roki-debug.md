---
description: Diagnose a failing ROKI Connect call from an error, status code or symptom
---

Diagnose this ROKI Connect problem.

$ARGUMENTS

Do not guess. Work from the corpus:

1. `roki_get_error` with the status code or the message text.
2. `roki_search_docs` for the behaviour involved.
3. Read the actual code in this project and compare it against `roki_get_operation` and
   `roki_get_schema`.
4. If a request payload is involved, run it through `roki_validate_request` - this API accepts
   wrong field names silently, so a payload can be "working" and still be wrong.

Some symptoms have a specific cause worth checking first:

- **`The route ... could not be found`** - either `/v1` is missing from the base URL, or a numeric
  payment id was passed where void, refund and receipts require the transaction UUID. The endpoint
  exists; the identifier is wrong.
- **`Pago no encontrado`** - the payment id belongs to the other environment. Sandbox and production
  ids do not cross.
- **A payment created without its tax, tip or service fee** - a field name was misspelled and
  silently ignored. Compare against the schema.
- **422 about `currency_code` in sandbox** - the sandbox terminal is not provisioned for that
  merchant. No code change fixes it.
- **Webhooks not arriving** - the endpoint is registered under the wrong environment, or the URL is
  not public HTTPS.
- **Signature failures** - use `roki_verify_webhook_signature`, which identifies the exact mistake.
- **A charge rejected with `Idempotency-Key` missing** - that header is mandatory on saved-card
  charges, unlike payment creation where it is only recommended.

Give me the specific cause and the fix, with file and line. Not a list of possibilities.
