---
description: Diagnose a failing ROKI webhook signature, or build the handler
---

Work on ROKI webhooks in this project.

$ARGUMENTS

**If a signature is failing**, this is the fastest path to the answer. Collect three things from a
real rejected event - the exact raw body, the full `ROKI-Signature` header, and the signing secret
for that environment - and pass them to `roki_verify_webhook_signature`.

That tool does not just say "invalid". It reproduces the specific wrong constructions developers
actually write, and when one matches your signature it names your exact mistake: signing the body
without the timestamp, omitting the dot separator, reversing the order, or signing re-serialized
JSON instead of the raw bytes. If none match, it points at the next most likely cause - usually the
wrong environment's secret, since sandbox and production have separate ones.

Ask me for those three values if you do not have them. Never guess at the cause when the tool can
tell you.

**If you are building the handler**, get it from `roki_get_webhook_guide` and respect these four
rules, each of which fails silently when broken:

1. Capture the raw body **before** any JSON-parsing middleware. This is the single most common bug.
2. Compare in constant time - `hash_equals`, `crypto.timingSafeEqual`, `hmac.compare_digest`.
3. Respond 400 on an invalid signature, and 200 fast on a valid one. Defer heavy work.
4. Deduplicate on the event id. Events can be redelivered, and order is not guaranteed.

Also handle the events that arrive without you asking: `payment.voided`, `payment.refunded` and
`payment.partially_refunded` fire when a human acts in the merchant portal, not only when your code
calls the API. A handler that assumes it is the only thing changing state will be wrong.

Remind me that the endpoint must be registered in the portal under the environment matching the key
in use, and that "Entregas recientes" there shows the last ten delivery attempts - that is the place
to look when nothing is arriving.
