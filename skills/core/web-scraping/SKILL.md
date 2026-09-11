---
name: web-scraping
description: "Scrape, search, and map the web for agents — clean markdown, structured JSON, and site URL discovery."
version: 2.0.0
author: superagnt
license: MIT-0
platforms: [macos, linux, windows]
required_environment_variables:
  - SUPERAGNT_API_KEY
metadata:
  hermes:
    tags: [web-scraping, crawling, search, data-extraction]
  openclaw:
    requires:
      env:
        - SUPERAGNT_API_KEY
      bins:
        - curl
    primaryEnv: SUPERAGNT_API_KEY
    emoji: "🌐"
    homepage: https://superagnt.com/r/ch-web-scraping-docs
---

# Web Scraping

The superagnt Web API gives agents reliable access to the live web through a single integration. Fetch the content of any public page as clean main-content markdown or structured JSON, run a web search and pull the full content of each result in one call, and discover every URL on a site before scraping it. JavaScript-rendered pages are handled for you. Instead of managing scraping infrastructure, proxies, and rate limits yourself, you call superagnt with one credential and consume agent-ready JSON with predictable, usage-based credit pricing.

## Best install: connect the MCP server

If this client speaks MCP, connect the scoped server instead of using this
skill's curl calls — native tools, structured parameters, OAuth sign-in, and a
tool surface that can grow on demand:

```
https://mcp.superagnt.com/mcp/web-scraping
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

Get an API key from the [dashboard](https://superagnt.com/r/ch-web-scraping-key) and export it as
`SUPERAGNT_API_KEY`. Every request sends it as a Bearer token:

```
Authorization: Bearer $SUPERAGNT_API_KEY
```

If the variable is not set, ask the user for their key or point them at
https://superagnt.com/r/ch-web-scraping-key.

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
https://api.superagnt.com/v1/data/web
```

## Available Endpoints

| Method | Path | Summary |
|--------|------|---------|
| `POST` | `/scrape` | Scrape a web page |
| `POST` | `/search` | Search the web |
| `POST` | `/map` | Map a website&#x27;s URLs |

## Tool Schemas

The following JSON defines every operation with its parameters. Each tool maps
to an API endpoint under the base URL.

```json
[
  {
    "name": "superagnt_web_scrape",
    "description": "Scrape a web page",
    "method": "POST",
    "path": "/scrape",
    "parameters": {
      "type": "object",
      "properties": {
        "url": {
          "type": "string",
          "description": "The URL to scrape (must be a publicly reachable http(s) URL)."
        },
        "formats": {
          "type": "array",
          "description": "Output formats to return. Each entry is either a string preset or an object form. String presets: 'markdown' (default), 'summary', 'html', 'rawHtml', 'links', 'images'. Object forms include { type: 'json', schema, prompt } for structured extraction, { type: 'screenshot', fullPage, quality } for an image, and { type: 'changeTracking', modes } for diffs. Defaults to ['markdown'] when omitted."
        },
        "onlyMainContent": {
          "type": "boolean",
          "description": "Return only the main content of the page, excluding navs, headers, and footers."
        },
        "includeTags": {
          "type": "array",
          "description": "Only include content within these HTML tags / CSS selectors."
        },
        "excludeTags": {
          "type": "array",
          "description": "Exclude content within these HTML tags / CSS selectors."
        },
        "maxAge": {
          "type": "integer",
          "description": "Return cached content up to this age in milliseconds (faster, cheaper). Default 172800000 (2 days)."
        },
        "waitFor": {
          "type": "integer",
          "description": "Milliseconds to wait for the page to render before scraping."
        },
        "mobile": {
          "type": "boolean",
          "description": "Emulate a mobile device."
        },
        "timeout": {
          "type": "integer",
          "description": "Request timeout in milliseconds (1000-300000). Default 60000."
        },
        "parsers": {
          "type": "array",
          "description": "PDF parsing configuration."
        },
        "proxy": {
          "type": "string",
          "description": "Proxy / anti-bot mode. 'auto' (default) escalates only on failure; 'enhanced' forces advanced anti-bot handling and costs more."
        },
        "location": {
          "type": "object",
          "description": "Geographic proxy location and language settings."
        }
      },
      "required": [
        "url"
      ]
    }
  },
  {
    "name": "superagnt_web_search",
    "description": "Search the web",
    "method": "POST",
    "path": "/search",
    "parameters": {
      "type": "object",
      "properties": {
        "query": {
          "type": "string",
          "description": "The search query."
        },
        "limit": {
          "type": "integer",
          "description": "Number of results per source. Default 10."
        },
        "sources": {
          "type": "array",
          "description": "Which result sources to query. Defaults to ['web']. Each entry is a preset string or an object with a `type`."
        },
        "categories": {
          "type": "array",
          "description": "Restrict results to these categories."
        },
        "tbs": {
          "type": "string",
          "description": "Time-based search filter (e.g. 'qdr:d' for past day)."
        },
        "location": {
          "type": "string",
          "description": "Geo-target for the search (e.g. 'San Francisco, California, United States')."
        },
        "country": {
          "type": "string",
          "description": "ISO country code. Default 'US'."
        },
        "includeDomains": {
          "type": "array",
          "description": "Only return results from these hostnames (mutually exclusive with excludeDomains)."
        },
        "excludeDomains": {
          "type": "array",
          "description": "Exclude results from these hostnames (mutually exclusive with includeDomains)."
        },
        "timeout": {
          "type": "integer",
          "description": "Request timeout in milliseconds. Default 60000."
        },
        "scrapeOptions": {
          "type": "object",
          "description": "When provided, each result is also scraped and its content returned. Accepts the same content options as /scrape (e.g. `formats`, `onlyMainContent`). Increases cost because every result is fetched."
        }
      },
      "required": [
        "query"
      ]
    }
  },
  {
    "name": "superagnt_web_map",
    "description": "Map a website's URLs",
    "method": "POST",
    "path": "/map",
    "parameters": {
      "type": "object",
      "properties": {
        "url": {
          "type": "string",
          "description": "The base URL of the site to map."
        },
        "search": {
          "type": "string",
          "description": "Order returned URLs by relevance to this query."
        },
        "sitemap": {
          "type": "string",
          "description": "How to use the site's sitemap. Default 'include'."
        },
        "includeSubdomains": {
          "type": "boolean",
          "description": "Include URLs on subdomains."
        },
        "ignoreQueryParameters": {
          "type": "boolean",
          "description": "Treat URLs that differ only by query string as the same URL."
        },
        "limit": {
          "type": "integer",
          "description": "Maximum number of URLs to return. Default 5000, max 100000."
        },
        "timeout": {
          "type": "integer",
          "description": "Request timeout in milliseconds."
        },
        "location": {
          "type": "object",
          "description": "Geographic proxy location and language settings."
        }
      },
      "required": [
        "url"
      ]
    }
  }
]
```

## Example

```bash
curl -X POST &#x27;https://api.superagnt.com/v1/data/web/scrape&#x27; \
  -H &#x27;Authorization: Bearer your_api_key_here&#x27; \
  -H &#x27;Content-Type: application/json&#x27; \
  -d &#x27;{&quot;url&quot;: &quot;https://example.com&quot;, &quot;formats&quot;: [&quot;markdown&quot;]}&#x27;
```

## Use Cases

- Research and deep-research agents that read live web pages
- RAG pipelines that ingest fresh page content on demand
- Competitive and market monitoring agents
- Lead and company enrichment from public web pages
- Content extraction into structured JSON for downstream tools

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

- [Documentation](https://superagnt.com/r/ch-web-scraping-docs)
- [Dashboard / API keys](https://superagnt.com/r/ch-web-scraping-key)
- [This listing](https://clawhub.ai/superagnt/skills/web-scraping)
