---
name: lead-enrichment
description: "Enrich people and companies from a name, domain, or LinkedIn URL — emails, titles, firmographics, and mobile numbers for agents."
version: 2.0.2
author: superagnt
license: MIT-0
platforms: [macos, linux, windows]
required_environment_variables:
  - SUPERAGNT_API_KEY
metadata:
  hermes:
    tags: [lead-enrichment, sales, prospecting, b2b-data]
  openclaw:
    requires:
      env:
        - SUPERAGNT_API_KEY
      bins:
        - curl
    primaryEnv: SUPERAGNT_API_KEY
    emoji: "🎯"
    homepage: https://superagnt.com/r/ch-lead-enrichment-docs
---

# Lead Enrichment

Turn a name, a domain, or a LinkedIn URL into a complete lead record in one call. The people endpoints resolve work emails, titles, seniority, location, and mobile numbers; the company endpoints add firmographics and context, and bulk variants process whole lists at once. Lookups run as an orchestrated waterfall across multiple data providers server-side, so the agent makes one call and gets the best available answer — no per-vendor accounts, no fallback logic to write. Built for sales agents that research before outreach, CRM-hygiene agents that fill gaps automatically, and prospecting pipelines that need reliable person-level data at scale.

## Best install: connect the MCP server

If this client speaks MCP, connect the scoped server instead of using this
skill's curl calls — native tools, structured parameters, OAuth sign-in, and a
tool surface that can grow on demand:

```
https://mcp.superagnt.com/mcp/lead-enrichment
```

That URL publishes full OAuth discovery: an MCP-capable client needs the URL
and nothing else (approve once in the browser). On clients that hold a bearer
instead, add it as an `Authorization: Bearer` header. Per-client setup lines:
https://mcp.superagnt.com/agent-setup/prompt.md

Two clients refresh tools live when the server grows (`agnt_tools_enable`):
Hermes and OpenClaw. Most others hold the tool list until reconnect — on a
cloud connector (claude.ai, ChatGPT) refresh the connector in its settings,
on a direct config start a new session.

This skill document stays fully usable on curl-only environments — everything
below works with just the API key.

## Authentication

Get an API key from the [dashboard](https://superagnt.com/r/ch-lead-enrichment-key) and export it as
`SUPERAGNT_API_KEY`. Every request sends it as a Bearer token:

```
Authorization: Bearer $SUPERAGNT_API_KEY
```

If the variable is not set, ask the user for their key or point them at
https://superagnt.com/r/ch-lead-enrichment-key.

## Verify the install (do this first)

One call proves the key, the credit balance, and this source end to end:

```bash
curl -s https://api.superagnt.com/v1/credits \
  -H &#x27;Authorization: Bearer $SUPERAGNT_API_KEY&#x27;
```

A JSON result means you are live. A 401 means the key is wrong or missing; a
402 means the workspace is out of credits — the dashboard shows both.

## Base URL

```
https://api.superagnt.com/v1/data/agnt
```

## Available Endpoints

| Method | Path | Summary |
|--------|------|---------|
| `POST` | `/people/enrich` | Enrich a person |
| `POST` | `/people/bulk-enrich` | Bulk enrich people |
| `POST` | `/people/search` | Search people |
| `POST` | `/people/find-mobile` | Find a mobile phone number |
| `POST` | `/people/email-finder` | Find a person&#x27;s professional email |
| `POST` | `/companies/enrich` | Enrich a company |

## Tool Schemas

The following JSON defines every operation with its parameters. Each tool maps
to an API endpoint under the base URL.

```json
[
  {
    "name": "superagnt_agnt_people_enrich",
    "description": "Enrich a person",
    "method": "POST",
    "path": "/people/enrich",
    "parameters": {
      "$ref": "#/components/schemas/PeopleEnrichRequest"
    }
  },
  {
    "name": "superagnt_agnt_people_bulk_enrich",
    "description": "Bulk enrich people",
    "method": "POST",
    "path": "/people/bulk-enrich",
    "parameters": {
      "$ref": "#/components/schemas/PeopleBulkEnrichRequest"
    }
  },
  {
    "name": "superagnt_agnt_people_search",
    "description": "Search people",
    "method": "POST",
    "path": "/people/search",
    "parameters": {
      "$ref": "#/components/schemas/PeopleSearchRequest"
    }
  },
  {
    "name": "superagnt_agnt_people_find_mobile",
    "description": "Find a mobile phone number",
    "method": "POST",
    "path": "/people/find-mobile",
    "parameters": {
      "$ref": "#/components/schemas/PeopleFindMobileRequest"
    }
  },
  {
    "name": "superagnt_agnt_people_email_finder",
    "description": "Find a person's professional email",
    "method": "POST",
    "path": "/people/email-finder",
    "parameters": {
      "$ref": "#/components/schemas/PeopleEmailFinderRequest"
    }
  },
  {
    "name": "superagnt_agnt_companies_enrich",
    "description": "Enrich a company",
    "method": "POST",
    "path": "/companies/enrich",
    "parameters": {
      "$ref": "#/components/schemas/CompaniesEnrichRequest"
    }
  }
]
```

## Example

```bash
curl -X POST &#x27;https://api.superagnt.com/v1/data/agnt/people/enrich&#x27; \
  -H &#x27;Authorization: Bearer $SUPERAGNT_API_KEY&#x27; \
  -H &#x27;Content-Type: application/json&#x27; \
  -d &#x27;{&quot;...&quot;: &quot;see tool schemas below&quot;}&#x27;
```

## Use Cases


## Growing beyond this source

The same key and credit balance cover every superagnt data source and, on the
MCP server, the full platform (workspace database, files, webhooks, queues,
first-party people/company enrichment). Over MCP, discover what is available
with `agnt_tools_search` and turn a family on with `agnt_tools_enable` — money
is never charged without a human confirming in the dashboard.

```bash
curl https://api.superagnt.com/v1/platforms
```

## Links

- [Documentation](https://superagnt.com/r/ch-lead-enrichment-docs)
- [Dashboard / API keys](https://superagnt.com/r/ch-lead-enrichment-key)
- [This listing](https://clawhub.ai/superagnt/skills/lead-enrichment)
