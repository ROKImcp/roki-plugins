# ROKI Connect plugin for Claude Code

Integrate ROKI Connect payments without inventing endpoints.

Installing this plugin connects the official ROKI MCP server automatically - no configuration file
to copy, no URL to paste - and adds four commands for the work that actually takes time.

## Install

```
/plugin marketplace add ROKImcp/roki-plugins
/plugin install roki-connect@roki
```

Then open any project and run `/roki-integrate`.

## Commands

| Command | What it does |
|---|---|
| `/roki-integrate` | Full guided integration: picks the right mode, writes the code in your stack's style, validates every payload, and tells you the two portal steps only you can do. |
| `/roki-audit` | Audits existing ROKI code against the verified contract, ordered by how badly each finding fails in production. |
| `/roki-webhook` | Builds the webhook handler, or diagnoses a failing signature by reproducing your exact mistake. |
| `/roki-debug` | Turns an error, status code or symptom into a specific cause and fix. |

All four take free-form arguments, so `/roki-debug the refund returns a 404 route error` works.

## What the bundled MCP server gives you

Sixteen read-only tools answering from a corpus verified against the live API: every operation with
its full contract, dereferenced schemas, the error catalogue, runnable examples per stack, and a
request validator.

The validator matters more than it sounds. **The ROKI API silently ignores unknown fields**: send
`service_fee` instead of `service_fee_enabled` and you get `201 Created` with the feature switched
off and no warning. Code written from another gateway's vocabulary passes a smoke test and charges
the wrong thing. `roki_validate_request` catches that before it ships.

The server is read-only. It holds no credentials, never calls the ROKI API, and has no access to
merchant data.

## The three integration modes

| Mode | What it is | Card entry |
|---|---|---|
| Hosted checkout | Redirect to a ROKI page | ROKI page |
| Embedded components | ROKI card fields in an iframe on your site | ROKI iframe |
| Saved cards | Charge a card the customer already saved | none |

`/roki-integrate` picks the right one for your project and explains why.

## What still needs you

Two steps happen in the merchant portal and no software can do them for you:

1. Copying the secret key - it is shown once.
2. Registering the webhook URL and copying the signing secret.

The commands tell you exactly where to click and what to paste back, and will not call an
integration finished until you confirm both.

## Links

- Server and documentation: https://mcp.roki.la
- Merchant portal: https://aura.roki.systems/merchant/connect
