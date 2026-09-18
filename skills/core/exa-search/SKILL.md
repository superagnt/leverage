---
name: exa-search
description: "Neural web search, parsed page contents, find-similar, and agentic Q&amp;A over the live web through the superagnt unified API."
version: 2.0.2
author: superagnt
license: MIT-0
platforms: [macos, linux, windows]
required_environment_variables:
  - SUPERAGNT_API_KEY
metadata:
  hermes:
    tags: [exa, integration, api, ai-agent]
  openclaw:
    requires:
      env:
        - SUPERAGNT_API_KEY
      bins:
        - curl
    primaryEnv: SUPERAGNT_API_KEY
    emoji: "🔎"
    homepage: https://superagnt.com/r/ch-exa-search-docs
---

# Exa Integration

The Exa integration connects your Exa AI account to superagnt so AI agents and automations can query the live web with neural + keyword search, pull parsed page content (text, highlights, summaries), discover semantically similar URLs, and run agentic Q&amp;A with cited sources. Supports the full set of Exa endpoints — /search, /contents, /findSimilar, and /answer — proxied through your superagnt API key.

## Best install: connect the MCP server

If this client speaks MCP, connect the workspace server instead of using this
skill's curl calls — once the Exa account is connected in the
dashboard, its tools appear on the server automatically as native MCP tools:

```
https://mcp.superagnt.com/mcp
```

The URL publishes full OAuth discovery — an MCP-capable client needs the URL
and nothing else (approve once in the browser). Per-client setup lines:
https://mcp.superagnt.com/agent-setup/prompt.md

Everything below works on curl-only environments with just the API key.

## Prerequisites

1. An API key — get one from the [dashboard](https://superagnt.com/r/ch-exa-search-key) and export it as
   `SUPERAGNT_API_KEY`.
2. A connected Exa account — connect it in the
   [connections dashboard](https://app.superagnt.com/dashboard/connections). Calls fail with a clear error
   until the vendor is connected; that error is the signal to send the user to
   the connections page, not a bug.

Note: `/v1/connections/*` calls run on the USER'S Exa credentials —
vendor-side rate limits and billing are theirs, not platform credits.

## Authentication

```
Authorization: Bearer $SUPERAGNT_API_KEY
```

If the variable is not set, ask the user for their key or point them at
https://superagnt.com/r/ch-exa-search-key.

## Verify the install (do this first)

```bash
curl -s https://api.superagnt.com/v1/credits \
  -H &#x27;Authorization: Bearer $SUPERAGNT_API_KEY&#x27;
```

A JSON result proves the key end to end. 401 = bad key; an error naming the
connection means the Exa account is not connected yet.

## Base URL

```
https://api.superagnt.com/v1/connections/exa
```

## Available Endpoints

| Method | Path | Summary |
|--------|------|---------|
| `POST` | `/search` | Search |
| `POST` | `/contents` | Get contents |
| `POST` | `/findSimilar` | Find similar links |
| `POST` | `/answer` | Generate an answer from search results |

## Tool Schemas

The following JSON defines every operation with its parameters. Each tool maps
to an API endpoint under the base URL.

```json
[
  {
    "name": "superagnt_connection_exa_search",
    "description": "Search",
    "method": "POST",
    "path": "/search",
    "parameters": {
      "type": "object",
      "properties": {
        "query": {
          "type": "string",
          "description": "The query string for the search."
        },
        "additionalQueries": {
          "type": "array",
          "description": "Additional query variations for deep-search variants. Used alongside the main query for more comprehensive results."
        },
        "stream": {
          "type": "boolean",
          "description": "If true, the response is returned as an SSE stream of OpenAI-compatible chat completion chunks."
        },
        "outputSchema": {
          "type": "object",
          "description": "JSON schema for synthesized output. When provided, the response includes an output object and output.content matches this schema."
        },
        "systemPrompt": {
          "type": "string",
          "description": "Instructions that guide synthesized output and, for deep-search variants, search planning."
        },
        "type": {
          "type": "string",
          "description": "Search strategy. `auto` intelligently combines neural and other search methods; `fast` uses streamlined models; `deep-lite` / `deep` / `deep-reasoning` are synthesized-output modes; `instant` is the lowest-latency mode."
        },
        "category": {
          "type": "string",
          "description": "A data category to focus on. `people` and `company` support a limited filter set — `startPublishedDate`, `endPublishedDate`, `startCrawlDate`, `endCrawlDate`, and `excludeDomains` are not allowed; `includeDomains` for `people` only accepts LinkedIn domains."
        },
        "userLocation": {
          "type": "string",
          "description": "Two-letter ISO country code of the user (e.g. `US`)."
        },
        "numResults": {
          "type": "integer",
          "description": "Number of results to return. Max 100 for `neural` and deep-search variants."
        },
        "includeDomains": {
          "type": "array",
          "description": "Restrict results to these domains."
        },
        "excludeDomains": {
          "type": "array",
          "description": "Exclude these domains from results."
        },
        "startCrawlDate": {
          "type": "string",
          "description": "Only include links crawled after this ISO 8601 date."
        },
        "endCrawlDate": {
          "type": "string",
          "description": "Only include links crawled before this ISO 8601 date."
        },
        "startPublishedDate": {
          "type": "string",
          "description": "Only include links with a published date after this ISO 8601 date."
        },
        "endPublishedDate": {
          "type": "string",
          "description": "Only include links with a published date before this ISO 8601 date."
        },
        "moderation": {
          "type": "boolean",
          "description": "Enable content moderation to filter unsafe content from results."
        },
        "contents": {
          "type": "object",
          "description": "Content enrichment options returned alongside search results. See /contents for the full schema."
        }
      },
      "required": [
        "query"
      ]
    }
  },
  {
    "name": "superagnt_connection_exa_getContents",
    "description": "Get contents",
    "method": "POST",
    "path": "/contents",
    "parameters": {
      "type": "object",
      "properties": {
        "urls": {
          "type": "array",
          "description": "URLs to crawl (backwards compatible with `ids`)."
        },
        "ids": {
          "type": "array",
          "description": "Document IDs obtained from previous searches."
        },
        "text": {
          "type": "string",
          "description": "Boolean or advanced text options object. Object form supports `maxCharacters`, `includeHtmlTags`, `verbosity` (`compact` | `standard` | `full`), `includeSections`, `excludeSections` (header | navigation | banner | body | sidebar | footer | metadata)."
        },
        "highlights": {
          "type": "string",
          "description": "Boolean or advanced highlights object. Object form supports `maxCharacters`, `query`."
        },
        "summary": {
          "type": "object",
          "description": "LLM-generated summary options."
        },
        "livecrawl": {
          "type": "string",
          "description": "Deprecated — use `maxAgeHours`."
        },
        "livecrawlTimeout": {
          "type": "integer",
          "description": "livecrawlTimeout"
        },
        "maxAgeHours": {
          "type": "integer",
          "description": "Max age of cached content in hours. Positive = livecrawl if older, 0 = always livecrawl, -1 = never livecrawl."
        },
        "subpages": {
          "type": "integer",
          "description": "subpages"
        },
        "subpageTarget": {
          "type": "string",
          "description": "String or array of strings."
        },
        "extras": {
          "type": "object",
          "description": "extras"
        }
      },
      "required": [
        "urls"
      ]
    }
  },
  {
    "name": "superagnt_connection_exa_findSimilar",
    "description": "Find similar links",
    "method": "POST",
    "path": "/findSimilar",
    "parameters": {
      "type": "object",
      "properties": {
        "url": {
          "type": "string",
          "description": "The URL to find similar links for."
        },
        "numResults": {
          "type": "integer",
          "description": "numResults"
        },
        "includeDomains": {
          "type": "array",
          "description": "includeDomains"
        },
        "excludeDomains": {
          "type": "array",
          "description": "excludeDomains"
        },
        "startCrawlDate": {
          "type": "string",
          "description": "startCrawlDate"
        },
        "endCrawlDate": {
          "type": "string",
          "description": "endCrawlDate"
        },
        "startPublishedDate": {
          "type": "string",
          "description": "startPublishedDate"
        },
        "endPublishedDate": {
          "type": "string",
          "description": "endPublishedDate"
        },
        "moderation": {
          "type": "boolean",
          "description": "moderation"
        },
        "contents": {
          "type": "object",
          "description": "Content enrichment options. Same shape as in /search and /contents."
        }
      },
      "required": [
        "url"
      ]
    }
  },
  {
    "name": "superagnt_connection_exa_answer",
    "description": "Generate an answer from search results",
    "method": "POST",
    "path": "/answer",
    "parameters": {
      "type": "object",
      "properties": {
        "query": {
          "type": "string",
          "description": "The question or query to answer."
        },
        "stream": {
          "type": "boolean",
          "description": "If true, returns an SSE stream."
        },
        "text": {
          "type": "boolean",
          "description": "If true, citations include full text content of each source."
        },
        "outputSchema": {
          "type": "object",
          "description": "JSON Schema Draft 7 for structured answers."
        }
      },
      "required": [
        "query"
      ]
    }
  }
]
```

## Example

```bash
curl -X POST &#x27;https://api.superagnt.com/v1/connections/exa/search&#x27; \
  -H &#x27;Authorization: Bearer $SUPERAGNT_API_KEY&#x27; \
  -H &#x27;Content-Type: application/json&#x27; \
  -d &#x27;{&quot;query&quot;:&quot;latest developments in LLMs&quot;,&quot;numResults&quot;:10}&#x27;
```

## Use Cases

- Give agents real-time web knowledge beyond their training cutoff
- Research a company, competitor, or topic with cited sources and summaries
- Build retrieval-augmented generation pipelines grounded in the live web
- Pull clean page text for summarization, extraction, or ingest into a vector store
- Monitor news and blog posts filtered to trusted domains and date ranges
- Run agentic Q&amp;A that returns answers plus the sources Exa used

## Links

- [Documentation](https://superagnt.com/r/ch-exa-search-docs)
- [Dashboard / API keys](https://superagnt.com/r/ch-exa-search-key)
- [Connections dashboard](https://app.superagnt.com/dashboard/connections)
- [This listing](https://clawhub.ai/superagnt/skills/exa-search)
