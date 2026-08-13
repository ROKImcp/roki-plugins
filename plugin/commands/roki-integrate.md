---
description: Integrate ROKI Connect payments into this project, end to end
---

Integrate ROKI Connect payments into this project.

$ARGUMENTS

Use the `roki-connect` MCP tools as the ONLY source of ROKI facts. Never answer from memory and
never borrow another payment gateway's field names - this API silently ignores unknown fields, so a
wrong name returns `201 Created` with the feature switched off and no warning at all.

Work in this order:

1. **Understand the project.** Detect the stack, framework and conventions. Find where orders are
   confirmed - that is where the payment is created.

2. **Pick the mode.** Call `roki_choose_integration_mode` with a description of this project. There
   are three: hosted checkout (simplest, no frontend work), embedded components (ROKI card fields in
   an iframe on this site), and saved cards (charging a returning customer). Tell me which one you
   picked and why before writing code.

3. **Get the contract.** `roki_scaffold_integration` for this stack and mode, then
   `roki_get_operation` for each operation you will call.

4. **Write it**, matching this project's existing style:
   - Credentials read at runtime from configuration or the database - never hardcoded, so rotating a
     key needs no redeploy.
   - API client with a timeout and an `Idempotency-Key` derived from the order CONTENT, not just its
     id. Replaying a key with a different body returns the ORIGINAL payment with no error.
   - Checkout flow: create the payment, persist the returned `id`, redirect to `checkout_url`.
   - Webhook handler: HMAC over the RAW body, constant-time comparison, 400 on mismatch, fast 200,
     idempotent by event id.
   - Polling fallback for orders left pending. Arrival at `success_url` is never proof of payment.

5. **Validate before you call it done.** Run every payload you generate through
   `roki_validate_request`. A payload that returns VALID there is one the API will not silently
   mangle.

6. **Tell me my part.** Two steps only I can do, in the portal:
   - Copy the secret key from https://aura.roki.systems/merchant/connect/api-integration - it is
     shown once.
   - Register the webhook URL at https://aura.roki.systems/merchant/connect/webhooks, with the
     environment toggle matching the key, then give you the signing secret.

   Give me the exact URL to register and what to paste back. Do not mark the integration complete
   until I confirm both.

7. **Prove it works.** Walk me through one test payment end to end, and tell me what I should see at
   each step.

If something is not in the MCP corpus, it does not exist. Say so instead of guessing.
