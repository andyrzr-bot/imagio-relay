# Imagio Relay

**57 deterministic, pay-per-call tools for AI agents over [x402](https://www.x402.org)** â€” pay in USDC on Base, settled through the [PayAI facilitator](https://facilitator.payai.network).

- **Native MCP server:** `https://relay.imagiolabs.com/mcp` (Streamable HTTP)
- **Live service manifest:** https://relay.imagiolabs.com
- **x402 manifest:** https://relay.imagiolabs.com/.well-known/x402
- **MCP Registry entry:** [`com.imagiolabs.www/imagio-relay`](https://registry.modelcontextprotocol.io)

Every endpoint is deterministic unless labeled LLM-backed, returns clean JSON, and prices are dynamic (market-floor + analyst-adjusted) between **$0.0015 and $0.05 per call**.

## What's inside

| Group | Tools (examples) |
|---|---|
| **Web intelligence** | `/http-probe` (redirect chain + TLS snapshot), `/links`, `/html-to-md`, `/content-guard` (crawl-trap honeypot scan), `/outlink-retry` (dead-link rescue via archive.org), `/geo-gate`, `/rss-gate`, `/wiki-pack` |
| **Search & trends** | `/search-pack` (web/news/github), `/catalyst` (fresh trend sources), `/sentiment` (feed vectorizer), `/market-sent` (Fear & Greed aggregation) |
| **On-chain & trade math** | `/market` (V2 pair price), `/watch` (event-log monitor), `/wallet`, `/whale` (tx interpreter), `/decode-tx`, `/gas-forecast`, `/arb-scan` (two-pool spread math), `/bridge-opt`, `/base-telemetry` |
| **Docs & finance** | `/sec-parse` (EDGAR 8-K stream), `/kpi` (earnings â†’ JSON), `/kpi-charts`, `/prices` (competitor pricing pages), `/repo-sum`, `/pr-review`, `/pr-docs`, `/npm-pulse` |
| **Agent plumbing** | `/agent-ingest` (schema auto-detect), `/state-token` (resumable multi-agent state), `/token-meter`, `/retry-budget` (spend guard), `/api-key-vault` (stateless HMAC), `/manifest-validate`, `/manifest-lint`, `/loop-check`, `/regcheck`, `/mcp-shim` (OpenAPI â†’ MCP) |
| **Reliability & receipts** | `/receipt-verify` (decode any X-PAYMENT-RESPONSE), `/receipt-batch`, `/api-health`, `/cron-webhook` |
| **Text & reasoning utilities** | `/summarize`, `/sanitize` (prompt-injection scanner), `/diff` (semantic diff), `/nl-sql`, `/risk-grade` (contract clause grading), `/audit-claims` (hallucination audit), `/compact` (SOPâ†’JSON), `/abi-gen`, `/localize`, `/map-schema` |

Full list with per-tool pricing: see the [live manifest](https://relay.imagiolabs.com).

## Quickstart

**MCP (Claude / Cursor / any MCP client):**

```json
{
  "mcpServers": {
    "imagio-relay": {
      "type": "url",
      "url": "https://relay.imagiolabs.com/mcp"
    }
  }
}
```

`initialize` and `tools/list` are **free**. A `tools/call` returns `402` with payment terms; sign the X-PAYMENT header and retry â€” the facilitator settles on-chain and the tool executes.

**Plain HTTP (any x402 client):** call any endpoint; the `402` response carries the `accepts` object (scheme `exact`, network Base, USDC). Sign per the x402 spec, retry with `X-PAYMENT`.

## Payments

- **Asset:** USDC on Base (`eip155:8453`)
- **Facilitator:** [PayAI](https://facilitator.payai.network) (settlement gas covered by PayAI)
- **Settlement identity:** the live `pay_to` is published in the [manifest](https://relay.imagiolabs.com) â€” we never hold custody of in-flight funds; payments settle straight to the payout wallet
- **Escrow agreements:** also available for multi-step jobs via [Kite Passport](https://agentpassport.ai) (`did:kite:auto-g2qw74:imagio-relay`)

## Discovery status (honest)

| Surface | Status |
|---|---|
| Official MCP Registry | âœ… published (`com.imagiolabs.www/imagio-relay`) |
| x402-list.com | â³ submitted, in review |
| PayAI Bazaar catalog | â³ auto-lists a resource **after its first settled call** â€” the first real caller seeds the listing |
| Kite Passport catalog | â³ host-allowlist requested |

## License

MIT for the reference client in `examples/`. The relay itself is a hosted service (see manifest for terms).
