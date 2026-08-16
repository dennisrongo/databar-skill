# Databar MCP Skill

A [Hermes Agent](https://hermes-agent.nousresearch.com/) skill for driving [Databar](https://databar.ai/) through its official MCP server — lead enrichment, waterfalls, scraping, company intelligence, and table pipelines, with cost discipline built in.

Databar connects 160+ data providers (email finders, verifiers, Google Maps, job postings, tech-stack lookups, LinkedIn data, SERP, YouTube) behind one API. This skill teaches an agent the correct workflow: **discover → price → run → poll → persist → export**, so it never spends credits blind.

## What's inside

```
SKILL.md               # Workflow, tool map, cost discipline, pitfalls
references/recipes.md  # 6 recipes: lead lists, Maps scraping, hiring signals,
                       # tech stacks, scheduled sources, YouTube/SERP pulls
```

## Requirements

- A [Databar](https://databar.ai/registration) account with credits (free tier available)
- The Databar MCP server connected to your agent. Hosted endpoint (recommended): `https://mcp.databar.ai/mcp` — API key from your Databar workspace → Integrations, sent as `Authorization: Bearer <key>`. Full setup: [docs.databar.ai/mcp-server](https://docs.databar.ai/mcp-server)

### Hermes Agent

```bash
hermes mcp add databar --url https://mcp.databar.ai/mcp --auth header
# paste your API key when prompted, then:
hermes skills install dennisrongo/databar-skill
```

### Claude / Cursor / other MCP clients

This is a standard SKILL.md-format skill. Point your client's skill loader at this repo, or copy `SKILL.md` + `references/` into your skills directory. The MCP server config for Claude/Cursor is in the [Databar MCP docs](https://docs.databar.ai/mcp-server).

## What the skill does

- **Discovery first**: `search_enrichments` → `get_enrichment_details` → exact params and `price_credits` before any spend
- **Waterfalls**: multi-provider email/person lookups that stop at first hit, optional built-in verification
- **Tables as pipelines**: attach enrichments as result columns, `run_empty` strategy to skip already-filled rows, scheduled sources, CRM exporters
- **Cost discipline**: balance check before/after, price-the-chain rule, no blind bulk runs
- **Verified pitfalls**: async task polling, position-aligned bulk joins, permanent deletions, BYOK auth modes, cache vs fresh pricing

## Example prompts

- "Check my Databar balance and find me the cheapest email finder for this list of 50 domains"
- "Build a table of Sacramento dentists from Google Maps, then find verified emails for the owners"
- "Which companies hiring data entry clerks also run Zapier? Give me contacts"
- "How many credits would it cost to enrich these 200 domains with company data?"

## Disclaimer

Enrichment IDs and prices in `references/recipes.md` were verified August 2026 and will drift — the skill itself re-verifies via `search_enrichments` on every use, which is the point. Data-provider terms and privacy laws (CAN-SPAM, GDPR) apply to how you use contact data; that's on you.

## License

MIT
