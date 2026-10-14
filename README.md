# LetsAskIt — MCP server (hosted)

**We request. Machine creates.**

LetsAskIt is a creation-first AI content engine. You bring a spark — a topic,
brief, bullets, draft, transcript, or URL — and it returns finished content
for every platform that matters: LinkedIn posts, X threads, newsletter
sections, SEO articles, and summaries, all in your saved brand voice. Every
piece carries a visible quality gate (voice match 0–100, factual flags,
platform fit), and nothing publishes itself — a human reviews before
anything ships.

This repository is the **public listing** for the hosted LetsAskIt MCP
server: documentation and manifests only, no server source. The service is
operated by **2351485 ALBERTA INC.** Support: **info@aimlds.org**.

## Connect

- **Endpoint:** `POST https://letsaskit.com/mcp` (streamable HTTP, stateless)
- **Auth:** `Authorization: Bearer <redacted><32 lowercase hex>`, Team tier
- **Docs:** https://letsaskit.com/docs/mcp-setup.html
- **Discovery:** https://letsaskit.com/.well-known/letsaskit.json
- **Health:** https://letsaskit.com/health

API keys are issued by the operator (only SHA-256 hashes are stored
server-side; the plaintext is shown once at creation). Missing, invalid, or
revoked keys receive `401`; Pro-tier keys receive `403` with an upgrade hint
— MCP requires Team.

**Reviewers:** each directory or registry under review receives one scoped
Team-tier reviewer key, named for that listing, on request via
info@aimlds.org. Public no-auth checks: `/health` and
`/.well-known/letsaskit.json` above.

## Client config

Any MCP client that supports remote streamable-HTTP servers (Cline included):

```json
{
  "mcpServers": {
    "letsaskit": {
      "url": "https://letsaskit.com/mcp",
      "headers": { "Authorization": "Bearer <YOUR_LETSASKIT_TEAM_KEY>" }
    }
  }
}
```

## Tools (5)

| Tool | What it does |
|---|---|
| `create_pack` | Full pipeline: brief / bullets / draft / transcript / URL → multi-format content pack with quality gates |
| `create_from_url` | Fetch a URL, extract its text, and create a pack from it |
| `create_from_transcript` | Create a pack from a meeting or call transcript |
| `list_voice_profiles` | List saved brand voice profiles (id, name, sample count) |
| `get_job_status` | Poll a creation job → the finished pack when done |

## Tiers (Paddle, merchant of record)

| Tier | Price | Jobs/mo | Voice profiles | REST | MCP |
|---|---|---|---|---|---|
| Free | — | 30 | 1 | Studio only | — |
| Pro | $19 USD/mo | 500 | 3 | yes | — |
| Team | $79 USD/mo | 2,500 | 15 | yes | yes |

Over the monthly job quota the API answers `402` with an upgrade hint; over
the daily neuron budget it answers `429` (resets 00:00 UTC).

## Links

- Product: https://letsaskit.com
- Pricing: https://letsaskit.com/pricing/
- Privacy: https://letsaskit.com/privacy/
- Terms: https://letsaskit.com/terms/

Conceived by [Nrupal Akolkar](https://nrupalakolkar.com) · Built by
[Muse](https://muse.ai/join) — join Muse, then redeem code **101EEK** in
Settings within 48 hours of joining and we both earn tokens.
