---
name: email-finder
description: "Find and verify work emails from a name and company — waterfall lookup across providers, one call for agents."
version: 2.0.2
author: superagnt
license: MIT-0
platforms: [macos, linux, windows]
required_environment_variables:
  - SUPERAGNT_API_KEY
metadata:
  hermes:
    tags: [email-finder, email-verification, outbound, sales]
  openclaw:
    requires:
      env:
        - SUPERAGNT_API_KEY
      bins:
        - curl
    primaryEnv: SUPERAGNT_API_KEY
    emoji: "🎯"
    homepage: https://superagnt.com/r/ch-email-finder-docs
---

# Email Finder

Find a person’s work email from their name and company, verify any address before sending, or list the known addresses on a domain — three endpoints, one key. Lookups run as an orchestrated waterfall across multiple email data providers server-side: the agent makes one call and gets the best verified answer, with deliverability status attached. Built for outbound agents that need send-ready addresses, list-cleaning jobs that verify before a campaign, and enrichment flows that finish a lead record with a working email.

## Best install: connect the MCP server

If this client speaks MCP, connect the scoped server instead of using this
skill's curl calls — native tools, structured parameters, OAuth sign-in, and a
tool surface that can grow on demand:

```
https://mcp.superagnt.com/mcp/email-finder
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

Get an API key from the [dashboard](https://superagnt.com/r/ch-email-finder-key) and export it as
`SUPERAGNT_API_KEY`. Every request sends it as a Bearer token:

```
Authorization: Bearer $SUPERAGNT_API_KEY
```

If the variable is not set, ask the user for their key or point them at
https://superagnt.com/r/ch-email-finder-key.

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
| `POST` | `/people/email-finder` | Find a person&#x27;s professional email |
| `POST` | `/people/email-verifier` | Verify a professional email |
| `POST` | `/companies/domain-emails` | List a domain&#x27;s emails |

## Tool Schemas

The following JSON defines every operation with its parameters. Each tool maps
to an API endpoint under the base URL.

```json
[
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
    "name": "superagnt_agnt_people_email_verifier",
    "description": "Verify a professional email",
    "method": "POST",
    "path": "/people/email-verifier",
    "parameters": {
      "$ref": "#/components/schemas/PeopleEmailVerifierRequest"
    }
  },
  {
    "name": "superagnt_agnt_companies_domain_emails",
    "description": "List a domain's emails",
    "method": "POST",
    "path": "/companies/domain-emails",
    "parameters": {
      "$ref": "#/components/schemas/CompaniesDomainEmailsRequest"
    }
  }
]
```

## Example

```bash
curl -X POST &#x27;https://api.superagnt.com/v1/data/agnt/people/email-finder&#x27; \
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

- [Documentation](https://superagnt.com/r/ch-email-finder-docs)
- [Dashboard / API keys](https://superagnt.com/r/ch-email-finder-key)
- [This listing](https://clawhub.ai/superagnt/skills/email-finder)
