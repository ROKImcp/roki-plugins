# ROKI plugins for AI coding agents

Official plugins that let an AI coding assistant integrate ROKI payments without inventing
endpoints.

## Install

```
/plugin marketplace add daniloantunez/roki-plugins
/plugin install roki-connect@roki
```

Then open any project and run `/roki-integrate`.

## What's here

### roki-connect

Bundles the official [ROKI Connect MCP server](https://mcp.roki.la) - installing the plugin connects
it automatically, with no configuration file to copy - and adds four commands:

| Command | What it does |
|---|---|
| `/roki-integrate` | Full guided integration: picks the right mode, writes the code in your stack's style, validates every payload, and tells you the two portal steps only you can do. |
| `/roki-audit` | Audits existing ROKI code against the verified contract, worst findings first. |
| `/roki-webhook` | Builds the webhook handler, or diagnoses a failing signature by reproducing your exact mistake. |
| `/roki-debug` | Turns an error, status code or symptom into a specific cause and fix. |

The MCP server it connects to is read-only: it holds no credentials, never calls the ROKI API, and
has no access to merchant data. It answers from a corpus verified empirically against the live API.

## Why it exists

The ROKI Connect API silently ignores unknown fields. Send `service_fee` instead of
`service_fee_enabled` and you get `201 Created` with the feature switched off and no warning at all.
Code written from another gateway's vocabulary passes a smoke test and charges the wrong thing.

These plugins make that failure mode impossible: the agent looks up the real contract, and validates
its payload before shipping.

## Links

- Server and documentation: https://mcp.roki.la
- Merchant portal: https://aura.roki.systems/merchant/connect

## License

Proprietary - ROKI.
